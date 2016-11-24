\\STEP 1:
\\LOAD IN FOLLOWING:    

    [DSCLocalConfigurationManager()]
        configuration LCMConfig
        {
            Node localhost
            {
                Settings
                {
                    RefreshMode = 'Push'
                    RebootNodeIfNeeded = $true
                    ConfigurationMode =  'ApplyAndAutoCorrect'
                }
            }
        }
    

\\STEP 2: 
\\EXECUTE LCMConfig:

    Set-DscLocalConfigurationManager -Path ".\" -Verbose -Force

\\STEP 3:
\\LOAD IN FOLLOWING:

    configuration DscConfig
    {
    
        Import-DscResource –ModuleName 'PSDesiredStateConfiguration'
        # One can evaluate expressions to get the node list
        # E.g: $AllNodes.Where("Role -eq Web").NodeName
        node localhost
        {
            
    
            Service BitsService
            {
                Name = "BITS"
                State = 'Running'
                StartupType = 'Automatic' 
                Ensure = "Present"
    
            }
    
            WindowsFeature SnmpFeature
            {
                Name = "SNMP-Service"
                Ensure =  "Present"
            }
    
            WindowsFeature TelnetFeature
            {
                Name = "Telnet-Client"
                Ensure =  "Present"
            }
    
            File file
            {
                Type =  'Directory'
                DestinationPath = "C:\temp"
                Ensure =  "Present"  
                  
    
            }       
        }
    
    
    }

\\STEP 4 
\\EXECUTE DscConfig:

    Start-DscConfiguration -Path ".\" -Wait -Verbose -Force