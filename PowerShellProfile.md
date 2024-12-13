
```powershell
# New-Item -Path $PROFILE -Type File -Force

clear

$FormatEnumerationLimit = -1 

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

function date { (Get-Date).ToShortDateString() }


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

```
