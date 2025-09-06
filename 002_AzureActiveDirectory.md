How do on-prem AD and Azure AD work together?

While it’s possible to have a purely cloud-based environment, most organizations today have a hybrid AD environment. 
They use the free Microsoft tool Azure AD Connect to sync identity data from their on-prem AD to Azure AD; 
then users can use their on-premises credentials 
to authenticate to cloud resources such as SharePoint Online, Teams, and SaaS apps like Dropbox, Google apps and Amazon Web Services (AWS).

Behind the scenes, IT pros manage users, groups and permissions (primarily) in the on-prem AD, 
and any changes are automatically synced up to the cloud. 
This alleviates the need to try to manage two completely separate sets of identities and permissions, 
which would be very difficult and highly prone to error.

However, not everything can be stored and managed in the on-premises AD. 
You will also have cloud-only objects and attributes, such as these:
