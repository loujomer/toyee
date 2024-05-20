

### Expand multi-valued properties 
`| Select-Object -Property *property* -ExpandProperty *property*

### Get a list of property members
`| Get-Member -MemberType property


### Filter property values

`| Where-Object -Property *property* -eq "value"`
