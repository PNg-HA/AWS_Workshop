---
title : "Detective - Summary"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.4.2 </b> "
---
Use the Summary page in Amazon Detective to identify entities to investigate the origin of activity during the previous 24 hours. The Amazon Detective Summary page helps you to identify entities that are associated with specific types of unusual activity. It is one of several possible starting points for an investigation.

To display the Summary page, in the Detective navigation pane, choose Summary. The Summary page is also displayed by default when you first open the Detective console.

From the Summary page, you can identify entities that meet the following criteria:
- Investigations that show potential security events identified by Detective.
- Entities involved in activity that occurred in newly observed geolocations
- Entities that made the largest number of API calls
- EC2 instances that had the largest volume of traffic
- Container clusters that had the largest number of containers

From each Summary page panel, you can pivot to the profile for a selected entity.

As you review the Summary page, you can adjust the Scope time to view the activity for any 24-hour time frame in the previous 365 days. When you change the Start date and time, the End date and time is automatically updated to 24 hours after your chosen start time.

With Detective, you can access up to a year of historical event data. This data is available through a set of visualizations that show changes in the type and volume of activity over a selected time window. Detective links these changes to GuardDuty findings.