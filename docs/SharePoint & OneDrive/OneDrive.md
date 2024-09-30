
## Find a list of all ODB sites in the tenant:

`Get-SPOSite -IncludePersonalSite $True -Limit All -Filter "Url -like '-`
`my.sharepoint.com/personal/'" | Select-Object Owner, Url | Sort-Object Owner`


## Find a list of M365 group or team URL

`Get-UnifiedGroup -Identity MyGroup | Select-Object SharePointSiteURL

