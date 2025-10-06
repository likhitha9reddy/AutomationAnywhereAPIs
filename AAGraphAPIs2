# GraphAPI using Automation Anywhere
The below code is written using REST Web Services v3.24.1 package

## Problem Statement #1
Let's say there is a company called 'somecompany' that has a sharepoint site to manage financial data based on different countries. The data is configured in a sharepoint list, say 'somelist'having different financial data. The Automation Anywhere bot should be able to download the input file based on status 'Ready for Posting'. Once downloaded, it should be change the status as 'Bot Processing'.
## Solution
### Authentication
*Replace the values in <> with actual values*
Request Type : POST
URI : https//login.microsoftonline.com/<tenant-id>/v2.0/token
Content type : application/x-www-form-urlencoded
Body Parameters : 
1. client_id = <your-client-id>
2. scope = https://graph.microsoft.com/.default
3. client_secret = <your-client-secret>
4. grant_type = client_credentials

Response : It will return access token that can be saved in Dictionary variable. Use the package 'JSON Deserializer' to retrieve the body of Dictionary. Let's say it is stored in variable AccessToken
*To get the exact header value, first print the dictionary into an excel file as datatable. Then manually open the file to get required headers. Pass this header details into JSON Deserializer: Retrieve a Value command to get the access token.*

### Fetching the SiteID
*Replace the values in <> with actual values*
Request Type : GET
URI : https//graph.microsoft.com/v1.0/sites/<subdomain>.sharepoint.com:/teams/<sitename>
Authentication Mode : No Authentication
Custom Headers : 
1. Authorization : Bearer $AccessToken$
2. Accept = application/json

Response: It will return the SiteID for the sitename passed in the URI.
*This SiteID will be in format - {hostname},{siteCollectionID},{siteID}. When mentioned in URI to pass as <site-id>, all 3 are to be passed.*

### Fetching the ListID
*Replace the values in <> with actual values*
Request Type : GET
URI : https//graph.microsoft.com/v1.0/sites/<site-id>/lists
Authentication Mode : No Authentication
Custom Headers : 
1. Authorization : Bearer $AccessToken$
2. Accept = application/json

Response: It will return all the available ListIDs for that particular site.
*Use JSON Deserializer: To Table command to store it as a DataTable variable. Use Data Table: Get number of rows and decrement it to 1 (header). Now loop through the DataTable variable to find the listname name 'somelist'. Once match is found, extract the ListID.*

### Fetching all the IDs with status 'Ready for Posting'
*Replace the values in <> with actual values*
Request Type : GET
URI : https//graph.microsoft.com/v1.0/sites/<site-id>/lists/<list-id>/items?$expand=fields/Status eq 'Ready for Posting'
Authentication Mode : No Authentication
Custom Headers : 
1. Authorization : Bearer $AccessToken$
2. Accept = application/json
3. Prefer : HonorNonIndexedQueriesWarningMayFailRandomly

Response: It will return all the IDs that have status 'Ready for Posting'.
*Use JSON Deserializer to retrieve only the 1st item-id and any other details if needed.*

### Fetching all the details for 1st Item Id
*Replace the values in <> with actual values*
Request Type : GET
URI : https//graph.microsoft.com/v1.0/sites/<site-id>/lists/<list-id>/items/<item-id>?expand=fields
Authentication Mode : No Authentication
Custom Headers : 
1. Authorization : Bearer $AccessToken$
2. Accept = application/json
3. Prefer : HonorNonIndexedQueriesWarningMayFailRandomly

Response: It will return all the details for the Item Id value passed.
*Use JSON Deserializer to retrieve details as needed, like the input file download URL, country name, and so on.*

### Downloading the Input File
*As of September 2025, Graph API does not support downloading the file attachment via ListItems.*
*Use SharePoint Package, first authenticate and then in List and List Item sub-section, use 'Download List Attachment(s)'*

### Changing status to 'Bot Processing'
*Replace the values in <> with actual values*
Request Type : POST
URI : https//graph.microsoft.com/v1.0/sites/<site-id>/lists/<list-id>/items/<item-id>/fields
Content Type : JSON (application/json)
Custom Parameters : 
{
Status: "Bot Processing"
}

