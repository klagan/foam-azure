# Groups

We recently had a client who was ready to streamline the security of their SharePoint Online site and change it from ‘Everyone’ access to groups of people with more specific access. Our recommendation to them was to use Azure AD groups so that the groups would be global and could be both centrally managed and used across site collections.
As we moved ahead with it, they had the groups added with the appropriate members. We then granted SharePoint permissions to the new AD groups by adding them into the appropriate SharePoint groups and removing the reference to ‘Everyone but external users’.
At first all seemed to work ok but as the week progressed, random problems started cropping up that we couldn’t explain, the biggest one revolving around search results. One of their users (and others, we later found out), who had full read access to the root site and all sub-sites, would only get back results from his OneDrive library and from the separate training documents site (which is open to Everyone). Yet he could easily navigate to and access all the document libraries in all the sites.
Thus began my long search on all sorts of things SharePoint search related, trying to figure out what was going on. For some reason, I finally decided to go look at the AD groups themselves with the thought that since roles are assigned to users, maybe the same thing might need to be done for groups. This was a bust of course, but being fairly new to administrating SharePoint Online, I was game for checking all sorts of things I didn’t know about.
Luckily this random check ultimately ended up pointing me to the real problem. It turns out these two new groups were setup as Office 365 Groups instead of security groups. At the time, I didn’t know anything about Office 365 Groups but didn’t really think this could be the problem. I decided to do a little research anyway into what that meant. One of the definitions I found was:
Office 365 Groups is a service that enables teams to come together and get work done by establishing a single team identity (managed in Azure Active Directory) and a single set of permissions across Office 365 apps including Outlook, SharePoint, OneNote, Skype for Business, Planner, Power BI, and Dynamics CRM.
So SharePoint was mentioned in that list of Office 365 apps, right? How could the group type be the problem then? We needed access to SharePoint and it says it does that. What it doesn’t tell you is that it’s mostly referring to access to the team site that is created, specifically for that group, when the group is first created. It does not mean that it will be very usable by other sites.
After more searching and finding very little, I decided it was time to do some of my own testing. First I had a test user added to the Office 365 Group currently in use. After giving the cloud some time to process this change, I signed in and ran a search or two. What I got back was very similar to the user mentioned above. I got little or no results back, even though I had access to everything in the site.
I then created a new Azure AD Security group, added the same test user to it and then granted it the same permissions in the SharePoint site as the Office 365 Group had. After waiting a decent time so I was sure the security change was processed by search, I signed back in and found that the behavior was now entirely different. With the test user as a member of my new security group, I got back tons of results, just as expected. To further verify, I then removed the user from my test security group, waited a bit, ran a search and found I was back to square one. This was pretty solid evidence that the Office 365 Group was the culprit.
Our end solution was to create new Azure AD Security groups, add the correct members, grant them the same access as had been granted to the Office 365 Groups and then remove the access for the Office 365 Groups. This seems to have corrected all the problems the users were experiencing.
I still don’t know a lot about Office 365 Groups and haven’t had time to research much further, but I do like the below snippet (found here) that very succinctly describes each group and what it does.
￼
I’m sure that Office 365 Groups have a place in the Microsoft world, but it is definitely not as a replacement to AD security groups.
As a quick recap, here are the areas that were impacted (at least on this particular site) by using an Office 365 Group instead of a security group:
* Search – would not return results from the site
* Starting a workflow – if a user in an O365 group kicked off a workflow, the workflow got hung up with an ‘Access Denied’ error before it ever got far enough along to send out custom errors.
* Site access – Various users had problems accessing the site, even though they were in the correct group. Was a very random thing as it worked for some and for others, it didn’t.
I hope this helps save someone some time in the future!

[source](https://threewill.com/office-365-groups-vs-azure-ad-security-groups/)
[[tenant.md]]
[//begin]: # "Autogenerated link references for markdown compatibility"
[aad]: aad "Azure Active Directory"
[//end]: # "Autogenerated link references"
