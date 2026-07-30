---
name: powershell-windows-master
description: "PowerShell Windows programming — PS7+ on Windows. Critical syntax rules, patterns, performance, security, modules, and testing. Windows-only, no cross-platform content. Works with any LLM and any harness/agent that loads instruction files."
risk: low
source: unified
date_added: "2026-05-29"
---

# PowerShell Windows Master

> Complete PowerShell expertise for Windows. Covers PS7+ features with 5.1 notes where required.
>
> **Harness-agnostic:** this skill is plain Markdown with explicit rules. It works with any LLM (Claude, GPT, Gemini, etc.) and any harness/agent that can load instruction/context files (Claude Code, Cursor, OpenCode, Cline, Aider, Codex, Copilot Chat, manual prompts). No API, plugin, or runtime dependency.

---

## 1. CRITICAL SYNTAX RULES

### 1.1 Parentheses with Logical Operators

| Wrong | Correct |
|-------|---------|
| `if (Test-Path "a" -or Test-Path "b")` | `if ((Test-Path "a") -or (Test-Path "b"))` |
| `if (Get-Item $x -and $y -eq 5)` | `if ((Get-Item $x) -and ($y -eq 5))` |

**Rule:** Every cmdlet call MUST be wrapped in parentheses when using logical operators (`-and`, `-or`, `-not`).

### 1.2 ASCII Only (No Unicode/Emoji)

| Purpose | Do NOT use | Use instead |
|---------|------------|-------------|
| Success | ✅ ✓ | [OK] [+] |
| Error | ❌ ✗ 🔴 | [!] [X] |
| Warning | ⚠️ 🟡 | [*] [WARN] |
| Info | ℹ️ 🔵 | [i] [INFO] |
| Progress | ⏳ | [...] |

**Rule:** Use ASCII characters only in PowerShell scripts.

### 1.3 Null Check Before Access

| Wrong | Correct |
|-------|---------|
| `$array.Count -gt 0` | `$array -and $array.Count -gt 0` |
| `$text.Length` | `if ($text) { $text.Length }` |

### 1.4 String Interpolation with Complex Expressions

```powershell
# Bad: nested expressions inside strings
"Value: $($obj.prop.sub.nested)"

# Good: store in variable first
$value = $obj.prop.sub.nested
"Value: $value"
```

### 1.5 JSON with -Depth (CRITICAL)

| Wrong | Correct |
|-------|---------|
| `ConvertTo-Json` | `ConvertTo-Json -Depth 10` |

**Rule:** Always specify `-Depth` for nested objects.

---

## 2. SCRIPT TEMPLATE

```powershell
#Requires -Version 7.0
#Requires -RunAsAdministrator  # Remove if elevation is not needed

<#
.SYNOPSIS
    Short description of the script.
.DESCRIPTION
    Detailed description.
.PARAMETER Name
    Parameter description.
.EXAMPLE
    PS> .\script.ps1 -Name "value"
.NOTES
    Author: Name
    Version: 1.0.0
    Date: 2026-05-29
#>

[CmdletBinding()]
param(
    [Parameter(Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$Name,

    [Parameter()]
    [int]$Count = 1
)

$ErrorActionPreference = "Stop"
Set-StrictMode -Version Latest

$ScriptDir = $PSScriptRoot  # Current script directory

try {
    Write-Verbose "Starting script"

    # Main logic here

    Write-Output "[OK] Done"
    exit 0
}
catch {
    Write-Warning "[!] Error: $_"
    exit 1
}
finally {
    # Resource cleanup
}
```

---

## 3. VARIABLES, TYPES AND OPERATORS

### 3.1 Variables

```powershell
# Dynamic typing (default)
$string    = "Hello"
$number    = 42
$array     = @(1, 2, 3)
$hashtable = @{Name="John"; Age=30}

# Strong typing
[string]$name = "John"
[int]$age     = 30
[datetime]$dt = Get-Date

# Special variables
$PSScriptRoot    # Directory containing the script
$PSCommandPath   # Full path to the script
$_               # Current pipeline object
$args            # Script arguments
$Error[0]        # Last error
$env:USERPROFILE # Windows environment variables
$env:COMPUTERNAME
$env:USERNAME
```

### 3.2 Operators

```powershell
# Comparison
-eq   # Equal
-ne   # Not equal
-gt   # Greater than
-lt   # Less than
-ge   # Greater or equal
-le   # Less or equal
-match    # Regex match
-like     # Wildcard (* ?)
-contains # Array contains element
-in       # Element is in array

# Logical
-and  -or  -not  !

# PS7+ modern operators
$result = $condition ? "yes" : "no"  # Ternary
$value  = $null ?? "default"         # Null-coalescing
$len    = $string?.Length            # Null-conditional

# Pipeline chain operators (PS7+)
npm install && npm run build
Get-Item "file.txt" || Write-Error "Not found"
```

---

## 4. CONTROL FLOW

```powershell
# If-ElseIf-Else
if ($condition) {
    # code
} elseif ($otherCondition) {
    # code
} else {
    # code
}

# Switch
switch ($value) {
    1            { "One" }
    2            { "Two" }
    {$_ -gt 10} { "Greater than 10" }
    default      { "Other" }
}

# Switch with regex (case-insensitive by default)
switch -Regex ($string) {
    "^error" { "Starts with error" }
    "ok$"    { "Ends with ok" }
}

# Foreach
foreach ($item in $collection) {
    # process $item
}

# Classic for
for ($i = 0; $i -lt 10; $i++) {
    # code
}

# While / Do-While
while ($condition) { }
do { } while ($condition)
```

---

## 5. ADVANCED FUNCTIONS

```powershell
function Get-Something {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter()]
        [ValidateRange(1, 100)]
        [int]$Count = 1,

        [Parameter()]
        [ValidateSet("A", "B", "C")]
        [string]$Option,

        [Parameter(ValueFromPipeline=$true)]
        [string[]]$InputObject
    )

    begin {
        # Initialization (runs once)
        $results = [System.Collections.Generic.List[object]]::new()
    }

    process {
        # Runs for each pipeline object
        foreach ($item in $InputObject) {
            $results.Add($item)
        }
    }

    end {
        # Finalization (runs once)
        return $results
    }
}
```

---

## 6. PIPELINE AND FILTERING

```powershell
# Basic
Get-Process | Where-Object { $_.CPU -gt 100 } | Select-Object Name, CPU

# Simplified syntax (PS 3.0+)
Get-Process | Where CPU -gt 100 | Select Name, CPU

# ForEach-Object
Get-ChildItem | ForEach-Object { Write-Host $_.Name }

# Group / Sort / Measure
Get-Process | Group-Object ProcessName
Get-Service | Sort-Object Status -Descending
Get-ChildItem | Measure-Object -Property Length -Sum

# Calculated properties
Get-Process | Select-Object Name, @{N="CPU_MB"; E={[math]::Round($_.CPU,2)}}
```

---

## 7. ERROR HANDLING

### 7.1 ErrorActionPreference

| Value | When to Use |
|-------|-------------|
| `Stop` | Development (fail fast), `try/catch` |
| `Continue` | Production, non-critical errors expected |
| `SilentlyContinue` | When error is expected and can be ignored |

### 7.2 Try/Catch/Finally

```powershell
try {
    Get-Content -Path "file.txt" -ErrorAction Stop
}
catch [System.IO.FileNotFoundException] {
    Write-Warning "[!] File not found"
}
catch [System.UnauthorizedAccessException] {
    Write-Warning "[!] Access denied"
}
catch {
    Write-Error "[!] Unexpected error: $_"
    throw  # Re-throw if needed
}
finally {
    # Always executes (resource cleanup)
}
```

**Rules:**
- Do not return inside `try` — return after the block
- Use `finally` to close connections, clean up temp files
- Use `throw` to propagate the error upward

---

## 8. PATHS AND FILE SYSTEM (WINDOWS)

### 8.1 Path Rules

| Pattern | Use |
|---------|-----|
| Literal | `C:\Users\User\file.txt` |
| With variable | `Join-Path $env:USERPROFILE "file.txt"` |
| Relative to script | `Join-Path $PSScriptRoot "data"` |
| Temporary | `$env:TEMP`, `$env:TMP` |

```powershell
# Build paths (use Join-Path — avoids double-slash errors)
$path = Join-Path $env:USERPROFILE "Documents\Reports"
$file = Join-Path $PSScriptRoot "config\settings.json"

# Check existence
if (Test-Path $path) { }
if (Test-Path $path -PathType Leaf)      { }  # File only
if (Test-Path $path -PathType Container) { }  # Directory only
```

### 8.2 File Cmdlets

```powershell
Get-ChildItem -Path C:\Logs -Filter *.log -Recurse
New-Item -Path "C:\temp\new.txt"  -ItemType File
New-Item -Path "C:\temp\folder"   -ItemType Directory
Remove-Item -Path "C:\temp\old.txt" -Force
Copy-Item -Path "source.txt" -Destination "dest.txt"
Move-Item -Path "source.txt" -Destination "dest.txt"
Rename-Item -Path "old.txt" -NewName "new.txt"

# Read and write
Get-Content -Path "file.txt" -Raw           # Entire content as string
Get-Content -Path "file.txt"                # Array of lines
Set-Content -Path "file.txt" -Value $text -Encoding UTF8
Add-Content -Path "file.txt" -Value $newLine
```

---

## 9. PERFORMANCE AND PARALLELISM

### 9.1 Parallel ForEach (PS 7+)

```powershell
# Parallel processing — ideal for I/O bound tasks
1..50 | ForEach-Object -Parallel {
    Start-Sleep -Milliseconds 200
    "Processed: $_"
} -ThrottleLimit 10  # Tune based on CPU/IO

# Share variables with $using:
$prefix = "Item"
1..20 | ForEach-Object -Parallel {
    "$using:prefix $_"
} -ThrottleLimit 5
```

### 9.2 ArrayList vs Array (Dynamic Collections)

```powershell
# BAD: array with += (recreates array on every addition)
$array = @()
foreach ($i in 1..1000) { $array += $i }  # SLOW

# GOOD: generic List
$list = [System.Collections.Generic.List[object]]::new()
foreach ($i in 1..1000) { $list.Add($i) }  # FAST

# GOOD: ArrayList (when type varies)
$list = [System.Collections.ArrayList]::new()
[void]$list.Add($item)  # [void] suppresses index return
```

### 9.3 Efficient Filtering

```powershell
# Use -Filter on the cmdlet (faster than Where-Object)
Get-ChildItem -Path C:\ -Filter *.log -Recurse

# Use .NET methods for large files
[System.IO.File]::ReadLines("large.txt") | Where-Object { $_ -match "error" }

# Avoid calling cmdlets inside loops — prefer pipeline or .NET methods
```

---

## 10. DATA OPERATIONS

### 10.1 JSON

```powershell
# Read JSON
$data = Get-Content "config.json" -Raw | ConvertFrom-Json

# Write JSON (always use -Depth for nested objects)
$data | ConvertTo-Json -Depth 10 | Out-File "config.json" -Encoding UTF8

# Create object and serialize
$obj = [PSCustomObject]@{
    Name  = "John"
    Age   = 30
    Tags  = @("admin", "dev")
}
$json = $obj | ConvertTo-Json -Depth 5
```

### 10.2 CSV

```powershell
# Import
$data = Import-Csv -Path "report.csv" -Delimiter ";"

# Export
$data | Export-Csv -Path "output.csv" -NoTypeInformation -Delimiter ";" -Encoding UTF8
```

### 10.3 Hashtable and PSCustomObject

```powershell
# Hashtable
$ht = @{
    Key1 = "value1"
    Key2 = 42
}
$ht["Key1"]
$ht.Key1

# PSCustomObject (better for pipeline and Select-Object)
$obj = [PSCustomObject]@{
    Name   = "Server01"
    Status = "Online"
    CPU    = 45
}
```

---

## 11. WINDOWS SYSTEM MANAGEMENT

### 11.1 Processes

```powershell
Get-Process
Get-Process -Name "notepad"
Stop-Process -Name "notepad" -Force
Start-Process "notepad.exe" -Wait
Start-Process "cmd.exe" -ArgumentList "/c dir" -NoNewWindow -Wait
```

### 11.2 Services

```powershell
Get-Service
Get-Service -Name "Spooler"
Start-Service   -Name "Spooler"
Stop-Service    -Name "Spooler" -Force
Restart-Service -Name "Spooler"
Set-Service     -Name "Spooler" -StartupType Automatic
```

### 11.3 Windows Registry

```powershell
# Read value
Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" -Name "ProductName"

# Create/modify value
Set-ItemProperty -Path "HKCU:\Software\MyApp" -Name "Config" -Value "value"

# Create key
New-Item -Path "HKCU:\Software\MyApp" -Force

# Check existence
Test-Path "HKCU:\Software\MyApp"
```

### 11.4 Network

```powershell
Test-Connection    -ComputerName "8.8.8.8" -Count 2
Test-NetConnection -ComputerName "server01" -Port 443

# WMI / CIM (Windows)
Get-CimInstance -ClassName Win32_OperatingSystem
Get-CimInstance -ClassName Win32_NetworkAdapterConfiguration | Where-Object IPEnabled
```

---

## 12. REST API AND WEB

```powershell
# GET
$response = Invoke-RestMethod -Uri "https://api.example.com/data" -Method Get

# POST with JSON
$body = @{ name = "John"; age = 30 } | ConvertTo-Json
$response = Invoke-RestMethod -Uri "https://api.example.com/users" `
    -Method Post `
    -Body $body `
    -ContentType "application/json"

# With Bearer auth headers
$headers = @{
    "Authorization" = "Bearer $token"
    "Accept"        = "application/json"
}
$response = Invoke-RestMethod -Uri $url -Headers $headers -ErrorAction Stop

# Download file
Invoke-WebRequest -Uri $url -OutFile "C:\Temp\file.zip"

# Simple retry
$attempts = 3
for ($i = 1; $i -le $attempts; $i++) {
    try {
        $resp = Invoke-RestMethod -Uri $url -ErrorAction Stop
        break
    }
    catch {
        if ($i -eq $attempts) { throw }
        Start-Sleep -Seconds ($i * 2)
    }
}
```

---

## 13. SECURITY (WINDOWS)

### 13.1 Execution Policy

```powershell
# Check
Get-ExecutionPolicy -List

# Set for current user (no elevation needed)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Bypass for single session
pwsh -ExecutionPolicy Bypass -File script.ps1
```

### 13.2 Credential Management (Never hardcode)

```powershell
# Modern: SecretManagement
Install-PSResource -Name Microsoft.PowerShell.SecretManagement
Install-PSResource -Name SecretManagement.KeyVault
Register-SecretVault -Name AzureKeyVault -ModuleName SecretManagement.KeyVault
$secret = Get-Secret -Name "DBPassword" -Vault AzureKeyVault

# Interactive (user types)
$cred = Get-Credential

# Encrypted file (same user/machine only)
$pwd = Read-Host "Password" -AsSecureString
$pwd | ConvertFrom-SecureString | Out-File "cred.txt"
$recovered = Get-Content "cred.txt" | ConvertTo-SecureString
```

### 13.3 Input Validation

```powershell
function Invoke-Action {
    [CmdletBinding()]
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter()]
        [ValidateRange(1, 100)]
        [int]$Count,

        [Parameter()]
        [ValidateSet("Option1", "Option2", "Option3")]
        [string]$Type,

        [Parameter()]
        [ValidatePattern('^\d{3}-\d{4}-\d{4}$')]
        [string]$Phone,

        [Parameter()]
        [ValidateScript({ Test-Path $_ })]
        [string]$FilePath
    )
}
```

### 13.4 JEA — Just Enough Administration

```powershell
# Restricted JEA session configuration
New-PSSessionConfigurationFile -SessionType RestrictedRemoteServer `
    -Path "C:\JEA\HelpDesk.pssc" `
    -VisibleCmdlets @{
        Name = 'Restart-Service'
        Parameters = @{ Name = 'Name'; ValidateSet = 'Spooler', 'Wuauserv' }
    }, @{ Name = 'Get-Service' } `
    -LanguageMode NoLanguage `
    -ExecutionPolicy RemoteSigned

Register-PSSessionConfiguration -Name HelpDesk -Path "C:\JEA\HelpDesk.pssc" -Force
```

### 13.5 Script Block Logging (Auditing)

```powershell
# Enable via registry (requires elevation)
# HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging
# EnableScriptBlockLogging = 1

# Query logs
Get-WinEvent -LogName "Microsoft-Windows-PowerShell/Operational" |
    Where-Object Id -eq 4104 |
    Select-Object TimeCreated, Message -First 20
```

### 13.6 Constrained Language Mode

```powershell
# Check current mode
$ExecutionContext.SessionState.LanguageMode
# FullLanguage = admin | ConstrainedLanguage = standard user

# Enable machine-wide (via machine environment variable)
[Environment]::SetEnvironmentVariable("__PSLockdownPolicy", "4", "Machine")
```

### 13.7 Code Signing (Production)

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My -CodeSigningCert
Set-AuthenticodeSignature -FilePath "script.ps1" -Certificate $cert
```

### 13.8 WDAC — Windows Defender Application Control

```powershell
# Create control policy for approved scripts
New-CIPolicy -FilePath "C:\WDAC\PowerShellPolicy.xml" `
    -ScanPath "C:\ApprovedScripts" `
    -Level FilePublisher `
    -Fallback Hash

ConvertFrom-CIPolicy -XmlFilePath "C:\WDAC\PowerShellPolicy.xml" `
    -BinaryFilePath "C:\Windows\System32\CodeIntegrity\SIPolicy.p7b"
```

---

## 14. MODULES AND PACKAGES

### 14.1 PSResourceGet (Modern, 2x faster)

```powershell
# Install (ships with PS 7.4+)
Install-Module -Name Microsoft.PowerShell.PSResourceGet -Force

# Install a module
Install-PSResource -Name Az -Scope CurrentUser -TrustRepository

# Search
Find-PSResource -Name "*Azure*"
Find-PSResource -Tag "Security"

# Update
Update-PSResource -Name Az
Update-PSResource                          # Update all

# List installed
Get-InstalledPSResource
Get-InstalledPSResource -Name Az

# Remove
Uninstall-PSResource -Name OldModule -AllVersions

# Offline: save and install
Save-PSResource -Name Az -Path C:\OfflineModules
Install-PSResource -Name Az -Path C:\OfflineModules
```

> Legacy commands `Install-Module`, `Find-Module`, etc. still work and call PSResourceGet internally.

### 14.2 Key Modules

| Module | Use | Install |
|--------|-----|---------|
| `Az` | Azure cloud | `Install-PSResource -Name Az` |
| `Microsoft.Graph` | Microsoft 365, AAD | `Install-PSResource -Name Microsoft.Graph` |
| `PnP.PowerShell` | SharePoint/Teams | `Install-Module PnP.PowerShell` |
| `PSWindowsUpdate` | Windows Update management | `Install-PSResource -Name PSWindowsUpdate` |
| `Pester` | Automated testing | `Install-PSResource -Name Pester` |
| `PSScriptAnalyzer` | Script linting | `Install-PSResource -Name PSScriptAnalyzer` |
| `ImportExcel` | Excel without Office | `Install-PSResource -Name ImportExcel` |
| `AWS.Tools.Installer` | AWS | `Install-Module AWS.Tools.Installer` |

### 14.3 PSWindowsUpdate

```powershell
# Install
Install-PSResource -Name PSWindowsUpdate

# Check available updates
Get-WindowsUpdate

# Install all updates (auto-accept)
Install-WindowsUpdate -AcceptAll -AutoReboot

# Install without reboot
Install-WindowsUpdate -AcceptAll -IgnoreReboot

# Install specific update by KB
Install-WindowsUpdate -KBArticleID "KB5034763" -AcceptAll

# List installed updates
Get-WUHistory

# Hide an update (skip specific KB)
Hide-WindowsUpdate -KBArticleID "KB1234567"
```

### 14.4 Azure (Az 14.5.0)

```powershell
Connect-AzAccount           # MFA required (Sep 2025+)
Get-AzVM
Get-AzResourceGroup
New-AzResourceGroup -Name "MyRG" -Location "BrazilSouth"
```

### 14.5 Microsoft 365 / Graph

```powershell
# MSOnline and AzureAD retired (2025) — use Microsoft.Graph
Connect-MgGraph -Scopes "User.Read.All", "Group.ReadWrite.All"
Get-MgUser
Get-MgGroup
```

### 14.6 Check and Import Modules

```powershell
if (-not (Get-Module -ListAvailable -Name Az)) {
    Install-PSResource -Name Az -Scope CurrentUser -TrustRepository
}
Import-Module -Name Az.Accounts
```

---

## 15. TESTING WITH PESTER

```powershell
# Install
Install-PSResource -Name Pester

# Test structure
Describe "Get-Something Tests" {
    BeforeAll {
        # Shared setup
        . "$PSScriptRoot\Get-Something.ps1"
    }

    Context "When input is valid" {
        It "Should return expected value" {
            $result = Get-Something -Name "Test"
            $result | Should -Be "Expected"
        }

        It "Should return non-null object" {
            Get-Something -Name "Test" | Should -Not -BeNullOrEmpty
        }
    }

    Context "When input is invalid" {
        It "Should throw an error" {
            { Get-Something -Name $null } | Should -Throw
        }
    }
}

# Run
Invoke-Pester -Path ./tests
Invoke-Pester -Path ./tests -CodeCoverage ./src/*.ps1
Invoke-Pester -Path ./tests -OutputFormat NUnitXml -OutputFile TestResults.xml
```

---

## 16. LINTING AND CODE ANALYSIS

```powershell
# Install
Install-PSResource -Name PSScriptAnalyzer

# Analyze script
Invoke-ScriptAnalyzer -Path "script.ps1"
Invoke-ScriptAnalyzer -Path "script.ps1" -Severity Error, Warning

# Analyze entire directory
Invoke-ScriptAnalyzer -Path "./src" -Recurse

# Gate before execution (CI/CD)
$results = Invoke-ScriptAnalyzer -Path "script.ps1"
if ($results.Count -gt 0) { exit 1 }
```

---

## 17. COMMON ERRORS AND SOLUTIONS

| Error Message | Cause | Solution |
|---------------|-------|----------|
| `A parameter 'or' cannot be found` | Missing parentheses | Wrap cmdlets in `()` |
| `Unexpected token` | Unicode character | Use ASCII only |
| `Cannot find property on null` | Null object | Check null before accessing |
| `Cannot convert value to type` | Type mismatch | Use `.ToString()` or `[int]` cast |
| `The term 'x' is not recognized` | Module not imported | `Import-Module ModuleName` |
| `Access denied` | Insufficient privileges | Run as Administrator |
| `Out-GridView search not working` | PS 7.5 / .NET 9 bug | Use `Where-Object` or export CSV |

### Execution Policy Blocking Script

```powershell
# For current user (no elevation)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# For single session
pwsh -ExecutionPolicy Bypass -File "script.ps1"
```

### Check PowerShell Version

```powershell
$PSVersionTable.PSVersion   # Check current version

# PS 5.1: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
# PS 7+:  C:\Program Files\PowerShell\7\pwsh.exe
```

---

## 18. PRE-EXECUTION CHECKLIST

Before running a script in production:

- [ ] `#Requires -Version 7.0` declared (or 5.1 if a Windows-only module requires it)
- [ ] `$ErrorActionPreference = "Stop"` set
- [ ] `Set-StrictMode -Version Latest` enabled
- [ ] All cmdlets inside try/catch with `-ErrorAction Stop`
- [ ] No hardcoded credentials
- [ ] Paths built with `Join-Path` or `$env:` variables
- [ ] Dynamic arrays using `List[T]` or `ArrayList`
- [ ] JSON serialized with `-Depth 10` (or higher)
- [ ] ASCII characters only in the script
- [ ] `Invoke-ScriptAnalyzer` returns no errors or warnings

---

## When to Use This Skill

- Writing, reviewing, or optimizing `.ps1` scripts for Windows
- Automating Windows tasks (services, registry, processes)
- Integrating with Azure, Microsoft 365, or AWS via PowerShell
- Implementing parallelism with `ForEach-Object -Parallel`
- Building or consuming REST APIs
- Configuring security (JEA, WDAC, code signing)
- Managing Windows Updates with PSWindowsUpdate
- Managing modules and packages via PSGallery/PSResourceGet

## Limitations

- This skill is Windows-exclusive — no cross-platform content
- For PS 5.1-specific scenarios (WSUS, legacy Group Policy), adapt the examples
- Always validate in a test environment before production
- Security and compliance depend on your specific corporate environment
