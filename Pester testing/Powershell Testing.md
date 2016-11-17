BAD:
```JSON
{
 "CPU" : 20,
 "DOMAINFQDN" : "testdomain2.local",
 "ENVIRONNMENT" : "PRD",
 "IPADRESS" : "10.134.800.22",
 "MEMORYGB" : 6,
 "VMNAME" : "testVM"

}
```
GOOD:
```JSON
{
 "CPU" : 2,
 "DOMAINFQDN" : "testdomain.local",
 "ENVIRONMENT" : "PRD",
 "IPADDRESS" : "10.134.8.22",
 "MEMORYGB" : 4,
 "VMNAME" : "testVMPRD01"

}
```
```Powershell

$good = (Get-Content VMDeployGood.json) | ConvertFrom-Json | gm -MemberType NoteProperty | select -exp Name
$bad = "CPU","DOMAINFQDN","ENVIRONMENT" , "IPADDRESS", "MEMORYGB", "VMNAME"
$a = Compare-Object $good $bad | where -Property SideIndicator -Like '=>'

$good = Get-Content VMDeployGood.JSON | ConvertFrom-Json


Describe "TestJSONGood" {

    It "Check Memory" {
         $good.MEMORYGB | should BeLessThan 5
    }

    It "Check Domain" {
         $good.DOMAINFQDN | should be "testdomain.local"
    }

    It "Check Environment" {
        $good.ENVIRONMENT | should be "PRD"

    }

    It "Check CPU" {
        $good.CPU | should BeLessThan 4
    }

    It "Check IP" {
        [IPAddress]$good.IPADDRESS | should be $true
    }

    It "Check name" {
        $good.VMNAME.Contains($good.ENVIRONMENT) | should be $true
    }

    It "Check property" {
        $a | should BeNullOrEmpty
    }

    
}


$good = (Get-Content VMDeployBad.json) | ConvertFrom-Json | gm -MemberType NoteProperty | select -exp Name
$bad = "CPU","DOMAINFQDN","ENVIRONMENT" , "IPADDRESS", "MEMORYGB", "VMNAME"
$b = Compare-Object $good $bad | where -Property SideIndicator -Like '=>'

$bad = Get-Content VMDeployBad.JSON | ConvertFrom-Json

Describe "TestJSONBad" {

    It "Check Memory" {
         $bad.MEMORYGB | should BeLessThan 5
    }

    It "Check Domain" {
         $bad.DOMAINFQDN | should be "testdomain.local"
    }

    It "Check Environment" {
        $bad.ENVIRONMENT | should be "DVL"

    }

    It "Check CPU" {
        $bad.CPU | should BeLessThan 4
    }

    It "Check IP" {
        [IPAddress]$bad.IPADDRESS | should be $true
    }

    It "Check name" {
        $bad.VMNAME.Contains($bad.ENVIRONNMENT) | should be $true
    }

    It "Check" {
        $b | Should BeNullOrEmpty
    }

    
}


```