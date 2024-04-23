## PowerShell Script to Retrieve WiFi Password

### Description
This PowerShell script prompts the user to input the name of a WiFi profile and retrieves the password for that profile.

### Usage
1. Run the script in PowerShell.
2. The script will display the all saved password if it's stored for the specified profile.

### Script
```powershell
# Get list of all WiFi profiles
$profiles = netsh wlan show profiles | Select-String -Pattern "All User Profile" | ForEach-Object { $_.ToString().Split(":")[1].Trim() }

# Iterate through each profile and retrieve password
foreach ($profile in $profiles) {
    $wifiPassword = netsh wlan show profile name="$profile" key=clear | Select-String -Pattern "Key Content" | ForEach-Object { $_.ToString().Split(":")[1].Trim() }
    
    if ($wifiPassword -ne $null) {
        Write-Output "Password for WiFi Profile '$profile': $wifiPassword"
    } else {
        Write-Output "WiFi Profile '$profile' password not found or not stored."
    }
}

