---
# required metadata

title: Set Advanced Threat Analytics notifications | Microsoft Docs
description: Describes how to set ATA alerts so you are notified when suspicious activities are detected.
keywords:
author: rkarlin
ms.author: rkarlin
manager: mbaldwin
ms.date: 11/7/2017
ms.topic: article
ms.prod:
ms.service: advanced-threat-analytics
ms.technology:
ms.assetid: 14cb7513-5dc8-49cb-b3e0-94f469c443dd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: bennyl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

*Applies to: Advanced Threat Analytics version 1.8*



# Set ATA Notifications
ATA can notify you when it detects a suspicious activity, either by email or by using ATA event forwarding and forwarding the event to your SIEM/syslog server. Before selecting which notifications you want to receive, you have to [set up your email server and your Syslog server](setting-syslog-email-server-settings.md).

> [!NOTE]
> -   Email notifications include a link that takes the user directly to the suspicious activity that was detected. The host name portion of the link is taken from the setting of the ATA Console URL on the ATA Center page. By default, the ATA Console URL is the IP address selected during the installation  of the ATA Center. If you are going to configure email notifications, it is recommended to use an FQDN as the ATA Console URL.
> -   Notifications are sent from the ATA Center to either the SMTP server and the Syslog server.


To receive notifications, set the following parameters:


1. In the ATA Console, select the settings option on the toolbar and select **Configuration**.

![ATA configuration settings icon](media/ATA-config-icon.png)

2. Under the **Notifications & Reports** section, select **Notifications**.
3. Under **Mail notifications**, specify which notifications should be sent via email - new suspicious activities and new health issues. You can set a separate email address for the suspicious activities to be sent to and for the health alerts so that, for example, suspicious activity notifications can be sent to your security analyst and your health alert notifications can be sent to your IT admin.
>	[!NOTE]
>   Email alerts for suspicious activities are only sent when the suspicious activity is created.
3. Under **Syslog notifications**, specify which notifications should be sent to your Syslog server - new suspicious activities, updated suspicious activities, and new health issues.
5. Click **Save**.

![ATA mail notification settings image](media/ata-mail-notification-settings.png)




## See Also
[Check out the ATA forum!](https://social.technet.microsoft.com/Forums/security/home?forum=mata)
