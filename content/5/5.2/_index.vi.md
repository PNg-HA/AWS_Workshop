---
title : "Set up a weekly vulnerability summary email"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: Bạn đã được giao nhiệm vụ thiết lập một email tóm tắt hàng tuần để nêu bật những thông tin chính dựa trên dữ liệu mà bạn đang tổng hợp trong **Security Hub**. Ban đầu, email cần nêu bật tình hình bảo mật hiện tại của bạn, các lỗ hổng bảo mật ảnh hưởng đến nhiều instances và images nhất, và các phát hiện có mức độ ưu tiên cao nhất.
{{%/notice%}}

{{%notice warning%}}
**Lưu ý cho Người Tham Gia**: Bạn phải hoàn thành module có tên "Sử dụng insights để ưu tiên và đo lường" trước khi bắt đầu module này.
{{%/notice%}}

Trong kịch bản này, bạn sẽ viết một **Lambda function** để gọi **Security Hub** và **Amazon Inspector APIs** để lấy các chi tiết cần thiết. Bạn sẽ sử dụng **EventBridge** để gọi **Lambda** hàng tuần, và bạn sẽ đăng ký nhận email bằng cách sử dụng **AWS Simple Notification Service**.

**Amazon Inspector** là một dịch vụ quản lý lỗ hổng tự động liên tục quét các khối lượng công việc **AWS** để tìm các lỗ hổng phần mềm và phơi nhiễm mạng không mong muốn. Các phát hiện về lỗ hổng từ **Inspector** được bao gồm trong email tóm tắt để cung cấp một cái nhìn toàn diện hơn về các lỗ hổng có thể tồn tại trong tài khoản mà email tóm tắt đang được tạo ra.



#### Cấu hình Amazon Simple Notification Service topic
1. Điều hướng đến [Amazon SNS](https://console.aws.amazon.com/sns/v3/home)

2. Nhấp vào **Topics** trong bảng điều hướng bên trái.

3. Nhấp vào **Create topic**.

4. Chọn loại **Standard**.

5. Đặt tên là **weekly-vulnerability-email**. Để nguyên mọi thứ khác và nhấp vào **Create topic** ở cuối trang. Điều này sẽ tạo chủ đề.

6. Ghi chú lại **SNS Topic ARN**. Bạn sẽ cần đến nó sau này. **ARN** trông giống như "arn:aws:sns:us-******:############:weekly-vulnerability-email".

#### Subscribe to the topic

7. Từ trang **weekly-vulnerability-email topic**, nhấp vào **Create subscription**.

8. Trên trang **Create subscription**, dưới **Protocol**, chọn **email**.

9. Trên trang **Create subscription**, dưới **Endpoint**, nhập địa chỉ email mà bạn muốn sử dụng cho workshop này để nhận thông báo. Bạn có thể hủy đăng ký sau khi hoàn thành workshop.

10. Nhấp vào **Create subscription**.

11. Trong vài phút, bạn sẽ nhận được một email tại địa chỉ email bạn đã nhập. Xác nhận đăng ký bằng cách nhấp vào "Confirm subscription" trong email.

#### Create the Lambda function to author the email

12. Đi đến https://console.aws.amazon.com/lambda/home?#/create/function?intent=authorFromScratch 
Trên trang **Create function**, đặt **Function name** là **weekly-vulnerability-email** và chọn **Node.js 20.x** cho **Runtime**.

13. Nhấp vào **Create function**.
Sao chép và dán đoạn mã sau vào trình soạn thảo được gắn nhãn **Code source** (xóa mã hiện có).


```
'use strict';
import {
    SecurityHubClient,
    GetInsightsCommand,
    GetInsightResultsCommand
} from "@aws-sdk/client-securityhub";

import {
    Inspector2Client,
    ListFindingAggregationsCommand
} from "@aws-sdk/client-inspector2";

import {
    SNSClient,
    PublishCommand
} from "@aws-sdk/client-sns";

const securityhubClient = new SecurityHubClient();
const inspectorClient = new Inspector2Client();
const snsClient = new SNSClient();

export const handler = async (event, context) => {
    try {
        let msg = '';
        const getInsights = new GetInsightsCommand({});
        const allCustomInsights = await securityhubClient.send(getInsights);
        const insightsToFetch = [
            ...allCustomInsights.Insights, // include all custom insights
            ...[
                {
                    Name: "Top products by counts of findings",
                    InsightArn: "arn:aws:securityhub:::insight/securityhub/default/28"
                }, // include default insight 28
                {
                    Name: "Severity by counts of findings",
                    InsightArn: "arn:aws:securityhub:::insight/securityhub/default/29"
                } // include default insight 29
            ]
        ];

        const insightsToFetchResults = await Promise.all(insightsToFetch.map(async (o, i) => {
            let getInsightResults = new GetInsightResultsCommand({InsightArn: o.InsightArn});
            let insightResults = await securityhubClient.send(getInsightResults);
            return {...insightResults, Name: o.Name};
        }));
        
        const results = insightsToFetchResults.forEach((o,i) => {
            msg += `------------------------------------\nInsight: ${o.Name}\n------------------------------------\n`;
            o.InsightResults.ResultValues.slice(0,9).forEach((o,i) => {
                msg += `${o.GroupByAttributeValue}: ${o.Count}\n`;
            }); 
            msg += `\nFollow the link below to see all ${o.InsightResults.ResultValues.length} results from this insight in Security Hub:\n`;
            msg += `https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/${o.InsightResults.InsightArn}\n\n`;
        });

        const listFindingAggregationsInput = {
            "aggregationType": "PACKAGE",
            "maxResults": 5,
            "aggregationRequest": {
                "packageAggregation": {
                    "sortOrder": "DESC",
                    "sortBy": "CRITICAL"
                }
            }
        };
        const listFindingAggregations = new ListFindingAggregationsCommand(listFindingAggregationsInput);
        const inspectorFindingAggregations = await inspectorClient.send(listFindingAggregations);
        msg += '\n------------------------------------\nSoftware vulnerabilities impacting the most instances and images:\n------------------------------------\n';
        inspectorFindingAggregations.responses.forEach(pkg => {
            msg += `Package: ${pkg.packageAggregation.packageName}:\nCounts of findings by severity:\n`;
            msg += `Critical: ${pkg.packageAggregation.severityCounts.critical}, `;
            msg += `High: ${pkg.packageAggregation.severityCounts.high}, `;
            msg += `Medium: ${pkg.packageAggregation.severityCounts.medium} \n\n`;
        });

        const emailTopicArn = process.env['emailTopicArn'];
        const messageInput = {
            TopicArn: emailTopicArn,
            Message: msg,
            Subject: "Weekly Vulnerability Report",
        };
        const publish = new PublishCommand(messageInput);
        const publishResponse = await snsClient.send(publish);

        return `Successfully sent message id ${publishResponse.MessageId}`;
    } catch (error) {
        console.error('Error:', error);
        throw error;
    }
}
```
16. Sau đó từ menu **Code source**, nhấp vào **File** rồi **Save**. Sau đó nhấp vào nút **Deploy**.

17. Lưu ý ở dòng 76 của mã **Lambda**, chúng ta tham chiếu đến một biến môi trường nơi chúng ta lưu trữ ARN của **SNS Topic**. Chúng ta cần cấu hình điều đó. Ngay phía trên phần **Code source**, nhấp vào tab **Configuration**. Sau đó, nhấp vào **Environment Variables** từ bảng điều hướng bên trái.

18. Nhấp vào **Edit** dưới phần **Environment Variables**. Sau đó nhấp vào **Add environment variable**.

19. Nhập "**emailTopicArn**" dưới **Key**. Nhập ARN của **SNS Topic** mà bạn đã tạo trong bước 6 làm **Value**. Đảm bảo không có khoảng trắng.

20. Nhấp vào **Save**.

#### Set the permissions of the Lambda function
21. Bây giờ chúng ta cần cấu hình quyền cho **Lambda function** để nó có thể thực thi các lệnh cần thiết để gửi email của chúng ta. Dưới cùng tab **Configuration**, nhấp vào **Permissions** từ bảng điều hướng bên trái. Chúng ta cần sửa đổi vai trò thực thi cho **Lambda function** này.

22. Nhấp vào **Role name**. Tên này sẽ bắt đầu bằng "weekly-vulnerability-email-role-". Điều này sẽ đưa bạn đến vai trò trong **Identity and Access Management (IAM)** console.

23. Từ trang **Role**, nhấp vào nút **Add permissions** và chọn **Create inline policy**.

24. Trên trang **Create policy**, nhấp vào tab **JSON**. Sau đó thay thế chính sách hiện tại bằng chính sách sau. Chính sách này cấp quyền cho **Lambda function** để truy xuất thông tin về **Security Hub insights**, truy xuất các phát hiện về lỗ hổng từ **Inspector**, và khả năng gửi tin nhắn đến **SNS**. Trong workshop này, chúng ta sẽ đơn giản cho phép **Lambda function** của chúng ta gửi tin nhắn đến bất kỳ **SNS Topic** nào, nhưng trong bất kỳ môi trường khác, tốt nhất là giới hạn điều này cho các ARN cụ thể.


```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"securityhub:GetInsightResults",
				"securityhub:GetInsightFindingTrend",
				"inspector2:ListFindingAggregations",
				"sns:Publish",
				"securityhub:GetInsights"
			],
			"Resource": "*"
		}
	]
}
```

25. Nhấp vào nút **Next**.

26. Đặt tên cho chính sách của bạn là *AllowSNSPublishAndInspectorList* và nhấp vào **Create policy**.

#### Set timeout of the Lambda function
27. Bây giờ chúng ta cần cấu hình thời gian chờ của **Lambda function** để nó có đủ thời gian thực thi tất cả các lệnh cần thiết để gửi email của chúng ta. Dưới cùng tab **Configuration**, nhấp vào **General Configuration** từ bảng điều hướng bên trái.

28. Nhấp vào **Edit**.

29. Đặt **Timeout** là **1 min 0 sec**.

30. Nhấp vào **Save**.

#### Test the vulnerability summary email

31. Chúng ta đã sẵn sàng để kiểm tra email tóm tắt lỗ hổng! Quay lại [Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions/weekly-vulnerability-email?tab=code) và mở chức năng mà bạn đã tạo.

32. Bên cạnh phần **Code source**, nhấp vào **Test** dropdown và chọn **Configure test event**. Một cửa sổ Configure test event sẽ mở ra.

33. Dưới **Event name**, nhập "**Generate-Email**". Sau đó nhấp vào **Save**. Chúng ta không cần lo lắng về **Event JSON** vì chúng ta không sử dụng nó trong chức năng của mình. Bạn sẽ thấy một biểu ngữ xuất hiện thông báo "The test event Generate-Email was successfully saved."

34. Nhấp vào **Test** một lần nữa để chạy thử nghiệm bằng cách sử dụng cấu hình của bạn. Bạn sẽ thấy kết quả thực thi dưới phần **Code Source**. Trong vài phút, bạn sẽ nhận được một email với tóm tắt lỗ hổng của mình!

{{%notice warning%}}
**Lưu ý cho Người Tham Gia**: Nếu bạn không nhận được email, hãy kiểm tra kết quả thực thi để tìm lỗi. Đảm bảo rằng ARN của **SNS Topic** mà bạn đã cấu hình biến môi trường kết thúc bằng "weekly-vulnerability-email". Nếu có bất kỳ ký tự nào sau "email", hãy xóa chúng và lưu biến môi trường. Sau đó thử lại.
{{%/notice%}}

#### Schedule the weekly vulnerability summary email with EventBridge
35. Cuối cùng, chúng ta cần tạo một **EventBridge rule** sẽ gọi **Lambda function** mà chúng ta đã tạo hàng tuần. Mở **EventBridge**. https://us-east-1.console.aws.amazon.com/events/home

36. Chọn **EventBridge Schedule** ở bên phải. Nhấp vào **Create schedule**.

37. Trên trang **Specify schedule detail**, đặt tên rule của bạn là *weekly-vulnerability-email*.

38. Dưới **Schedule pattern** chọn "**Recurring schedule**".

39. Để nguyên "**Cron-based schedule**" được chọn.

40. Chúng ta sẽ lên lịch email mỗi thứ Hai lúc 7 giờ sáng CDT. Dưới **Cron expression**, đặt **cron( 0 12 ? * 2 * )**. Chọn **Local time zone** từ danh sách thả xuống. Bạn sẽ thấy một bản xem trước của **Next 10 trigger date(s)**.

41. Dưới "**Flexible time window**", chọn "**Off**". Sau đó nhấp vào **Next**.

42. Trên bước **Select target**, chọn **AWS Lambda - Invoke**. Sau đó chọn "weekly-vulnerability-email" từ danh sách thả xuống **Lambda function**. Nhấp vào **Next**.

43. Trên bước **Settings**, nhấp vào **Next**.

44. Trên bước **Review and create schedule**, nhấp vào **Create schedule**.
