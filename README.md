# Azure Blob Storage Static Website Hosting

**Live Site:** [https://adebolaportfolio.z1.web.core.windows.net/](https://adebolaportfolio.z1.web.core.windows.net/)

---

## Project Overview

This project demonstrates how to host a static website using **Azure Blob Storage** as a cost-effective, serverless alternative to traditional web servers. Rather than provisioning and maintaining a virtual machine running Apache or Nginx, Azure Blob Storage serves static content directly from cloud storage via a public endpoint — eliminating server management overhead while maintaining high availability.

The hosted site is a personal portfolio for **Adebola**, an IT and Cloud Computing student, showcasing skills, projects, and contact information. The portfolio itself acts as a live demonstration of the serverless deployment principles covered in this program.

---

## Live URL

```
https://adebolaportfolio.z1.web.core.windows.net/
```

---

## Screenshots

All Azure Portal screenshots are located in the `/screenshots` folder:

| File | Description |
|------|-------------|
| `storage-account-created.png` | Storage account overview showing configuration details |
| `static-website-enabled.png` | Static website settings with endpoint, index doc, and error doc |
| `web-container-files.png` | `$web` container contents showing all uploaded files and folders |
| `live-site.png` | Live portfolio site rendered in the browser |
| `lighthouse-report.png` | Google Lighthouse performance audit results |

---

## Source Files

```
/
├── index.html          # Main portfolio page
├── 404.html            # Custom error page
├── css/
│   └── style.css       # Full responsive stylesheet
├── js/
│   └── main.js         # Scroll animations and nav highlighting
└── README.md           # This file
```

---

## Azure Configuration

### Storage Account Settings

| Setting | Value | Reason |
|--------|-------|--------|
| Performance tier | Standard | Sufficient for static content; Premium is for low-latency transactional workloads |
| Redundancy | LRS (Locally Redundant Storage) | Lowest cost option; adequate for a student portfolio with no SLA requirement |
| Region | Sweden Central | Selected for availability under Azure for Students subscription |
| Account kind | StorageV2 (general purpose v2) | Supports static website hosting feature |

### Static Website Settings

| Setting | Value |
|--------|-------|
| Index document | `index.html` |
| Error document | `404.html` |
| Primary endpoint | `https://adebolaportfolio.z1.web.core.windows.net/` |

---

## Redundancy & Performance Tier Rationale

### Why LRS?

**Locally Redundant Storage (LRS)** replicates data three times within a single Azure data centre. It was chosen because:

- This is a student portfolio — there is no business-critical uptime requirement
- LRS is the most cost-effective redundancy option (~$0.018/GB/month vs ~$0.035/GB for GRS)
- All content is static and can be re-uploaded from source if needed, reducing recovery risk
- The site's source files are also stored in a GitHub repository, providing an independent backup

For a production deployment serving paying customers, **GRS (Geo-Redundant Storage)** or **RA-GRS** would be more appropriate, as they replicate data to a secondary region and protect against regional outages.

### Why Standard Performance?

**Standard** performance uses HDD-backed storage and is ideal for general-purpose workloads including static websites, where files are read infrequently and latency tolerances are relaxed. **Premium** performance (SSD-backed) is designed for high-transaction workloads such as databases or real-time applications — unnecessary and more expensive for a static site.

---

## Cost Estimate

For this project's scale (under 1 MB of files, minimal traffic):

| Resource | Estimated Monthly Cost |
|----------|----------------------|
| Blob Storage (LRS, Standard) | < $0.01 |
| Static website hosting | Free (included with storage account) |
| Bandwidth (first 100 GB/month egress) | Free within Azure for Students |
| **Total** | **~$0.00 – $0.01/month** |

---

## Scalability & CDN Integration

Azure Blob Storage static hosting works well for low-to-medium traffic sites, but for global performance the recommended approach is to front the storage endpoint with **Azure Front Door** or **Azure CDN**:

- **Azure CDN** caches static assets at edge nodes worldwide, reducing latency for users far from the origin region (Sweden Central)
- **Azure Front Door** adds caching, WAF (Web Application Firewall), custom domains, and HTTPS certificates
- Both services integrate directly with Azure Blob Storage static websites via the primary endpoint

For this student project, CDN integration was not enabled to keep costs at zero, but the architecture is designed to support it — the primary endpoint can be set as the CDN origin with no changes to the source files.

---

## Performance Audit (Google Lighthouse)

A Lighthouse audit was run on the live site via Chrome DevTools on **24 June 2026** using Desktop mode.

| Category | Score | Rating |
|----------|-------|--------|
| Performance | 79 | 🟠 Needs Improvement |
| Accessibility | 81 | 🟠 Needs Improvement |
| Best Practices | 77 | 🟠 Needs Improvement |
| SEO | 90 | 🟢 Good |

> **Note:** Scores were measured without CDN integration, from the Sweden Central origin. Chrome extensions present during the audit may have slightly lowered the Performance score. True scores are likely marginally higher when run in incognito mode.

### What the scores mean

**Performance (79):** The site loads reasonably well for a static site served directly from blob storage without a CDN. Adding **Azure CDN** to cache assets at edge nodes closer to the user would significantly reduce latency and improve this score — particularly for users outside Europe.

**Accessibility (81):** Good baseline. Minor improvements such as adding `aria-label` attributes to icon-only links and improving colour contrast on secondary text would push this into the green range.

**Best Practices (77):** Primarily affected by the absence of a custom domain with HTTPS. Adding a custom domain with an Azure-managed SSL certificate via CDN would address this.

**SEO (90):** Strong result. The site includes proper meta tags, semantic HTML structure, and descriptive link text — all contributing to good search engine visibility.

### Planned improvements

- Integrate **Azure CDN** → improves Performance by reducing global latency
- Configure **custom domain + HTTPS** → improves Best Practices score
- Add missing `aria-label` attributes → improves Accessibility score

---

## Tasks Completed

- [x] Provisioned Azure Storage Account (Standard, LRS)
- [x] Enabled Static Website Hosting
- [x] Configured `index.html` as index document and `404.html` as error document
- [x] Uploaded all website assets to `$web` container with correct folder structure
- [x] Verified live deployment via primary endpoint
- [x] Reviewed security settings and scalability options
- [x] Documented redundancy/tier rationale and cost estimate
- [x] Ran Google Lighthouse performance audit and documented results

---

## Author

**Adebola** — IT & Cloud Computing Student  
Hosted on **Microsoft Azure** · Azure Blob Storage Static Website Hosting
