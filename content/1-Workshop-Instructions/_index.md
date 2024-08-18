---
title : "Workshop Instructions"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

This workshop is designed to give you an introduction and then take you deeper into AWS threat detection and response services use cases, best practices, and specific scenarios. This workshop starts with an introduction to services and then focuses on advanced topics of threat detection and response with modules focusing on multi-service solutions, integrations, custom orchestration examples, and examples of responding to specific detections. All of this is designed to prepare you and help you operate more securely on AWS.

### Available labs

#### Introduction to Threat Detection and Response Services
| Module | Topic                                             | Level | Services     |
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

#### Integrating AWS Services and Partner Solutions
| Module | Topic                                           | Level | Services                    |
|--------|-------------------------------------------------|-------|-----------------------------|
| 3.1    | Centralizing findings from AWS security services| 100   | Security Hub, GuardDuty     |
| 3.2    | Aggregating findings from multiple AWS accounts | 100   | --                          |
| 3.3    | Centralizing findings from AWS partner solutions| 100   | Security Hub                |
| 3.4    | Cross-region finding aggregation                | 200   | Security Hub                |
| 3.5    | Building your own Security Hub integration      | 100   | Security Hub                |

#### Managing and Prioritizing Security Findings
| Module | Topic                                           | Level |
|--------|-------------------------------------------------|-------|
| 4.1    | Prioritizing findings at scale with automations | 100   |
| 4.2    | Suppressing findings at scale with automations  | 100   |
| 4.3    | Using insights for prioritization and metrics   | 200   |

#### Automating Notifications and Response
| Module | Topic                                           | Level | Services                            |
|--------|-------------------------------------------------|-------|-------------------------------------|
| 5.1    | Setting up notifications                        | 200   | Security Hub                        |
| 5.2    | Set up a weekly vulnerability summary email     | 300   | Security Hub, Inspector             |
| 5.3    | Automated Security Response on AWS              | 200   | Security Hub                        |
| 5.4    | Building your own automated response            | 300   | Security Hub, GuardDuty             |
| 5.5    | Enriching security findings with investigative data | 400   | Security Hub, GuardDuty, Detective  |

#### Security Simulations and Scenarios
| Module | Topic                                           | Level | Services                    |
|--------|-------------------------------------------------|-------|-----------------------------|
| 6.1    | Respond to IAM Role credential exfiltration     | 300   | GuardDuty                   |
| 6.2    | Respond to a compromised S3 Bucket              | 300   | GuardDuty                   |
| 6.3    | Respond to compromised IAM credentials          | 300   | GuardDuty, Detective        |
| 6.4    | Respond to a Lambda function calling malicious IP | 300 | GuardDuty                   |
| 6.5    | Respond to Malware on Amazon Elastic Block Store | 200  | GuardDuty                   |
| 6.6    | Respond to a compromised EC2 instance           | 200   | GuardDuty, Detective        |

#### Software Vulnerability Management
| Module | Topic                                           | Level | Services                          |
|--------|-------------------------------------------------|-------|-----------------------------------|
| 7.1    | Patching EC2 with Patch Manager                 | 300   | Inspector, Systems Manager        |
| 7.2    | Vulnerability management for serverless applications | 300 | Inspector                         |
| 7.3    | Integrating Amazon Inspector into a CI/CD pipeline | 300 | Inspector                         |
