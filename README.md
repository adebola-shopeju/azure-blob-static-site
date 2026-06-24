Azure Blob Storage Static Website Hosting

Live Site: https://adebolaportfolio.z1.web.core.windows.net/


Project Overview

This project demonstrates how to host a static website using Azure Blob Storage as a cost-effective, serverless alternative to traditional web servers. Rather than provisioning and maintaining a virtual machine running Apache or Nginx, Azure Blob Storage serves static content directly from cloud storage via a public endpoint — eliminating server management overhead while maintaining high availability.

The hosted site is a personal portfolio for Adebola, an IT and Cloud Computing student, showcasing skills, projects, and contact information. The portfolio itself acts as a live demonstration of the serverless deployment principles covered in this program.


Live URL

https://adebolaportfolio.z1.web.core.windows.net/


Screenshots

All Azure Portal screenshots are located in the /screenshots folder:

FileDescriptionstorage-account-created.pngStorage account overview showing configuration detailsstatic-website-enabled.pngStatic website settings with endpoint, index doc, and error docweb-container-files.png$web container contents showing all uploaded files and folderslive-site.pngLive portfolio site rendered in the browser


Source Files

/
├── index.html          # Main portfolio page
├── 404.html            # Custom error page
├── css/
│   └── style.css       # Full responsive stylesheet
├── js/
│   └── main.js         # Scroll animations and nav highlighting
└── README.md           # This file


Azure Configuration

Storage Account Settings

SettingValueReasonPerformance tierStandardSufficient for static content; Premium is for low-latency transactional workloadsRedundancyLRS (Locally Redundant Storage)Lowest cost option; adequate for a student portfolio with no SLA requirementRegionSweden CentralSelected for availability under Azure for Students subscriptionAccount kindStorageV2 (general purpose v2)Supports static website hosting feature

Static Website Settings

SettingValueIndex documentindex.htmlError document404.htmlPrimary endpointhttps://adebolaportfolio.z1.web.core.windows.net/


Redundancy & Performance Tier Rationale

Why LRS?

Locally Redundant Storage (LRS) replicates data three times within a single Azure data centre. It was chosen because:


This is a student portfolio — there is no business-critical uptime requirement
LRS is the most cost-effective redundancy option (~$0.018/GB/month vs ~$0.035/GB for GRS)
All content is static and can be re-uploaded from source if needed, reducing recovery risk
The site's source files are also stored in a GitHub repository, providing an independent backup


For a production deployment serving paying customers, GRS (Geo-Redundant Storage) or RA-GRS would be more appropriate, as they replicate data to a secondary region and protect against regional outages.

Why Standard Performance?

Standard performance uses HDD-backed storage and is ideal for general-purpose workloads including static websites, where files are read infrequently and latency tolerances are relaxed. Premium performance (SSD-backed) is designed for high-transaction workloads such as databases or real-time applications — unnecessary and more expensive for a static site.


Cost Estimate

For this project's scale (under 1 MB of files, minimal traffic):

ResourceEstimated Monthly CostBlob Storage (LRS, Standard)< $0.01Static website hostingFree (included with storage account)Bandwidth (first 100 GB/month egress)Free within Azure for StudentsTotal~$0.00 – $0.01/month


Scalability & CDN Integration

Azure Blob Storage static hosting works well for low-to-medium traffic sites, but for global performance the recommended approach is to front the storage endpoint with Azure Front Door or Azure CDN:


Azure CDN caches static assets at edge nodes worldwide, reducing latency for users far from the origin region (Sweden Central)
Azure Front Door adds caching, WAF (Web Application Firewall), custom domains, and HTTPS certificates
Both services integrate directly with Azure Blob Storage static websites via the primary endpoint


For this student project, CDN integration was not enabled to keep costs at zero, but the architecture is designed to support it — the primary endpoint can be set as the CDN origin with no changes to the source files.


Tasks Completed


 Provisioned Azure Storage Account (Standard, LRS)
 Enabled Static Website Hosting
 Configured index.html as index document and 404.html as error document
 Uploaded all website assets to $web container with correct folder structure
 Verified live deployment via primary endpoint
 Reviewed security settings and scalability options
 Documented redundancy/tier rationale and cost estimate



Author

Adebola — IT & Cloud Computing Student

Hosted on Microsoft Azure · Azure Blob Storage Static Website Hosting
