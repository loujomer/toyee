
## Find a list of all ODB sites in the tenant:

`Get-SPOSite -IncludePersonalSite $True -Limit All -Filter "Url -like '-`
`my.sharepoint.com/personal/'" | Select-Object Owner, Url | Sort-Object Owner`




