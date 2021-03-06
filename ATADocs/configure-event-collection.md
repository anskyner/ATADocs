---
# required metadata

title: Configure Windows Event Forwarding in Advanced Threat Analytics | Microsoft Docs
description: Describes your options for configuring Windows Event Forwarding with ATA
keywords:
author: rkarlin
ms.author: rkarlin
manager: mbaldwin
ms.date: 11/7/2017
ms.topic: get-started-article
ms.prod:
ms.service: advanced-threat-analytics
ms.technology:
ms.assetid: 3f0498f9-061d-40e6-ae07-98b8dcad9b20

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



# Configuring Windows Event Forwarding

> [!NOTE]
> For ATA versions 1.8 and higher, event collection configuration is no longer necessary for ATA Lightweight Gateways. The ATA Lightweight Gateway can now read events locally, without the need to configure event forwarding.


To enhance detection capabilities, ATA needs the following Windows events: 4776, 4732, 4733, 4728, 4729, 4756, 4757. These can either be read automatically by the ATA Lightweight Gateway or in case the ATA Lightweight Gateway is not deployed, it can be forwarded to the ATA Gateway in one of two ways, by configuring the ATA Gateway to listen for SIEM events or by configuring Windows Event Forwarding.



### WEF configuration for ATA Gateway's with port mirroring

After you configured port mirroring from the domain controllers to the ATA Gateway, follow the following instructions to configure Windows Event forwarding using Source Initiated configuration. This is one way to configure Windows Event forwarding. 

**Step 1: Add the network service account to the domain Event Log Readers Group.** 

In this scenario, assume that the ATA Gateway is a member of the domain.

1.	Open Active Directory Users and Computers, navigate to the **BuiltIn** folder and double-click **Event Log Readers**. 
2.	Select **Members**.
4.	If **Network Service** is not listed, click **Add**, type **Network Service** in the **Enter the object names to select** field. Then click **Check Names** and click **OK** twice. 

After adding the **Network Service** to the **Event Log Readers** group, reboot the domain controllers for the change to take effect.

**Step 2: Create a policy on the domain controllers to set the Configure target Subscription Manager setting.** 
> [!Note] 
> You can create a group policy for these settings and apply the group policy to each domain controller monitored by the ATA Gateway. The steps below modify the local policy of the domain controller. 	

1.	Run the following command on each domain controller: *winrm quickconfig*
2.  From a command prompt type *gpedit.msc*.
3.	Expand **Computer Configuration > Administrative Templates > Windows Components > Event Forwarding**

 ![Local policy group editor image](media/wef 1 local group policy editor.png)

4.	Double-click **Configure target Subscription Manager**.
   
    1.	Select **Enabled**.
    2.	Under **Options**, click **Show**.
    3.	Under **SubscriptionManagers**, enter the following value and click **OK**:	*Server=http://<fqdnATAGateway>:5985/wsman/SubscriptionManager/WEC,Refresh=10* (For example: Server=http://atagateway9.contoso.com:5985/wsman/SubscriptionManager/WEC,Refresh=10)
 
   ![Configure target subscription image](media/wef 2 config target sub manager.png)
   
    5.	Click **OK**.
    6.	From an elevated command prompt type *gpupdate /force*. 

**Step 3: Perform the following steps on the ATA Gateway** 

1.	Open an elevated command prompt and type *wecutil qc*
2.	Open **Event Viewer**. 
3.	Right-click **Subscriptions** and select **Create Subscription**. 

   1.	Enter a name and description for the subscription. 
   2.	For **Destination Log**, confirm that **Forwarded Events** is selected. For ATA to read the events, the destination log must be **Forwarded Events**. 
   3.	Select **Source computer initiated** and click **Select Computers Groups**.
        1.	Click **Add Domain Computer**.
        2.	Enter the name of the domain controller in the **Enter the object name to select** field. Then click **Check Names** and click **OK**. 
       
        ![Event Viewer image](media/wef3 event viewer.png)
   
        
        3.	Click **OK**.
   4.	Click **Select Events**.

        1. Click **By log** and select **Security**.
        2. In the **Includes/Excludes Event ID** field type the event number and click **OK**. For example, type 4776, like in the following sample.

 ![Query filter image](media/wef 4 query filter.png)

   5.	Right-click the created subscription and select **Runtime Status** to see if there are any issues with the status. 
   6.	After a few minutes, check to see that the events you set to be forwarded is showing up in the Forwarded Events on the ATA Gateway.


For more information, see: [Configure the computers to forward and collect events](https://technet.microsoft.com/library/cc748890)

## See Also
- [Install ATA](install-ata-step1.md)
- [Check out the ATA forum!](https://social.technet.microsoft.com/Forums/security/home?forum=mata)
