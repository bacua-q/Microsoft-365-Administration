
\> Relation Topics <a href="Archive Online" class="wikilink">ArchiveOnline</a> \| <a href="Retention Policy" class="wikilink">RetentionPolicy</a> \| <a href="Retention Policy - MRM"
class="wikilink">RetentionPolicy-MRM</a> \| <a href="Deleted Retention Policy"
class="wikilink">DeletedRetentionPolicy</a>

1.  1.  In the new EAC, navigate to **Recipients** \> **Mailboxes**.
2.  In the list of mailboxes, select the user to enable their mailbox for archive.
3.  In the flyout pane, select **Others**, and under **Mailbox archive**, select **Manage mailbox archive**

<figure>
<img src="Pasted image 20230815185958.png" class="wikilink"
alt="Pastedimage20230815185958.png" />
<figcaption
aria-hidden="true">Pastedimage20230815185958.png</figcaption>
</figure>

4.  On the **Manage mailbox archive** pane, turn on **Mailbox archive**, and then **Save**.

### Enable Archive Mailbox - Using PowerShell

    Enable-Mailbox -Identity <username> -Archive
    Disable-Mailbox -Identity <username> -Archive

### Auto-Expanding

However, Microsoft 365 provides auto-expanding archiving for Office 365 Enterprise E3 and E5 licenses. This must be enabled before the archive mailbox reaches its maximum size. When auto-expanding archive is enabled, it can take up to 30 days before free space is added to the archive mailbox.

Run Test: https://aka.ms/PillarArchiveMailbox

    # Enable Unlimited Archiving for the Organization 

    Get-OrganizationConfig | FL AutoExpandingArchiveEnabled

    Set-OrganizationConfig -AutoExpandingArchive

    # Enable Unlimited Archiving for a User Mailbox

    Get-Mailbox -Identity bcuadra@support365.cloudns.ph | FL AutoExpandingArchiveEnabled

    Enable-Mailbox bcuadra@support365.cloudns.ph -AutoExpandingArchive

## No archive

Using Assist 365, check ELC Status, with "Display Mail User Overview"

<figure>
<img src="Pasted image 20230919093844.png" class="wikilink"
alt="Pastedimage20230919093844.png" />
<figcaption
aria-hidden="true">Pastedimage20230919093844.png</figcaption>
</figure>

ELC Process should be - FALSE.

<figure>
<img src="Pasted image 20230919094017.png" class="wikilink"
alt="Pastedimage20230919094017.png" />
<figcaption
aria-hidden="true">Pastedimage20230919094017.png</figcaption>
</figure>

1.  In compliance \> Data cycle Management \> Check MRM Policy, and Tag. Verify the is no personal.
    <img src="Pasted image 20230920074739.png" class="wikilink"
    alt="Pastedimage20230920074739.png" />
    <img src="Pasted image 20230920075018.png" class="wikilink"
    alt="Pastedimage20230920075018.png" />

2.  Active Litigation Hold.

<figure>
<img src="Pasted image 20230920075842.png" class="wikilink"
alt="Pastedimage20230920075842.png" />
<figcaption
aria-hidden="true">Pastedimage20230920075842.png</figcaption>
</figure>

Using Assist 365, for verify Litigation Hold Status. "Analyzes M365 mailbox MRM processing status"

<figure>
<img src="Pasted image 20230919103219.png" class="wikilink"
alt="Pastedimage20230919103219.png" />
<figcaption
aria-hidden="true">Pastedimage20230919103219.png</figcaption>
</figure>

3.  In compliance \> Data cycle management \> Microsoft 365 \> Retention policies. Verify that the user is not in a retention policy. Exclude to mailbox.
    <img src="Pasted image 20230920075540.png" class="wikilink"
    alt="Pastedimage20230920075540.png" /><img src="Pasted image 20230920075643.png" class="wikilink"
    alt="Pastedimage20230920075643.png" />
    Wait for the Delayhold police to act. after you remove the user from the hold policy.

<!-- -->

    Set-Mailbox Usuario@Dominio.com -RemoveDelayHoldApplied  

Force archive:

    Start-ManagedFolderAssistant -Identity bcuadra@support365.cloudns.ph

Display process:

    Get-MailboxStatistics -Identity bcuadra@support365.cloudns.ph -Archive | Select DisplayName, TotalItemSize, ItemCount

Verify Archive

<figure>
<img src="MicrosoftTeams-image.png" class="wikilink"
alt="MicrosoftTeams-image.png" />
<figcaption aria-hidden="true">MicrosoftTeams-image.png</figcaption>
</figure>

Until 1.5 TB

[Descripción del servicio de Archivado de Exchange Online - Service Descriptions \| Microsoft Learn](https://learn.microsoft.com/es-es/office365/servicedescriptions/exchange-online-archiving-service-description/exchange-online-archiving-service-description)

[Enable auto-expanding archiving \| Microsoft Learn](https://learn.microsoft.com/en-us/purview/enable-autoexpanding-archiving)''


    Get-MailboxStatistics -Identity bcuadra@support365.cloudns.ph -Archive | Select DisplayName, TotalItemSize, ItemCount

    $logProps = Export-MailboxDiagnosticLogs bcuadra@safe.cloudns.us -ExtendedProperties
    $xmlprops = [xml]($logProps.MailboxLog)
    $xmlprops.Properties.MailboxTable.Property | ? {$_.Name -like "ELC*"}


    Get-MailboxStatistics -Identity bcuadra@safe.cloudns.us -Archive | Select DisplayName, TotalItemSize, ItemCount

    Start-ManagedFolderAssistant -Identity bcuadra@safe.cloudns.us

ElcLastRunDeletedFromRootItemCount  - items from Deleted Items folder that expire and should be moved to Recoverable Items automatically   
ElcLastRunDeletedFromDumpsterItemCount -- items from Recoverable Items folder that are purged  
ElcLastRunArchivedFromRootItemCount - items that are being moved from the primary mailbox Inbox or Top of Information Store, into the archive's Inbox or Top of Information Store  
<font color="#00b050">**ElcLastRunArchivedFromDumpsterItemCount** </font> - items that are being moved from the primary mailbox Recoverable Items folder into the archive mailbox Recoverable Items folder  
<font color="#00b050">ElcLastSuccessTimestamp </font>- the last time when MRM processed the mailbox without encountering any errors; in case of MRM throttling, these errors can be temporary, which means that items will continue to be moved/deleted but at a slower rate than usual.

*\> Autor: Bayardo Cuadra*
