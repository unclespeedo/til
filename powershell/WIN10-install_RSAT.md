# TODO: add granular, component-by-component install
# Sourced from Martin Bengtsson (@mwbengtsson) script
# Filename: Install-RSATv1809v1903v1909.ps1

PS> `Get-WindowsCapability -Online | Where-Object {$_.Name -like "Rsat.ActiveDirectory*" -OR $_.Name -like "Rsat.DHCP.Tools*" -OR $_.Name -like "Rsat.Dns.Tools*" -OR $_.Name -like "Rsat.GroupPolicy*" -OR $_.Name -like "Rsat.ServerManager*" -AND $_.State -eq "NotPresent" } | Add-WindowsCapability -Online`