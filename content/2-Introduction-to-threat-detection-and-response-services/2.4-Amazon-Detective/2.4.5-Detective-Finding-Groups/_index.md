---
title : "Detective - Finding Groups"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.4.5 </b> "
---
Amazon Detective finding groups let you examine multiple activities as they relate to a potential security event. You can analyze the root cause for high severity GuardDuty findings using finding groups. If a threat actor is attempting to compromise your AWS environment, they typically perform a sequence of actions that lead to multiple security findings and unusual behaviors. These actions are often spread across time and entities. When security findings are investigated in isolation, it can lead to a misinterpretation of their significance, and difficulty in finding the root cause. Amazon Detective addresses this problem by applying a graph analysis technique that infers relationships between findings and entities, and groups them together. We recommend treating finding groups as the starting point for investigating the involved entities and findings.

Detective analyzes data from findings and groups them with other findings that are likely to be related based on resources they share. For example, findings related to actions taken by the same IAM role sessions or originating from the same IP address are very likely to be part of the same underlying activity. It's valuable to investigate findings and evidence as a group, even if the associations made by Detective aren't related.

In addition to findings, each group includes entities involved in the findings. The entities can include resources outside of AWS such as IP Addresses or user agents.

After an initial GuardDuty finding occurs that is related to another finding, the finding group with all related findings and all involved entities is created within 48 hours.