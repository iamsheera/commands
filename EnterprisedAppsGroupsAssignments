# Install the AzureAD module if not already installed
if (!(Get-Module -Name AzureAD -ListAvailable)) {
    Install-Module -Name AzureAD -Force -AllowClobber -Scope CurrentUser
}
 
# Connect to Azure AD
Connect-AzureAD
 
# Get all groups
$groups = Get-AzureADGroup -All $true
 
# Create an array to store results
$groupApps = @()
 
# Loop through each group and check application assignments
foreach ($group in $groups) {
    $groupName = $group.DisplayName
    $groupId = $group.ObjectId
 
    # Get assigned applications (Service Principals)
    $appAssignments = Get-AzureADGroupAppRoleAssignment -ObjectId $groupId
 
    if ($appAssignments) {
        foreach ($app in $appAssignments) {
            $appDetails = Get-AzureADServicePrincipal -ObjectId $app.ResourceId

            $groupApps += [PSCustomObject]@{
                GroupName  = $groupName
                GroupId    = $groupId
                AppName    = $appDetails.DisplayName
                AppId      = $appDetails.AppId
            }
        }
    }
}
 
# Output results in table format
$groupApps | Format-Table -AutoSize
 
# Optional: Export results to a CSV file
$groupApps | Export-Csv -Path "C:\AzureAD_Groups_Applications.csv" -NoTypeInformation
