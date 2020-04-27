The scripts below, in addition to groups.csv and users.csv can be used to great email distribution groups and add members to them.

# groups.csv (GroupDisplayName, GroupName, GroupEmail)
Import-CSV groups.csv | ForEach-Object {
    $GroupName = $_.GroupName
    if (Get-DistributionGroup | Where-Object {$_.Name -eq $GroupName}) {
        Write-Host "Group $GroupName already exists, enforcing RequireSenderAuthenticationEnabled setting"
        Set-DistributionGroup -Identity $_.GroupName -RequireSenderAuthenticationEnabled $False
    } else {
        Write-Host "Group $GroupName doesn't exist, creating."
        New-DistributionGroup -DisplayName $_.GroupDisplayName -Name $_.GroupName -PrimarySmtpAddress $_.GroupEmail -RequireSenderAuthenticationEnabled $False
    }
}

# users.csv (Group, Name, Email)
Import-CSV users.csv | ForEach-Object {
    $EmailAddress = $_.Email
    $Name = $_.Name
    $Group = $_.Group

    if (Get-MailContact |Where-Object {$_.PrimarySmtpAddress -eq $EmailAddress}) {
        Write-Host "Contact already exists for $EmailAddress"
    } else {
        Write-Host "Creating new contact: $EmailAddress"
        New-MailContact -Name $Name -ExternalEmailAddress $EmailAddress
        Add-DistributionGroupMember -Identity $Group -Member $Name
    }
}
