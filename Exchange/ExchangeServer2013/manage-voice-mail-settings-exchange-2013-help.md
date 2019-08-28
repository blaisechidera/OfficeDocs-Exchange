---
title: 'Manage voice mail settings for a user: Exchange 2013 Help'
TOCTitle: Manage voice mail settings for a user
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.date: 11/17/2014
ms.reviewer: 
ms.assetid: 73957938-048a-4f9c-bd0f-a3c2c3dcd638
mtps_version: v=EXCHG.150
---

# Manage voice mail settings for a user in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can view or set the Unified Messaging (UM) and voice mail features and configuration settings for a user that's been enabled for UM and voice mail. For example, you can do the following:

- Reset their Outlook Voice Access PIN.

- Add a personal operator extension number.

- Add other extension numbers.

- Enable or disable Automatic Speech Recognition (ASR).

- Enable or disable Call Answering Rules.

- Enable or disable access to their email or calendar.

> [!NOTE]
> Some of the settings and features can only be configured by using the Shell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging Permissions](http://technet.microsoft.com/library/d326c3bc-8f33-434a-bf02-a83cc26a5498.aspx) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the existing user is currently enabled for Unified Messaging. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to view or configure a UM-enabled user's properties

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox for which you want to change the UM mailbox policy.

3. In the details pane, under **Phone and Voice Features** \> **Unified Messaging**, click **View details**.

4. On the **UM Mailbox** page, click **UM mailbox settings** to view or change the following UM properties for an existing UM-enabled user:

   - **PIN Status**: This display-only field shows the status of the user's mailbox. By default, when a user is UM-enabled, the PIN status is listed as **Not locked out**. However, if the user has input an incorrect Outlook Voice Access PIN multiple times, the status is listed as **Locked Out**.

   - **UM mailbox policy**: This box shows the name of the UM mailbox policy associated with the UM-enabled user. You can click **Browse** to locate and specify the UM mailbox policy to be associated with this UM mailbox.

   - **Personal operator extension**: Use this box to specify the operator extension number for the user. By default, an extension number isn't configured. The length of the extension number can be from 1 through 20 characters. This enables incoming calls for the UM-enabled user to be forwarded to the extension number that you specify in this box.

     You can configure other types of operator extension numbers on dial plans and auto attendants. However, those extensions are generally meant for company-wide receptionists or operators. The personal operator extension setting could be used when an administrative assistant or personal assistant answers incoming calls before they're answered for a particular user.

5. On the **UM Mailbox** page, under **Other extensions**, you can add, change, and view extension numbers for the user.

   - To add an extension number, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). On the **Add another extension** page, use **Browse** to select the UM dial plan, and then enter the extension number in the **Extension number** box.

   - To remove an extension number, select the extension number you want to remove, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

6. If you make any changes, click **Save**.

## Use the Shell to configure features for a UM-enabled user

This example disables Play on Phone and missed call notifications, but enables text message (SMS) notifications.

> [!NOTE]
> For on-premises and hybrid deployments, when you're integrating Unified Messaging and Lync Server, missed call notifications aren't available to users who have a mailbox located on an Exchange 2007 or Exchange 2010 Mailbox server. A missed call notification is generated when a user disconnects before the call is sent to a Mailbox server.

```powershell
Set-UMMailbox -Identity tony@contoso.com -UMEnabled $true -UMMailboxPolicy AdminPolicy -MissedCallNotificationEnabled $false -PlayonPhoneEnabled $false -SMSMessageWaitingNotificationEnabled $true
```

This example prevents a user from accessing the calendar, but enables access to email when the user is using Outlook Voice Access.

```powershell
Set-UMMailbox -Identity tony@contoso.com -UMEnabled $true -UMMailboxPolicy AdminPolicy -Extension 523456 -FAXEnabled $true -TUIAccessToCal $false -TUIAccessToEmail True
```

This example prevents a user from accessing the calendar and email when the user is using Outlook Voice Access.

```powershell
Set-UMMailbox -Identity tony@contoso.com -TUIAccessToCalendarEnabled $false -TUIAccessToEmailEnabled $false
```

This example prevents a user from creating call answering rules, receiving incoming faxes, and using Outlook Voice Access, but enables Automatic Speech Recognition (ASR).

```powershell
Set-UMMailbox -Identity tony@contoso.com -AutomaticSpeechRecognitionEnabled $true -CallAnsweringRulesEnabled $false -FaxEnabled $false -SubscriberAccessEnabled $false
```

## Use the Shell to view a UM-enabled user's properties

This example displays a list of all the UM-enabled mailboxes in the forest in a formatted list.

```powershell
Get-UMMailbox | Format-List
```

This example displays the UM mailbox properties for tonysmith@contoso.com.

```powershell
Get-UMMailbox -Identity tonysmith@contoso.com
```

> [!IMPORTANT]
> When you're running Exchange 2007 and Exchange 2013 and the user's mailbox is located on an Exchange 2007 Mailbox server, running the **Get-UMMailbox** cmdlet won't work correctly. To resolve the issue, run the **Get-UMMailbox** cmdlet from an Exchange 2007 server or a computer running the Exchange 2007 administrative tools.