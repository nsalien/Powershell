
Clone TasmanianTraders-IaC-ActiveDirectory

Change azuredeploy.parameters.json suiting your environment

    $ cd C:\Users\xyz\Documents\TasmanianTraders-IaC-ActiveDirectory 
    //folder containing cloned TasmanianTraders-IaC-ActiveDirectory files
    
    $ New-AzureRmResourceGroup -Name ns-iac-addc-dev -Location "North Europe"
    
    $ New-AzureRmResourceGroupDeployment -ResourceGroupName ns-iac-addc-dev -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json