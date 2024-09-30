## Adding SharePoint Sites for Teams Private and Shared Channels

* Teams private and shared channels keep their conversations and files separate from the other parts of the team to ensure that only channel members can access this content
* Each private or shared channel has a dedicated SharePoint site, which is a child of the site owned by the team

* **Private channel** - search the parent team 
* **Shared channel** - add one or more members of the channel to search the compliance records stored in the personal mailboxes

* to add the SharePoint site belonging to a private or shared channel to a content search, you need to know its URL
	* open the channel in Teams and use the “Open in SharePoint” option in the Files tab to open the channel site
	* then you can copy the URL shown in the browser navigation bar



## How to use Targeted Collections with Microsoft 365 Content Searches

https://practical365.com/targeted-collection-content-search/


## Auditing of Search Activities

$StartDate = (Get-Date).AddDays(-7) ; $EndDate = Get-Date
$Records = (Search-UnifiedAuditLog -StartDate $StartDate -EndDate $EndDate -Operations 
"SearchExportDownloaded", "SearchViewed", "ViewedSearchPreviewed" -ResultSize 1000)
If (!($Records)) {
 Write-Host "No audit records for content search activities found." }
Else {
 Write-Host "Processing" $Records.Count "audit records..."
 $Report = [System.Collections.Generic.List[Object]]::new()
 ForEach ($Rec in $Records) {
 $AuditData = ConvertFrom-Json $Rec.Auditdata
 $ReportLine = [PSCustomObject]@{
 TimeStamp = Get-Date ($Rec.CreationDate) -format g
 User = $AuditData.UserId
 Action = $AuditData.Operation
 Exchange = $AuditData.ExchangeLocations
 SharePoint = $AuditData.SharePointLocations
 Query = $AuditData.Query }
 $Report.Add($ReportLine)
 }}
$Report | Export-Csv c:\temp\SearchAuditRecords.csv -NoTypeInformation

## Microsoft Purview eDiscovery

