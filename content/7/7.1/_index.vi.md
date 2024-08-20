---
title : "Cập nhật bản vá cho EC2 với Patch Manager"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---

1. Tìm số lượng phát hiện bảo mật theo sản phẩm trong Security Hub bằng cách mở trang Insights và nhấp vào Insight: 23. Top products by counts of findings. https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/arn:aws:securityhub:::insight/securityhub/default/28 
![VPC](/images/7/7.1/s1.png)

2. Lưu ý rằng một phần đáng kể các phát hiện bảo mật trong môi trường của bạn đến từ Inspector.



3. Nhấp vào Inspector để xem danh sách các phát hiện cá nhân. Dựa trên tiêu đề của các phát hiện này, có vẻ như hầu hết, nếu không muốn nói là tất cả, đều là lỗ hổng phần mềm.

![VPC](/images/7/7.1/s3.png)

4. Điều hướng đến bảng điều khiển Inspector. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/dashboard 

![VPC](/images/7/7.1/s4.png)

5. Từ bảng điều khiển Inspector, bạn có thể nhanh chóng thấy rằng hầu hết các EC2 instance và hàm Lambda của bạn đang được quét. Bạn cũng có thể thấy các lỗ hổng ảnh hưởng đến nhiều instance và image nhất.


{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Nhóm của bạn đã nhận thấy ngày càng nhiều các phát hiện bảo mật từ Amazon Inspector. Nhóm của bạn đã bật Inspector để liên tục quét Amazon Elastic Compute Cloud (EC2), các hàm AWS Lambda và container image trong Amazon ECR để tìm lỗ hổng phần mềm. Tuy nhiên, dựa trên số lượng phát hiện, có vẻ như không có ai đang quản lý các lỗ hổng này. Bạn đã được giao nhiệm vụ xác định và thiết lập quản lý bản vá cho các instance EC2 của mình, bắt đầu với những instance đang được sử dụng cho phát triển.
{{%/notice%}}

Amazon Inspector là một dịch vụ quản lý lỗ hổng liên tục quét các khối lượng công việc AWS của bạn để tìm lỗ hổng phần mềm và tiếp xúc mạng không mong muốn. Amazon Inspector tự động phát hiện và quét các Amazon EC2 instance đang chạy, các container image trong Amazon Elastic Container Registry (Amazon ECR), và các hàm AWS Lambda để tìm lỗ hổng phần mềm đã biết và tiếp xúc mạng không mong muốn.

Amazon Inspector tạo ra một phát hiện khi nó phát hiện ra lỗ hổng phần mềm hoặc vấn đề cấu hình mạng. Một phát hiện mô tả lỗ hổng, xác định tài nguyên bị ảnh hưởng, đánh giá mức độ nghiêm trọng của lỗ hổng và cung cấp hướng dẫn khắc phục. Bạn có thể phân tích các phát hiện bằng cách sử dụng bảng điều khiển Amazon Inspector hoặc xem và xử lý các phát hiện của bạn thông qua các dịch vụ AWS khác.

Patch Manager, một khả năng của AWS Systems Manager, tự động hóa quá trình cập nhật các node được quản lý với các bản cập nhật liên quan đến bảo mật và các loại cập nhật khác. Bạn có thể sử dụng Patch Manager để áp dụng các bản vá cho cả hệ điều hành và ứng dụng. (Trên Windows Server, hỗ trợ ứng dụng bị giới hạn ở các bản cập nhật cho ứng dụng do Microsoft phát hành.) Bạn có thể sử dụng Patch Manager để cài đặt Service Packs trên các Windows node và thực hiện nâng cấp phiên bản nhỏ trên các Linux node. Bạn có thể vá các nhóm Amazon Elastic Computing Cloud (Amazon EC2) instance, thiết bị biên, máy chủ tại chỗ và máy ảo (VM) theo loại hệ điều hành.

Bạn có thể quét các instance để chỉ xem báo cáo về các bản vá bị thiếu hoặc bạn có thể quét và tự động cài đặt tất cả các bản vá bị thiếu.

Patch Manager sử dụng các bản vá baseline, bao gồm các quy tắc phê duyệt tự động các bản vá trong vài ngày kể từ khi chúng được phát hành, cùng với các danh sách tùy chọn về các bản vá được chấp thuận và bị từ chối. Khi một hoạt động vá diễn ra, Patch Manager so sánh các bản vá hiện đang được áp dụng cho một node được quản lý với những bản vá nên được áp dụng theo các quy tắc thiết lập trong bản vá baseline. Bạn có thể chọn để Patch Manager chỉ hiển thị cho bạn một báo cáo về các bản vá thiếu (Scan), hoặc bạn có thể chọn để Patch Manager tự động cài đặt tất cả các bản vá mà nó tìm thấy là thiếu từ một nút được quản lý (Scan và cài đặt).

Trong phần này bạn sẽ:

- Xác định bản vá baseline
- Quét các instance của bạn với AWS-RunPatchBaseline qua Run Command
- Xem xét việc tuân thủ bản vá cho các node được quản lý
- Cài đặt các bản vá bị thiếu bằng Run Command
- Lên lịch các hoạt động vá lỗi bằng cách sử dụng chính sách bản vá
- Xem lại liên kết được tạo bởi chính sách bản vá

#### Xác định bản vá baseline
6. Điều hướng đến Systems Manager và mở Patch Manager từ thanh điều hướng bên trái. https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager?region=us-east-1 


7. Nhấp vào Start with an overview, dưới nút "Create patch policy".
![VPC](/images/7/7.1/s7.png)

8. Nhấp vào tab Patch baselines, sau đó chọn Create patch baseline.
![VPC](/images/7/7.1/s8.png)

9. Trên trang "Create patch baseline", nhập tên "AmazonLinux2SecAndNonSecBaseline" trong phần Patch baseline details.


10. Trên trang "Create patch baseline", chọn hệ điều hành "Amazon Linux 2" trong phần Patch baseline details.



11. Trong phần Approval rules for operating systems, chọn "All" Products, "Security"  "Bugfix" Classification, cũng như Mức độ nghiêm trọng "Critical" và "Important" Severity. Chọn Approve patches after a specified number of days và nhập  "14" ngày. Cuối cùng, chọn "Critical" cho Compliance reporting.



12. Xác nhận rằng Operating system rule 1 trông giống như hình ảnh sau.

![VPC](/images/7/7.1/s12.png)

13. Nhấp vào Add rule để tạo "Operating system rule 2".


14. Đối với "Operating system rule 2", để tất cả các giá trị mặc định, ngoại trừ việc đặt tự động phê duyệt thành 7 ngày, Compliance reporting to "Medium", và chọn ô để bao gồm các cập nhật không bảo mật.



15. Xác nhận rằng Operating system rule 2 trông giống như hình ảnh sau.

![VPC](/images/7/7.1/s15.png)

16.  Ở cuối trang, nhấp vào Create patch baseline. Bạn sẽ thấy một thông báo ở đầu trang ghi rằng "Create patch baseline request succeeded". Bản vá Baseline này sẽ chỉ quét hoặc cài đặt các bản cập nhật dựa trên các tiêu chí được xác định trong các quy tắc phê duyệt bản vá. Nếu một EC2 instance thiếu một bản vá dựa trên các tiêu chí được chỉ định trong baseline bản vá, instance đó sẽ bị gắn cờ là không tuân thủ.

![VPC](/images/7/7.1/s16.png)
Kết quả:
![VPC](/images/7/7.1/s16r.png)
#### Quét các instance với AWS-RunPatchBaseline qua Run Command
Sử dụng Run Command, một khả năng của AWS Systems Manager, bạn có thể quản lý cấu hình của các node từ xa và an toàn.  Một node được quản lý là bất kỳ Amazon Elastic Compute Cloud (Amazon EC2) instance hoặc máy không thuộc EC2 nào trong môi trường hybrid và multi-cloud của bạn đã được cấu hình cho Systems Manager. Run Command cho phép bạn tự động hóa các tác vụ quản trị chung và thực hiện các thay đổi cấu hình một lần trên quy mô lớn.

AWS Systems Manager hỗ trợ AWS-RunPatchBaseline, một Systems Manager Document (SSM document) cho Patch Manager, một khả năng của AWS Systems Manager. SSM Document này thực hiện các hoạt động vá trên các node được quản lý cho cả các cập nhật liên quan đến bảo mật và các loại cập nhật khác. Khi Document được chạy, nó sử dụng bản vá baseline được chỉ định là "default" " cho một loại hệ điều hành nếu không có nhóm bản vá nào được chỉ định. Nếu không, nó sử dụng bản vá baseline được liên kết với nhóm bản vá.

{{%notice tip%}}
Bạn có thể tìm hiểu thêm về AWS-RunPatchBaseline SSM document trong phần Documents của Systems Manager hoặc trong tài liệu tại đây: https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-aws-runpatchbaseline.html 
{{%/notice%}}

17. Mở [Run Command](https://us-east-1.console.aws.amazon.com/systems-manager/run-command?region=us-east-1) từ bảng điều hướng bên trái.


18. Nhấp vào nút Run command.



19. Tìm kiếm và chọn AWS-RunPatchBaseline.
![VPC](/images/7/7.1/s19.png)

20. Trong phần Command parameters, đảm bảo rằng "Scan" là Operation được chọn. Giữ nguyên tất cả các tham số mặc định khác.


21. Trong phần Target selection, xác định thẻ instance. Nhập "Environment" cho Tag key và "Development" cho Tag value.


22. Nhấp vào Add bên cạnh cặp key-value mà bạn vừa nhập.
![VPC](/images/7/7.1/s22.png)

23. Trong phần Output options, bỏ chọn Enable an S3 bucket.

![VPC](/images/7/7.1/s23.png)

24. Nhấp vào Run ở cuối trang. Làm mới trang cho đến khi overall status là "Success".
![VPC](/images/7/7.1/s24.png)

25. Trong phần Targets and outputs, nhấp vào Instance ID của một trong các EC2 instances targeted (có cặp key-value mà bạn đã chỉ định) để xem kết quả từ quá trình thực thi lệnh. Dành một chút thời gian để xem xét các kết quả của từng bước.
![VPC](/images/7/7.1/s25.png)
Quan sát bước 2:
![VPC](/images/7/7.1/s25_step2.png)
Quan sát bước 3:
![VPC](/images/7/7.1/s25_step3.png)
#### Xem xét sự tuân thủ bản vá cho các node được quản lý

26. Quay lại bảng điều khiển của Patch Manager bằng cách sử dụng thanh điều hướng trong Systems Manager. https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager/dashboard?region=us-east-1 
![VPC](/images/7/7.1/s26.png)

27. Ở đây bạn sẽ nhận thấy rằng một số EC2 instance của bạn không tuân thủ và nhiều node thiếu các bản vá.


28. Mở tab Compliance reporting và xem xét các instance được liệt kê cùng với trạng thái tuân thủ của chúng và thông tin khác như các bản vá còn thiếu.
![VPC](/images/7/7.1/28.png)

29. Chọn một trong các EC2 instance có trạng thái tuân thủ là Not compliant.


30. Nhấp vào liên kết cho instance đó trong cột Security non-compliant count.
![VPC](/images/7/7.1/s30.png)

31. Trang này hiển thị tóm tắt các bản vá cho instance. Bạn có thể chọn các bản vá còn thiếu để xem thêm chi tiết nếu muốn.
![VPC](/images/7/7.1/s31.png)

#### Cài đặt các bản vá còn thiếu bằng Run Command
32. Quay lại trang Run Command trong Systems Manager và nhấp vào nút Run Command.


33. Từ danh sách các command document, tìm kiếm và chọn AWS-RunPatchBaseline.


34. Lần này, trong phần Command parameters, đặt Operation thành "Install".


35. Một lần nữa, trong phần Target selection, đặt Tag key là "Environment" và Tag value là "Development". Nhấp vào Add.
![VPC](/images/7/7.1/s35.png)

36. Mở rộng phần Rate control và đặt Concurrency targets là 1 và Error threshold là 1.

![VPC](/images/7/7.1/s36.png)

37. Bỏ chọn Enable an S3 bucket trong phần Output options.


38. Nhấp vào nút Run ở cuối trang. Quá trình này sẽ mất từ 3-5 phút để hoàn thành. Nếu có bất kỳ bản cập nhật nào được cài đặt bởi Patch Manager, instance được vá sẽ tự động khởi động lại theo mặc định. Làm mới trang cho đến khi bạn thấy trạng thái là "Success".
Quan sát bước 1, 2:
![VPC](/images/7/7.1/s38_step1-2.png)
Quan sát bước 3:
![VPC](/images/7/7.1/s38_step3.png)
39. Để kiểm tra những instance nào vẫn thiếu các bản vá, hãy quay lại bảng điều khiển của Patch Manager và mở tab Compliance reporting. Bạn sẽ thấy rằng không còn bản vá nào bị thiếu nữa!


<!-- #### Lên lịch hoạt động vá lỗi bằng cách sử dụng chính sách bản vá
Chính sách bản vá là một cấu hình mà bạn thiết lập bằng Quick Setup, một tính năng của AWS Systems Manager. Chính sách bản vá cung cấp khả năng kiểm soát trung tâm và rộng rãi hơn đối với các hoạt động bản vá so với các phương pháp cấu hình bản vá khác. Chính sách bản vá định nghĩa lịch trình và cơ sở mà bạn sử dụng khi tự động bản vá các node và ứng dụng của mình.

Thay vì sử dụng các phương pháp khác để bản vá các node của bạn, hãy sử dụng chính sách bản vá để tận dụng các tính năng chính sau:

- Thiết lập một lần – Thiết lập các hoạt động vá lỗi bằng cách sử dụng cửa sổ bảo trì hoặc liên kết State Manager có thể yêu cầu nhiều tác vụ ở các phần khác nhau trong bảng điều khiển Systems Manager. Khi sử dụng chính sách bản vá, tất cả các hoạt động vá lỗi của bạn có thể được thiết lập trong một trình hướng dẫn duy nhất.
- Hỗ trợ nhiều Account / nhiều Region – Khi sử dụng cửa sổ bảo trì, liên kết State Manager, hoặc tính năng Patch now trong Patch Manager, bạn bị giới hạn trong việc chọn các node mục tiêu được quản lý trong một cặp  AWS Account - AWS Region duy nhất.  Nếu bạn sử dụng nhiều Account và nhiều Region, các tác vụ thiết lập và bảo trì của bạn có thể mất rất nhiều thời gian, vì bạn phải thực hiện các tác vụ thiết lập trong từng cặp Account - Region. Tuy nhiên, nếu bạn sử dụng AWS Organizations, bạn có thể thiết lập một chính sách bản vá áp dụng cho tất cả các node được quản lý trong tất cả các AWS Region trong tất cả các AWS Account của bạn. Hoặc, nếu bạn muốn, một chính sách bản vá có thể áp dụng chỉ cho một số đơn vị tổ chức (OU) trong các Account và Region bạn chọn. Một chính sách bản vá cũng có thể áp dụng cho một Account cục bộ duy nhất, nếu bạn muốn.
- Hỗ trợ cài đặt ở cấp độ tổ chức – Tùy chọn cấu hình Host Management configuration hiện có trong Quick Setup cung cấp hỗ trợ quét hàng ngày các node được quản lý của bạn để đảm bảo tuân thủ bản vá lỗi. Tuy nhiên, việc quét này được thực hiện vào thời gian đã được xác định trước và chỉ cung cấp thông tin tuân thủ bản vá lỗi. Không có cài đặt bản vá lỗi nào được thực hiện. Sử dụng chính sách bản vá, bạn có thể chỉ định các lịch trình khác nhau để quét và cài đặt. Bạn cũng có thể chọn tần suất và thời gian của các hoạt động này bằng cách sử dụng các biểu thức CRON hoặc Rate tùy chỉnh. Ví dụ, bạn có thể quét các bản vá lỗi bị thiếu hàng ngày để cung cấp cho bạn thông tin tuân thủ cập nhật thường xuyên. Lịch trình cài đặt của bạn có thể chỉ là một lần mỗi tuần để tránh thời gian lãng phí không mong muốn.
- Lựa chọn bản vá baseline được đơn giản hóa – Chính sách bản vá vẫn tích các hợp bản vá baseline, và không có thay đổi nào đối với cách cấu hình các bản vá baseline.  Tuy nhiên, khi bạn tạo hoặc cập nhật một chính sách bản vá, bạn có thể chọn baseline do AWS quản lý hoặc baseline tùy chỉnh mà bạn muốn sử dụng cho từng loại hệ điều hành (OS) trong một danh sách duy nhất. Không cần phải chỉ định baseline mặc định cho từng loại OS trong các tác vụ riêng biệt.



40.  Trở về bảng điều khiển Patch Manager bằng cách sử dụng thanh điều hướng trong Systems Manager. https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager/dashboard?region=us-east-1 


41. Nhấp vào Create patch policy. 
{{%notice tip%}}
Nếu AWS Quick Setup hiển thị yêu cầu bạn Get started with Quick Setup, hãy chọn us-east-1 làm khu vực chính và nhấp vào nút Get started.
{{%/notice%}}


42.	Trên trang Create patch policy, đặt Configuration name là "patch-policy-workshop".


43.	Đối với Scanning and installation, chọn Scan and install.


44.	Đối với Installation schedule, chọn Custom install schedule.

45.	Đối với Installation frequency, chọn Custom CRON Expression và sau đó nhập biểu thức CRON "cron(30 23 ? * SAT *)" để cài đặt bản cập nhật vào Thứ Bảy lúc 23:30 UTC.


46.	Đảm bảo Wait to install updates until first CRON interval. đã được chọn.


47.	Trong phần Patch baseline, chọn Custom patch baseline.



48.	Dưới View or change baselines, đối với "Amazon Linux 2", chọn "AmazonLinux2SecAndNonSecBaseline".


49.	Bỏ chọn Write output to S3 bucket.


50.	Dưới Targets, chọn Specify node tag.


51.	Dưới Instance tag details, nhấp vào Add. Nhập "Environment" cho Key và "Development" cho Value.


52.	Đối với Rate control, nhập "50" Percentage cho cả Concurrency và Error threshold.


53.	Đánh dấu vào ô Add required IAM policies to existing instance profiles attached to your instances.


54.Nhấp vào Create. Quick Setup của Patch Manager sẽ mất khoảng 2 phút để cập nhật. Chờ đến khi xuất hiện thông báo "Your Patch Manager Quick Setup configuration was successfully updated."


#### Xem lại Association được tạo bởi chính sách vá lỗi
55. Mở State Manager bằng cách sử dụng thanh điều hướng bên trái trong Systems Manager. https://us-east-1.console.aws.amazon.com/systems-manager/state-manager?region=us-east-1# 


56. Từ danh sách Associations, tìm liên kết bắt đầu bằng "AWS-QuickSetup-PatchPolicy-ScanForPatches-LA-". Nhấp vào liên kết cho association id để mở mô tả.


57. Nếu trạng thái là "Failed" hoặc "Pending", nhấp vào Apply association now để chạy hoạt động vá lỗi ngay bây giờ. Nếu được yêu cầu "Are you sure you want to send the request to apply this association now?", nhấp vào Apply lần nữa.


58. Chờ 2-3 phút để trạng thái hiển thị "Success".


59. Mở lại Association bắt đầu bằng "AWS-QuickSetup-PatchPolicy-ScanForPatches-LA-". Xem lại thông tin dưới từng tab, bắt đầu từ Description và kết thúc với Execution history. -->