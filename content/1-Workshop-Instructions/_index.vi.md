---
title : "Giới thiệu workshop"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 1. </b> "
---
Workshop này được thiết kế để cung cấp cho bạn một cái nhìn tổng quan về các dịch vụ phát hiện và ứng phó mối đe dọa trên AWS, sau đó đi sâu vào các tình huống sử dụng thực tế, các phương pháp tốt nhất, và những kịch bản cụ thể. Workshop sẽ khởi đầu bằng phần giới thiệu tổng quan về các dịch vụ, tiếp đến là các chủ đề nâng cao về phát hiện và ứng phó mối đe dọa với các mô-đun tập trung vào giải pháp đa dịch vụ, tích hợp hệ thống, điều phối tùy chỉnh, và xử lý các tình huống đe dọa cụ thể. Tất cả các nội dung này được thiết kế nhằm trang bị cho bạn kiến thức và kỹ năng để quản lý và vận hành hệ thống một cách an toàn và hiệu quả hơn trên nền tảng AWS.

  
### Các bài lab

#### Giới thiệu về các dịch vụ phát hiện và ứng phó mối đe dọa
| Mục | Chủ đề                                             | Độ khó | Dịch vụ liên quan     |
|--------|---------------------------------------------------|-------|--------------|
|        | **AWS Security Hub**                              |       |              |
| 2.1.1  | Security Hub - Overview                           | 100   | Security Hub |
| 2.1.2  | Security Hub - Dashboard                          | 100   | Security Hub |
| 2.1.3  | Security Hub - Findings                           | 100   | Security Hub |
| 2.1.4  | Security Hub - Pricing                            | 100   | Security Hub |
| 2.1.5  | Security Hub - Notifications                      | 100   | Security Hub |
|        | **Amazon GuardDuty**                              |       |              |
| 2.2.1  | GuardDuty - Overview                              | 100   | GuardDuty    |
| 2.2.2  | GuardDuty - Findings                              | 100   | GuardDuty    |
| 2.2.3  | GuardDuty - Protection plans                      | 100   | GuardDuty    |
| 2.2.4  | GuardDuty - Building your own threat list         | 100   | GuardDuty    |
| 2.2.5  | GuardDuty - Suppressing findings                  | 100   | GuardDuty    |
| 2.2.6  | GuardDuty - Pricing                               | 100   | GuardDuty    |
| 2.2.7  | GuardDuty - Simple notifications                  | 100   | GuardDuty    |
| 2.2.8  | GuardDuty - Retaining findings                    | 100   | GuardDuty    |
|        | **Amazon Inspector**                              |       |              |
| 2.3.1  | Inspector - Overview                              | 100   | Inspector    |
| 2.3.2  | Inspector - Dashboard                             | 100   | Inspector    |
| 2.3.3  | Inspector - Findings                              | 100   | Inspector    |
| 2.3.4  | Inspector - Vulnerability database search         | 100   | Inspector    |
| 2.3.5  | Inspector - Suppressing findings                  | 100   | Inspector    |
| 2.3.6  | Inspector - Pricing                               | 100   | Inspector    |
|        | **Amazon Detective**                              |       |              |
| 2.4.1  | Detective - Overview                              | 100   | Detective    |
| 2.4.2  | Detective - Summary                               | 100   | Detective    |
| 2.4.3  | Detective - Search                                | 100   | Detective    |
| 2.4.4  | Detective - Investigations                        | 100   | Detective    |
| 2.4.5  | Detective - Finding Groups                        | 100   | Detective    |
| 2.4.6  | Detective - Pricing                               | 100   | Detective    |
| 2.4.7  | Detective - EKS Audit Logs                        | 100   | Detective    |

#### Tích hợp Dịch vụ AWS và Giải pháp Đối tác
| Mục | Chủ đề                                           | Độ khó | Dịch vụ liên quan                    |
|--------|-------------------------------------------------|-------|-----------------------------|
| 3.1    | Centralizing findings from AWS security services| 100   | Security Hub, GuardDuty     |
| 3.2    | Aggregating findings from multiple AWS accounts | 100   | --                          |
| 3.3    | Centralizing findings from AWS partner solutions| 100   | Security Hub                |
| 3.4    | Cross-region finding aggregation                | 200   | Security Hub                |
| 3.5    | Building your own Security Hub integration      | 100   | Security Hub                |

#### Quản lý và Ưu tiên các phát hiện bảo mật
| Mục | Chủ đề                                           | Độ khó |
|--------|-------------------------------------------------|-------|
| 4.1    | Prioritizing findings at scale with automations | 100   |
| 4.2    | Suppressing findings at scale with automations  | 100   |
| 4.3    | Using insights for prioritization and metrics   | 200   |

#### Tự động hóa thông báo và ứng phó
| Mục | Chủ đề                                           | Độ khó | Dịch vụ liên quan                            |
|--------|-------------------------------------------------|-------|-------------------------------------|
| 5.1    | Setting up notifications                        | 200   | Security Hub                        |
| 5.2    | Set up a weekly vulnerability summary email     | 300   | Security Hub, Inspector             |
| 5.3    | Automated Security Response on AWS              | 200   | Security Hub                        |
| 5.4    | Building your own automated response            | 300   | Security Hub, GuardDuty             |
| 5.5    | Enriching security findings with investigative data | 400   | Security Hub, GuardDuty, Detective  |

#### Mô phỏng kịch bản bảo mật
| Mục | Chủ đề                                           | Độ khó | Dịch vụ liên quan                    |
|--------|-------------------------------------------------|-------|-----------------------------|
| 6.1    | Respond to IAM Role credential exfiltration     | 300   | GuardDuty                   |
| 6.2    | Respond to a compromised S3 Bucket              | 300   | GuardDuty                   |
| 6.3    | Respond to compromised IAM credentials          | 300   | GuardDuty, Detective        |
| 6.4    | Respond to a Lambda function calling malicious IP | 300 | GuardDuty                   |
| 6.5    | Respond to Malware on Amazon Elastic Block Store | 200  | GuardDuty                   |
| 6.6    | Respond to a compromised EC2 instance           | 200   | GuardDuty, Detective        |

#### Quản lý Lỗ hổng Phần mềm
| Mục | Chủ đề                                           | Độ khó | Dịch vụ liên quan                          |
|--------|-------------------------------------------------|-------|-----------------------------------|
| 7.1    | Patching EC2 with Patch Manager                 | 300   | Inspector, Systems Manager        |
| 7.2    | Vulnerability management for serverless applications | 300 | Inspector                         |
| 7.3    | Integrating Amazon Inspector into a CI/CD pipeline | 300 | Inspector                         |
