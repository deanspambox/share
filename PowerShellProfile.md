
```powershell
clear

# Set-ExecutionPolicy unrestricted
# Install-Module PSColor
Import-Module PSColor


$FormatEnumerationLimit = -1 

# Set some command Defaults 

$PSDefaultParameterValues = @{
    "*:autosize"                    = $true 
    'Receive-Job:keep'              = $true 
    '*:Wrap'                        = $true
    "*-Csv:Delimiter"               = ";"
    "*-Csv:NoTypeInformation"       = $true
    "*-Csv:NoClobber"               = $true
  } 

#Alias
Set-Alias gh    Get-Help 

function date { (Get-Date).ToShortDateString() }
function time { Get-Date -Format 'HH:mm:ss' }
function uptime { ((get-date) - (gcim Win32_OperatingSystem).LastBootUptime).ToString() }

# Write banner
Write-Host 'Tls loaded: 	' $([System.Net.SecurityProtocolType]::Tls12) -ForegroundColor Cyan 
Write-Host 'HostName: 	' $($env:COMPUTERNAME) -ForegroundColor Cyan
Write-Host 'PS version: 	' $($PSVersionTable.PSVersion.ToString()) -ForegroundColor Cyan 
Write-Host 'Date: 		' $(date) -ForegroundColor Cyan 


Set-Location c:\REPO

function prompt {
    $curPath = (Get-Location).Path
    $curTime = Get-Date -Format 'HH:mm:ss' 
    Write-Host $curTime -ForegroundColor Blue -NoNewline
    Write-Host ' ' -NoNewline
    Write-Host $curPath -ForegroundColor Green -NoNewline
    Write-Host '>' -NoNewline
    return ' '
}



#function prompt {
#Write-Host ($(pwd).path +">") -nonewline -foregroundcolor Yellow
#return " "
#} 

function tail {
  param($Path, $n = 10, [switch]$f = $true)
  Get-Content $Path -Tail $n -Wait:$f
}

# Compute file hashes - useful for checking successful downloads -------------------------------------------------
function md5 { Get-FileHash -Algorithm MD5 $args | FL }
function sha1 { Get-FileHash -Algorithm SHA1 $args | FL }
function sha256 { Get-FileHash -Algorithm SHA256 $args | FL }

$global:PSColor = @{
    File = @{
        Default    = @{ Color = 'White' }
        Directory  = @{ Color = 'Cyan'}
        Hidden     = @{ Color = 'DarkGray'; Pattern = '^\.' } 
        Code       = @{ Color = 'Magenta'; Pattern = '\.(java|c|cpp|cs|js|css|html)$' }
        Executable = @{ Color = 'Red'; Pattern = '\.(exe|bat|cmd|py|pl|ps1|psm1|vbs|rb|reg)$' }
        Text       = @{ Color = 'Yellow'; Pattern = '\.(txt|cfg|conf|ini|csv|log|config|xml|yml|md|markdown)$' }
        Compressed = @{ Color = 'Green'; Pattern = '\.(zip|tar|gz|rar|jar|war)$' }
    }
    Service = @{
        Default = @{ Color = 'White' }
        Running = @{ Color = 'DarkGreen' }
        Stopped = @{ Color = 'DarkRed' }     
    }
    Match = @{
        Default    = @{ Color = 'White' }
        Path       = @{ Color = 'Cyan'}
        LineNumber = @{ Color = 'Yellow' }
        Line       = @{ Color = 'White' }
    }
	NoMatch = @{
        Default    = @{ Color = 'White' }
        Path       = @{ Color = 'Cyan'}
        LineNumber = @{ Color = 'Yellow' }
        Line       = @{ Color = 'White' }
    }
}

#E.g. if you would like to override the red color for Executables, in favour of blue; you could add the following to your PowerShell profile:
$global:PSColor.File.Executable.Color = 'Blue'

```
