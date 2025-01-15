# Application settings
You can configure the various application settings associated with Hornbill ITOM in Configuration > Hornbill ITOM. This article describes some of the more commonly used settings. This is not a complete list; more settings are visible within Configuration.

## Before you begin
* Be familiar with how to use [Configuration](/esp-config/getting-started/using-configuration).

## Accessing application settings
1. At the bottom of the left-hand menu bar, click the cog icon to open Configuration. (A shortcut is to use CTRL+SHIFT+S on your keyboard.)
1. Select **Hornbill ITOM**.
1. Under *Advanced Tools & Settings*, select **Application Settings**.

## Filters
Above the settings list, there is a quick filter and some other options available to help you find the setting you want.

## Settings
|Name|Description|
|:----|:----|
|`itom.concurrentJobs`|The maximum number of jobs that the SIS server service will process at the same time.|
|`itom.jobHeartbeatTimeout`|The maximum time in seconds allowed if no heartbeat is received from EspSisExec before Hornbill assumes the job has gone AWOL.|
|`itom.maximumConcurrentPendingJobUpdate`|The maximum number of pending job updates that will be processed at the same time.|
|`itom.maximumDeferredJobLifetime`|The maximum time in hours that a deferred job is allowed to remain in the queue.|
|`itom.outputFilters.filterPowerShellCLIXML`|This enables the parsing and reformatting of PowerShell CLIXML output from Windows PowerShell and PowerShell Core package operations.|
|`itom.reservedNonIdleCapacity`|The minimum amount of available concurrent jobs required so that a job with Idle priority gets started.|