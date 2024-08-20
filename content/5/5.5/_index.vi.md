---
title : "Làm giàu kết quả bảo mật với dữ liệu điều tra"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---
Trong bối cảnh mối đe dọa đang thay đổi nhanh chóng ngày nay, việc hiểu rõ rủi ro và tìm ra nguyên nhân gốc rễ của một vấn đề một cách kịp thời là điều quan trọng đối với các doanh nghiệp. **Amazon Detective** giúp dễ dàng phân tích, điều tra và nhanh chóng xác định nguyên nhân gốc rễ của các vấn đề bảo mật tiềm ẩn hoặc hoạt động đáng ngờ. **Amazon Detective** hỗ trợ khả năng tự động điều tra các thực thể **AWS Identity and Access Management (IAM)** để tìm các chỉ báo về việc bị xâm phạm (**IoC**). Khả năng này giúp các nhà phân tích bảo mật xác định xem các thực thể **IAM** có thể đã bị xâm phạm hoặc tham gia vào bất kỳ chiến thuật, kỹ thuật và quy trình (**TTP**) nào được biết đến từ **MITRE ATT&CK framework**. Bạn có thể sử dụng **Detective Investigation API** một cách lập trình để giúp điều tra các vấn đề bảo mật tiềm ẩn.

**Security Hub** là một công cụ quản lý tư thế bảo mật trên đám mây tự động phát hiện khi các tài khoản và tài nguyên **AWS** của bạn không tuân theo các phương pháp bảo mật tốt nhất, tổng hợp các cảnh báo bảo mật vào một nơi và định dạng duy nhất, và hiểu rõ tư thế bảo mật tổng thể của bạn trên tất cả các tài khoản **AWS** của bạn. **Security Hub** cung cấp cho bạn tùy chọn gửi **findings** đến một giải pháp bảo mật khác, chẳng hạn như **AWS Security Lake** hoặc một giải pháp của **AWS Partner Network (APN)**. Nhiều khách hàng sử dụng **AWS Security Hub** để phát hiện các cấu hình bảo mật sai và xem xét, ưu tiên các **findings** bảo mật. Họ thích cái nhìn đơn giản, tập trung vào **findings** trong **Security Hub** hơn là việc xem xét và ưu tiên các **findings** trong từng dịch vụ và vùng mà chúng xuất phát.

**Amazon GuardDuty** là một dịch vụ phát hiện mối đe dọa thông minh liên tục theo dõi các tài khoản **AWS**, khối lượng công việc, hoạt động runtime và dữ liệu của bạn để tìm hoạt động độc hại. Nếu phát hiện hoạt động độc hại tiềm ẩn, chẳng hạn như hành vi bất thường hoặc rò rỉ thông tin xác thực, **GuardDuty** tạo ra các **findings** bảo mật chi tiết. Khi bạn kích hoạt **GuardDuty** và **Security Hub** trong cùng một tài khoản và cùng một vùng **AWS**, **GuardDuty** sẽ bắt đầu gửi tất cả các **findings** được tạo ra đến **Security Hub**.

Trong module này, bạn sẽ xây dựng một giải pháp tích hợp **Detective** với **Security Hub** giúp bạn có cái nhìn tốt hơn về các chỉ báo mối đe dọa và dữ liệu điều tra trực tiếp từ **Security Hub**. Điều này cho phép bạn thực hiện các cuộc điều tra kịp thời hơn về các **findings** của **GuardDuty** từ một cái nhìn tập trung. Giải pháp sẽ tự động thực hiện làm giàu dữ liệu cho các **findings** có mức độ nghiêm trọng cao và trung bình và cung cấp cho bạn sự linh hoạt để khởi tạo các cuộc điều tra và làm giàu dữ liệu bổ sung theo yêu cầu. Bằng cách làm giàu các **findings** trong **Security Hub**, bạn có tùy chọn xem xét những **findings** đã được làm giàu trực tiếp trong **Security Hub console**, hoặc bạn có thể kích hoạt tích hợp để xem xét các **findings** đã được làm giàu trong giải pháp mà bạn chọn.

#### The solution architecture
**Security Hub** tự động gửi tất cả các **findings** mới và tất cả các bản cập nhật cho các **findings** hiện có đến **EventBridge** dưới dạng các sự kiện **Security Hub Findings - Imported**. Mỗi sự kiện **Security Hub Findings - Imported** chứa một **finding** duy nhất. **Security Hub** cũng gửi các **findings** liên quan đến các **custom actions** đến **EventBridge** dưới dạng các sự kiện **Security Hub Findings - Custom Action**. Như được hiển thị trong sơ đồ kiến trúc dưới đây, bạn sẽ sử dụng **EventBridge Rules** để gọi một **Lambda function** cho mỗi **finding** mà bạn làm giàu. Bạn sẽ sử dụng một **EventBridge Rule** cho các sự kiện **Imported** để tự động khởi tạo **Lambda function** cho các **findings** có mức độ nghiêm trọng cao (và trung bình) được **Security Hub** tổng hợp từ **GuardDuty**. Bạn sẽ sử dụng một **EventBridge Rule** thứ hai cho các sự kiện **Custom Action** để khởi tạo các cuộc điều tra và làm giàu dữ liệu bổ sung theo yêu cầu. Cả hai **EventBridge Rules** sẽ hướng tới cùng một **Lambda function**. **Lambda function** sẽ gọi **Detective Investigation API**, đợi kết quả, sau đó gọi **Security Hub API** để cập nhật **finding**.

![S3](/images/5/5.5/arch-integration.png)


{{%notice info%}}
**Scenario / Problem Statement**: Kể từ khi nhóm của bạn bật các dịch vụ bảo mật khác nhau của **AWS** và tổng hợp tất cả mọi thứ trong **Security Hub**, ban lãnh đạo của bạn đang yêu cầu các thông tin chi tiết từ tất cả dữ liệu đó. Cụ thể, bạn đã được yêu cầu rút ra các thông tin chi tiết về tuân thủ, cảnh báo có mức độ ưu tiên cao nhất, và các tài nguyên tạo ra số lượng cảnh báo bảo mật nhiều nhất.
{{%/notice%}}

#### Retrieve investigative data from Detective using Lambda
Bạn có thể bắt đầu các cuộc điều tra trong **Detective** và truy xuất kết quả thông qua **API**. Để làm điều này, bạn sẽ sử dụng một **Lambda function** được viết bằng **JavaScript** (**Node.js 20.x**). **AWS Lambda** hỗ trợ nhiều ngôn ngữ lập trình, nhưng bạn sẽ sử dụng **JavaScript** trong ví dụ này.

Để bắt đầu một cuộc điều tra, bạn phải cung cấp cho **API** một **Amazon Resource Name (ARN)** của một **IAM role** hoặc **user**, thời gian bắt đầu, thời gian kết thúc, và **ARN** của **Detective behavior graph**. **Detective API** sẽ truy xuất kết quả của cuộc điều tra bao gồm các chỉ số về việc bị xâm phạm (**IoC**), **TTPs** (chiến thuật, kỹ thuật và quy trình), và một điểm số mức độ nghiêm trọng theo danh mục. Điểm số mức độ nghiêm trọng được trả về được tính toán bằng cách sử dụng hai chiều; **confidence** và **impact**; trong đó **confidence** đại diện cho khả năng cao các sự kiện là bất thường và không phải là hành vi bình thường của người dùng. Chiều thứ hai là **impact**; điều này định lượng tác hại có thể xảy ra từ các sự kiện như một thước đo ảnh hưởng của **TTPs**.

1. Đầu tiên, tạo **Lambda function**. Truy cập https://console.aws.amazon.com/lambda/home?#/create/function?intent=authorFromScratch.

2. Trên trang **Create function**, đặt **Function name** là "InvestigateWithDetective" và chọn **Node.js 20.x** cho **Runtime**.

3. Giữ nguyên tất cả các cài đặt khác. Nhấp vào **Create function**.

4. Sao chép và dán đoạn mã sau vào trình soạn thảo được gắn nhãn **Code source**. Đây sẽ là mục tiêu của **EventBridge rule** trong kiến trúc. **Function** này lấy **ARN** từ một phát hiện bảo mật của **GuardDuty** được **Security Hub** tổng hợp và gọi **Investigation API**. Khi kết quả được trả về, **function** định dạng dữ liệu thành **AWS Security Finding Format (ASFF)** được sử dụng bởi **Security Hub**, và gọi **BatchUpdateFindings API** để gửi **finding** đã được làm giàu và cập nhật trở lại **Security Hub**.


```
"use strict";
import {
  DetectiveClient,
  GetInvestigationCommand,
  ListGraphsCommand,
  StartInvestigationCommand,
} from "@aws-sdk/client-detective";
import { BatchUpdateFindingsCommand, SecurityHubClient } from "@aws-sdk/client-securityhub";

const SHClient = new SecurityHubClient();
const detectiveClient = new DetectiveClient();

export const handler = async (event) => {
  try {
    // Handle only one (the first) finding per function call
    const finding = event.detail.findings[0];

    if (finding.ProductName != "GuardDuty") {
      // Handle only GuardDuty findings
      throw new Error("This is not a GuardDuty finding!");
    }

    const listgraphs = new ListGraphsCommand({});
    const graphs = await detectiveClient.send(listgraphs);
    const graphArn = graphs.GraphList[0].Arn;

    const IAMResourceARNs = finding.Resources.filter((resource) => {
      return (
        resource.Type == "AwsIamRole" ||
        resource.Type == "AwsIamUser" ||
        (resource.Type == "AwsIamAccessKey" && resource.Details.AwsIamAccessKey.PrincipalName != "aws:ec2-instance")
      );
    }).map((resource) => {
      if (resource.Type == "AwsIamRole" || resource.Type == "AwsIamUser") {
        return {
          arn: resource.Id,
          type: resource.Type == "AwsIamRole" ? "role" : "user",
        };
      } else if (resource.Type == "AwsIamAccessKey") {
        return {
          arn: `arn:aws:iam::${finding.AwsAccountId}:role/${resource.Details.AwsIamAccessKey.PrincipalName}`,
          type: "role",
        };
      }
    });

    if (IAMResourceARNs.length == 0) {
      throw new Error("No IAM resource to investigate!");
    }

    // Investigate the first IAM role or user identified in the finding
    const investigationTarget = IAMResourceARNs[0].arn;
    const investigationTargetType = IAMResourceARNs[0].type;

    const investigationEndTime = new Date(Date.now());
    investigationEndTime.setMinutes(investigationEndTime.getMinutes() - 5);

    let investigationStartTime;
    if (finding.FirstObservedAt) {
      investigationStartTime = new Date(finding.FirstObservedAt);
    } else if (finding.CreatedAt) {
      investigationStartTime = new Date(finding.CreatedAt);
    } else if (finding.ProcessedAt) {
      investigationStartTime = new Date(finding.ProcessedAt);
    } else {
      throw new Error("Investigation start time invalid!");
    }
    investigationStartTime.setHours(investigationStartTime.getHours() - 24);

    const totalInvestigationTime = Math.round(
      (investigationEndTime.getTime() - investigationStartTime.getTime()) / (1000 * 60 * 60),
    ); // Hours

    const startInvestigationRequest = {
      GraphArn: graphArn,
      EntityArn: investigationTarget,
      ScopeStartTime: investigationStartTime,
      ScopeEndTime: investigationEndTime,
    };

    const startinvestigation = new StartInvestigationCommand(startInvestigationRequest);
    const investigation = await detectiveClient.send(startinvestigation);
    const investigationId = investigation.InvestigationId;

    const getInvestigationRequest = {
      GraphArn: graphArn,
      InvestigationId: investigationId,
    };

    let investigationResult = { Status: "RUNNING" };
    while (investigationResult.Status == "RUNNING") {
      await new Promise((r) => setTimeout(r, 30000));
      const getinvestigation = new GetInvestigationCommand(getInvestigationRequest);
      investigationResult = await detectiveClient.send(getinvestigation);
      if (investigationResult.Status == "FAILED") {
        throw new Error("Detective investigation failed!");
      }
    }

    let investigationSummary = "";
    switch (investigationResult.Severity) {
      case "INFORMATIONAL":
      case "LOW":
        investigationSummary += `We did not observe uncommon behavior for the associated ${investigationTargetType} during the ${totalInvestigationTime} hour investigation window.`;
        break;
      case "MEDIUM":
        investigationSummary += `We observed anomalous behavior for the associated ${investigationTargetType} during the ${totalInvestigationTime} hour investigation window which might be indicative of compromise.`;
        break;
      case "HIGH":
      case "CRITICAL":
        investigationSummary += `We observed anomalous behavior for the associated ${investigationTargetType} during the ${totalInvestigationTime} hour investigation window indicating potential compromise.`;
        break;
      default:
        throw new Error("Severity information not found!");
    }

    investigationSummary += " For more information, visit ";
    investigationSummary += `https://${process.env.AWS_REGION}.console.aws.amazon.com/detective/home?region=${process.env.AWS_REGION}#investigationReport/${investigationResult.InvestigationId}`;

    const findingUpdateInput = {
      FindingIdentifiers: [
        {
          Id: finding.Id,
          ProductArn: finding.ProductArn,
        },
      ],
      Note: {
        Text: investigationSummary.substring(0, 512),
        UpdatedBy: "Detective Investigation Lambda function.",
      },
      UserDefinedFields: {
        investigate: "complete",
      },
    };

    const batchUpdateCommand = new BatchUpdateFindingsCommand(findingUpdateInput);
    const updatedFinding = await SHClient.send(batchUpdateCommand);

    return updatedFinding;
  } catch (error) {
    console.error("Error:", error);
    throw error;
  }
};
```
5. Sau đó từ menu **Code source**, nhấp vào **File** rồi **Save**.

6. Sau đó nhấp vào nút **Deploy**.

7. Ngay phía trên phần **Code source** cho **Lambda function** bạn đã tạo, nhấp vào tab **Configuration**.

8. Trên tab **General Configuration**, nhấp vào **Edit**.

9. Thời gian chờ của **function** cần được kéo dài đến 15 phút để cho phép **Detective** hoàn thành cuộc điều tra. Cập nhật thời gian chờ thành 15 phút và nhấp vào **Save**.

10. Ngay phía trên phần **Code source** cho **Lambda function** bạn đã tạo, nhấp vào tab **Configuration**.

11. Để **function** này hoạt động như mong muốn, bạn sẽ cần thay đổi quyền của **function**. Quyền này cần bao gồm các hành động cần thiết mà bạn đang thực hiện với **Detective** và **Security Hub** trong **function**. Bạn cần thêm một chính sách mới vào vai trò thực thi cho **Lambda function** này. Sau đó nhấp vào **Permissions** từ bảng điều hướng bên trái xuất hiện.

12. Nhấp vào **Role name**. Tên này sẽ bắt đầu bằng "InvestigateWithDetective-role-". Điều này sẽ đưa bạn đến vai trò trong **Identity and Access Management (IAM) console**.
![VPC](/images/5/5.5/s12.png)

13. Từ trang **Role**, nhấp vào nút **Add permissions** và chọn **Create inline policy**.

14. Trên trang **Create policy**, nhấp vào tab **JSON**. Sau đó thay thế chính sách hiện tại bằng chính sách sau (không sử dụng chính sách này trong môi trường sản xuất, vì nó không hoàn toàn tuân theo nguyên tắc least privilege và chỉ được dùng làm ví dụ).

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "detective:ListGraphs",
                "detective:searchGraph",
                "detective:StartInvestigation",
                "detective:UpdateInvestigationState",
                "detective:GetInvestigation",
                "detective:ListInvestigations",
                "detective:ListIndicators",
                "securityhub:BatchUpdateFindings",
                "securityhub:UpdateFindings"
            ],
            "Resource": "*"
        }
    ]
}
```
15. Nhấp vào nút **Next**. Nếu bạn thấy một biểu ngữ thông báo **You need permissions**, hãy bỏ qua nó.

16. Đặt tên cho chính sách của bạn là "DetectiveSecurityHubIntegration" và nhấp vào **Create policy**.
![VPC](/images/5/5.5/s16.png)

#### Set up automated investigations and finding enrichment with easy to configure filters
Bây giờ bạn đã bật **GuardDuty**, **Security Hub**, và **Detective** (điều này đã được thực hiện cho bạn) và đã thiết lập **Lambda function**, bạn đã sẵn sàng thiết lập hai phương pháp khởi tạo các cuộc điều tra. Phương pháp đầu tiên liên quan đến việc tự động điều tra và làm giàu các **GuardDuty findings** có mức độ nghiêm trọng **HIGH** và **MEDIUM**. Điều này sẽ tăng tốc quá trình điều tra cho các **findings** có mức độ nghiêm trọng cao nhất vì bạn không cần phải vào **Security Hub** hoặc **Detective** và chọn thủ công các **findings** để điều tra.

Trong phương pháp này, bạn sẽ khởi tạo **Lambda function** mà bạn đã thiết lập trước đó bằng cách sử dụng **Security Hub Automations** và một **EventBridge Rule**. Sử dụng **Security Hub Automations** cho phép bạn cấu hình và cập nhật các **findings** nào được tự động điều tra và làm giàu mà không cần thay đổi mã liên tục (các quy tắc tự động hóa sử dụng giao diện người dùng với các tùy chọn thả xuống cho tiêu chí).

17. Mở **Security Hub** và đi đến [Automations page](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/automation-rule).

18. Nhấp vào **Create rule**.

19. Chọn **Create custom rule**.
![VPC](/images/5/5.5/s19.png)

20. Đặt tên cho quy tắc của bạn là "Automated Investigation" và cung cấp mô tả cho quy tắc.

21. Dưới phần **Criteria**, chọn **ProductName** cho **Key**, **Equals** cho **Operator**, và nhập "GuardDuty" cho **Value**.

22. Nhấp vào **Add criteria**.

23. Đối với tiêu chí thứ hai, chọn **SeverityLabel** cho **Key**, **Equals** cho **Operator**, và **HIGH** cho **Value**.

24. Sau đó, dưới tiêu chí đó, nhấp vào văn bản **Add another value**. Lần này, chọn **Equals** cho **Operator** và **MEDIUM** cho **Value**.

25. Nhấp vào **Add criteria** lần thứ ba. Lần này, chọn **ResourceType** cho **Key**, **Equals** cho **Operator**, và nhập "AwsIamAccessKey" cho **Value** (phân biệt chữ hoa chữ thường).

26. Dưới tiêu chí **ResourceType**, nhấp vào **Add another value** hai lần để thêm hai giá trị nữa. Sử dụng **operator** là **Equals** cho cả hai. Sau đó, sử dụng các giá trị "AwsIamUser" và "AwsIamRole" tương ứng. Đảm bảo rằng phần **Criteria** khớp với ảnh chụp màn hình bên dưới.

26. Cuộn xuống phần **Automated action** và nhấp vào **Add another key-value pair**. Đây là cách bạn sẽ thiết lập một trường do người dùng xác định để gắn cờ các **findings** mà bạn muốn tự động điều tra.

27. Đối với **Key**, nhập "investigate", và đối với **Value**, nhập "true". Giữ nguyên tất cả các trường khác.

28. Ở cuối trang, nhấp vào **Create rule**.
![VPC](/images/5/5.5/s28.png)

29. Tiếp theo, bạn cần thiết lập một **EventBridge Rule** để xác định những **findings** nào được điều tra dựa trên trường do người dùng xác định. Bạn có thể sử dụng một quy tắc duy nhất trong **Amazon EventBridge**. Trong trường hợp này, **EventBridge Rule** sẽ tìm kiếm **Security Hub Findings - Imported**. Mỗi sự kiện **Security Hub Findings - Imported** chứa một **finding** duy nhất. Tuy nhiên, bạn chỉ muốn khớp với các **findings** có chứa trường do người dùng xác định, "investigate". Điều hướng đến [EventBridge](https://us-east-1.console.aws.amazon.com/events/home?region=us-east-1#/).

30. Nhấp vào **Create rule** dưới phần **Get started** (với **EventBridge Rule** được chọn).

31. Trên trang **Define rule detail**, đặt tên và mô tả cho quy tắc của bạn. Đối với **Name**, nhập "AutomatedInvestigation". Sau đó nhấp vào **Next**.
![VPC](/images/5/5.5/s31.png)

32. Ở bước **Build event pattern**, cuộn xuống **Event pattern**. Chọn phương pháp **Custom pattern (JSON editor)**.
Sao chép và dán JSON sau vào trường **Event pattern**.


```
{
  "source": ["aws.securityhub"],
  "detail": {
    "findings": {
      "UserDefinedFields": {
        "investigate": ["true"]
      }
    }
  }
}
```
34. Nhấp vào **Next**.
![VPC](/images/5/5.5/s34.png)

35. Ở bước **Select target(s)**, chọn **Lambda function** từ danh sách thả xuống **Select a target**. Sau đó chọn **InvestigateWithDetective** từ danh sách thả xuống **Function**.

36. Nhấp vào **Next**.
![VPC](/images/5/5.5/s36.png)

37. Ở bước **Configure tags**, nhấp vào **Next**.

38. Ở bước **Review and create**, nhấp vào **Create rule**. Khi các **findings** mới được tổng hợp trong **Security Hub**, chúng sẽ được đánh giá và cập nhật bởi quy tắc tự động hóa. Bất kỳ **findings** nào nhận được trường do người dùng xác định sẽ kích hoạt **Lambda function**. Sau khi khởi tạo **Lambda**, quá trình thực thi có thể mất vài phút để hoàn thành và xuất hiện trong **Security Hub**.
![VPC](/images/5/5.5/s38.png)

{{%notice warning%}}
**Lưu ý cho Người Tham Gia**: Có thể mất một khoảng thời gian trước khi một **finding** được cập nhật hoặc tạo mới, điều này sẽ kích hoạt tự động hóa này. Tiếp tục theo hướng dẫn và thực hiện các bước tiếp theo. Trong phần tiếp theo, bạn sẽ thiết lập quy trình khởi tạo cuộc điều tra và làm giàu **finding** theo yêu cầu.
{{%/notice%}}

#### Set up on-demand finding investigation and enrichment
Phương pháp thứ hai liên quan đến việc điều tra và làm giàu **findings** theo yêu cầu. Bạn có thể muốn sử dụng cả hai phương pháp trong trường hợp có những **findings** không đáp ứng tiêu chí của quy tắc tự động hóa trước đó mà bạn vẫn muốn điều tra.

39. Trong phương pháp này, bạn sẽ khởi tạo cùng một **Lambda function** thông qua việc sử dụng tính năng **Custom Actions** trong **Security Hub**. Mở **Security Hub** và truy cập trang [Custom actions](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/settings/actions).

40. Nhấp vào **Create custom action**.

41. Đối với **Action name**, nhập "Investigate". Đối với **Action Description**, nhập "Investigate with Detective". Đối với **Action ID**, nhập "investigate".

42. Nhấp vào **Create custom action**.
![VPC](/images/5/5.5/s42.png)

43. Sao chép **Custom action ARN** đã được tạo cho **finding** tùy chỉnh của bạn. Bạn sẽ cần nó trong một vài bước tiếp theo.

44. Tiếp theo, bạn cần tạo một quy tắc khác trong **EventBridge**, nhưng lần này nó sẽ khớp với các sự kiện cho **custom action** của bạn. Điều hướng đến [EventBridge](https://us-east-1.console.aws.amazon.com/events/home?region=us-east-1#/).

45. Nhấp vào **Create rule** dưới phần **Get started** (với **EventBridge Rule** đã được chọn).

46. Trên trang **Define rule detail**, đặt tên và mô tả cho quy tắc của bạn. Đối với **Name**, nhập "InitiatedAutomatedInvestigation". Sau đó nhấp vào **Next**.
![VPC](/images/5/5.5/s46.png)

47. Ở bước **Build event pattern**, cuộn xuống **Event Pattern**. Để **Event source** được đặt là **AWS Services**. Cuộn xuống **Event pattern**. Dưới **Event source**, chọn **Security Hub**.

48. Đối với **Event Type**, chọn **Security Hub Findings – Custom Action**.

49. Sau đó chọn nút radio **Specific custom action ARN(s)** và nhập **ARN** cho **custom action** mà bạn đã tạo trước đó.

50. Nhấp vào **Next**.
![VPC](/images/5/5.5/s50.png)

51. Ở bước **Select target(s)**, chọn **Lambda function** từ danh sách thả xuống **Select a target**. Sau đó chọn **InvestigateWithDetective** từ danh sách thả xuống **Function**.

52. Nhấp vào **Next**.
![VPC](/images/5/5.5/s52.png)

53. Ở bước **Configure tags**, nhấp vào **Next**.

54. Ở bước **Review and create**, nhấp vào **Create rule**.
![VPC](/images/5/5.5/s54.png)

#### Initiate an on-demand finding investigation
55. Quay lại **Security Hub**. Mở trang **Findings**.

55. Trong ô **Add filter input**, thêm một bộ lọc cho **Resource Type**. Đặt bộ lọc thành **Resource Type is AwsIamAccessKey** (phân biệt chữ hoa chữ thường). Nhấp vào **Apply**.

56. Trong ô **Add filter input**, thêm một bộ lọc cho **Product Name**. Đặt bộ lọc thành **Product Name is GuardDuty** (phân biệt chữ hoa chữ thường). Nhấp vào **Apply**.
![VPC](/images/5/5.5/s56.png)

57. Nhấp vào tiêu đề của một **finding** trong danh sách và kiểm tra xem có bất kỳ **Notes** nào không. Nếu có, hãy mở rộng phần đó và xem nội dung của **notes** (nếu có, bạn có thể xác định **note** đến từ đâu không?). Lưu ý: 
![VPC](/images/5/5.5/s57.png)

58. Đừng chọn **finding** liên quan đến **RegisterManagedInstance**.
![VPC](/images/5/5.5/s58.png)

59. Nhấp vào hộp kiểm bên trái của **finding** này để chọn nó.

60. Trong menu thả xuống **Actions**, chọn **custom action** mới của bạn (Investigate).
![VPC](/images/5/5.5/s60.png)
Chọn một **finding** khác để điều tra:
![VPC](/images/5/5.5/s60b.png)
Sau 3 phút:
![VPC](/images/5/5.5/s60c.png)

61. Quá trình điều tra có thể mất tới 15 phút để hoàn thành. Làm mới trang sau mỗi vài phút cho đến khi **finding** có một phần **Notes** mới trong chi tiết. Nhấp vào tiêu đề của **finding** đó và mở rộng phần **Notes**.
Chờ sau 5 phút để thấy cập nhật **node**:
![VPC](/images/5/5.5/s61.png)

62. Bạn cũng có thể chuyển sang tab **History** trong chi tiết **finding** để xem sự kiện được đánh dấu thời gian với thay đổi **Note**.

#### Surface investigated findings in a custom insight
63. Nếu bạn muốn dễ dàng theo dõi tài nguyên và **findings** đã được điều tra thông qua bất kỳ phương pháp nào mà bạn đã triển khai, hãy tạo một **custom insight** mới trong **Security Hub**. Mở **Security Hub** và truy cập trang [Insights](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights).

64. Nhấp vào **Create insight**.

65. Nhấp vào trường ghi "Add filter" ở đầu trang và chọn **User defined fields**.

66. Đặt **Key** là **investigate** và **Value** là **complete**. Điều này sẽ lọc danh sách **findings** chỉ bao gồm những **findings** đã được làm giàu thành công. Nhấp vào **Apply**.
![VPC](/images/5/5.5/s66.png)
Kết quả:
![VPC](/images/5/5.5/s66b.png)

67. Nhấp vào trường ghi "Add filter" ở đầu trang một lần nữa và chọn **GroupBy**. Sau đó chọn **Resource ID**. Nhấp vào **Apply**.
![VPC](/images/5/5.5/s67.png)

68. **Insight** hiện đang hiển thị danh sách các tài nguyên và số lượng **findings** đang hoạt động, liên quan đến tài nguyên đó đã được điều tra và làm giàu.
![VPC](/images/5/5.5/s68.png)

69. Nhấp vào **Create insight**. Đặt tên cho **insight** của bạn là "Investigated Resources" và nhấp vào **Create insight** một lần nữa.
![VPC](/images/5/5.5/s69.png)

70. Ở đầu trang, nhấp vào **Insight details** để xem các biểu đồ cho **insight** mới.
![VPC](/images/5/5.5/s70.png)
