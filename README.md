<div align="center">

# Project Overview
In this guided project, the goal is to apply hands-on practice to accurately and confidently perform the tasks required to configure Microsoft Sentinel to ingest data and detect threats. This project is meant to give an understanding of how to:
<li>Create a Log Analytics Workspace and install the Microsoft Sentinel Solution.</li>
<li>Identify and install Content Hub Solutions.</li>
<li>Explore and configure data connectors.</li>
<li>Enable and configure analytic rules.</li>
<li>Create and configure an automation rule for a repetitive task.</li>
<li>Explore MITRE ATT&CK and display analytic rule coverage.</li>
<li>Enable and save a workbook and then view the graphical information from your data.</li>

## Create a Sentinel Workspace
Go to the Microsoft Azure portal (portal.azure.com). In the search bar, search for and select Microsoft Sentinel. In the command bar, click Create.

![1](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/c43ba1d8-4b9e-435a-8d60-1c67d8c62c4a)

On the next page, in the command bar, click Create a new workspace. Under the Basic tab, the Subscription will be automatically selected. For the Resource group, click on the Create new link and fill in the name (rgSentinel). Under Instance details, fill in the Name (mySentinel) and select the Region. Note that the name of the Sentinel workspace will be named after the log analytics workspace. Continue by clicking Review + create, followed by Create. 

![2](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/6b340b85-20b7-49a4-be42-d2163a2d894f)

When the workspace is successfully created, select the mySentinel workspace and click Add. You will be automatically forwarded to the Microsoft Sentinel workspace.

## Install Content Hub Solutions
Having created the Microsoft Sentinel workspace, we need to configure components to ingest logs and correlate log data to generate alerts and manage incidents. In this task, we install solutions from the Microsoft Sentinel content hub solution that contain data connectors, analytic rules, workbooks, and other components. These solutions are not only from Microsoft but can also be from third-party vendors. In the Microsoft Sentinel workspace, under Content management, select Content hub. Search for, select and install Azure Active Directory (or Entra ID), Azure Activity, Microsoft 365 Defender, and Windows Security Events.

![3](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/fe7e643e-87f7-4db6-aa99-9d963b4eb2f6)

## Explore and Configure Data Connectors
In the Microsoft Sentinel workspace, navigate to the side menu, under Configuration, select Data connectors. All previously installed connectors are displayed here. Select the Azure Active Directory connector, which opens a side panel, and click on the Open connector page button.

![4](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/11b9985d-6d66-4625-b969-af8d829adcb6)

In the left panel of the Azure Active Directory data connector, information is displayed as well as the Data types that will be ingested with this data connector. On the right panel, the log types are selected. Under Configuration, select the Sign-In Logs and Audit Logs, then click the Apply Changes button.

![5](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/07f11938-23f9-4ade-aff1-35242d8e3fd6)

Go back one page by clicking in the top menu to Microsoft Sentinel | Data connectors.

Do the same for the Azure Activity data connector by opening the connector page. Scroll down the right panel and under Configuration, click the Launch Azure Policy Assignment Wizard. In the Basics tab, for the scope, click on the ellipsis, select the Subscription and Resource Group.

![6](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/95557e07-7b90-45ed-86d9-49c77151ac40)

Next, go to the Parameters tab. For the Primary Log Analytics workspace, select mySentinel. In the Remediation tab, click the Create a remediation task checkbox. Leave all settings as default and click Review + create, followed by Create.

![7](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/90e324d5-9e18-4df6-b6c8-0d12d4fc0ed0)

All resources will now have their log data sent to Sentinel.

Go back to the Data connectors list. Select Microsoft 365 Defender and open the connector page. Under Configuration, the Connect events list options that allow specification of events to be sent from Microsoft 365 Defender to Microsoft Sentinel. No additional options are selected for the scope of this project.

Go back to the Data connectors list and open the connector page for Windows Security Events via AMA. This data connector uses data collection rules, also known as DCR, to identify and collect logs from Windows hosts. Under Configuration, select the +Create data collection rule button. In the Create Data Collection Rule window, fill in the Rule name (gpWindowsDCR). In the other tabs, the rule can be expanded upon to add resources or select which events to stream for data. For the scope of this project, we leave these as default and click the Next: Review + create button, followed by Create. Click the Refresh button and wait for the rule to show.

![8](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/3d159403-1bb8-414e-89c2-f0f564a30452)

## Enable and Configure Analytic Rules
Having connected the data sources with data connectors, the next step is to activate or create analytic rules to monitor and correlate the data to generate alerts and incidents of suspicious activity. In the Microsoft Sentinel workspace, navigate to the side menu, under Configuration, select Analytics. There is one default active rule in Microsoft Sentinel, the Advanced Multistage Attack Detection rule, which can be edited by scrolling to the right in the list, clicking on the ellipsis and selecting Edit. Back on the Analytics page, select Rule templates, search for and select Scheduled Task Hide. On the panel (select expand if hidden), select Create rule.

![9](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/380658d9-acd2-4414-8e7c-26186b9d8264)

In the Analytics rule wizard, fill in the name (MyScheduledTaskHide). Click on Tactics and techniques, expand the Persistence list, search for and select T1053 which is the scheduled Task/Job. In the Set rule logic tab, under the Query box, click the View query results link. In the Logs window, click the Run button and monitor if the query works properly (no errors). (There won’t be any information to show.) Close the window by clicking Continue Editing Alert.

![10](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/27184fc9-a057-44eb-b5de-856b7c88cf8f)

Still in the Set rule logic tab, under Alert enhancement, expand the Entity mapping section. This section allows specification of return data as an entity. Entities can be correlated across other alerts to see if there are alerts that could possibly be related to an incident. Click next, leaving all other options as default, and Save the Scheduled rule.

![11](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/033ed576-f244-4385-afd0-32567c714b50)

## Create an Automation rule
There are two types of automation to handle repetitive tasks, automation rules and playbooks. Automation rules are easy to create case management level tasks. Playbooks use Azure logic apps to allow the analysts to perform more complex tasks on the incident and related data. In the Microsoft Sentinel workspace, navigate to the side menu, under Configuration, select Automation. In the command bar, select Create new automation rule. Fill in the Automation rule name (MyAutomationRule). Under Actions, select Change severity, and High. This will set the severity of any incident, when it is created, automatically to High. Click on Apply to create the rule.

![12](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/0de6feef-3a47-4ac9-9d35-14f920dbc366)

## Explore MITRE ATT&CK
In the Microsoft Sentinel workspace, navigate to the side menu, under Threat management, select MITRE ATT&CK. Here, you’ll have a grid of all the tactics and techniques used by attackers. Select Scheduled Task/Job and show the hidden panel if needed. 

![13](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/e9b1c866-b395-4fa9-9426-997bf46b04ce)

Click on the View link in the panel to see the active list of rules that have coverage of this tactic and technique.

![14](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/483d1ff8-d65b-4c9c-8137-94c030a58f6a)

## Enable a Workbook
A workbook in Microsoft Sentinel allows you to display list and graphical information about your data. In the Microsoft Sentinel workspace, navigate to the side menu, under Threat management, select Workbooks. From the Templates, select Azure Activity. On the right panel, there is a description of what the workbook is. Select View template to see what is in the workbook. Go back to the workbooks page, select Azure Activity and click the Save button. The workbook will now be saved to the My workbooks tab. So click on the My workbooks tab, select Azure Activity and in the panel, click the View saved workbook button. (Because we are working in a new environment, there is currently no data displayed.)

![15](https://github.com/GeoffreyMorren/Microsoft-Sentinel/assets/152500568/f1f0701e-84b5-4142-b9d7-da78acbfe116)

</div>
