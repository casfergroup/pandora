---
title: Google Dorking Cheat Sheet
description: Guide on how to transform Google into a powerful tool.
links:
  - https://www.google.com/
killchain:
  - recon
---
## What's Google Dorking?
Google Dorking is the practice of using advanced search operators and filters in search engines, such as Google, to discover valuable information about a target. Often, organizations or individuals unintentionally expose confidential files, sensitive configuration archives, internal documents, or security credentials online, making them accessible to anyone who knows how to search effectively.

These exposed resources can be incredibly valuable during reconnaissance, allowing you to gather intelligence about a target both passively (by simply reviewing the search results) and actively (by visiting and interacting with the discovered pages). Passive gathering carries minimal risk of detection, appart from the search engine itself, whereas active exploration — accessing the files or pages — could trigger logs on the target's servers.

When conducting active searches, it’s important to remember that the target **may log details such as IP addresses, user-agents, and timestamps**. To protect your anonymity, you should take appropriate [OpSec](https://en.wikipedia.org/wiki/Operations_security) measures. This can include using privacy-focused tools like [Talis](https://tails.net/) or routing your traffic through [Tor](https://www.torproject.org/) to hide your real identity and location.

Properly used, Google Dorking is a powerful tool in both ethical hacking and malicious reconnaissance. **Always ensure you have permission when engaging in any form of testing or data gathering, as unauthorized access or exploitation can be illegal.**

## Search Engines
There are plenty of search engines available in the wild. Depending on your target and the type of content you're aiming to find, one engine may be more effective than another. Each has different strengths, regional coverage, and indexing behaviors that can give you an edge during reconnaissance.

Engine | Primary Region | vs Google
---|---|---
[Google](https://google.com/) | Americas, Western Euroupe, Japan | Google
[Bing](https://bing.com/) | Americas, Western Euroupe, Japan | Slower indexing; strong image-specific filters; sometimes finds different or less-moderated content.
[DuckDuckGo](https://duckduckgo.com/) | Americas, Western Euroupe, Japan | Privacy-focused; powered mainly by Bing results; fewer personalized search results.
[Yandex](https://yandex.com/) | Russia, ex-URSS, Turkey | Excellent for image searches; indexes different segments of the web, especially Russian-speaking sites.
[Baidu](https://www.baidu.com) | China | Best for Chinese-language content and local Chinese websites; limited indexing of global content.

## Filters
This guide will focus on filters that work primarily with Google, as they are the most universal and often compatible across other platforms too. Some basic filters even function on social media sites like LinkedIn, Twitter, and Facebook.

### Logical Filters
#### `AND`
Finds results that contain both specified terms.

Usage:
``` html {linenos=inline style=base16-snazzy}
string1 AND string2
```

Example: hacker AND "penetration testing"

Notes:

Google usually implies AND by default, but using it can help in complex queries to force clarity.

#### `OR`
Finds results that contain either one term or the other. Usage:
``` html {linenos=inline style=base16-snazzy}
string1 OR string2
```
Example: "cybersecurity consultant" OR "security analyst"

Notes: OR must be in uppercase; otherwise, Google treats it as a normal word.

#### `-` (Minus)
Excludes results containing a specific term. Usage:
``` html {linenos=inline style=base16-snazzy}
string1 -string2
```
Example: pentest report -template

Notes: The minus sign must be placed immediately before the word you want to exclude (no space).

#### `""` (Quotation Marks)
Searches for an exact match of the text between the quotes. Usage:
``` html {linenos=inline style=base16-snazzy}
"exact phrase here"
```

Example: "internal use only"

Notes: Quotation marks are crucial for finding specific phrases, file names, or strings.

#### `*` (Wildcard)
Acts as a placeholder for any word. Usage:
``` html {linenos=inline style=base16-snazzy}
"initial string * end string"
```

Example: "configure * server"

Notes: Wildcards can help when you're unsure about specific words in a phrase.

#### `( )` (Parentheses)
Groups terms or operators to control the logic of a search. Usage:
``` html {linenos=inline style=base16-snazzy}
(string1 OR string2) AND string3
```

Example: (pentest OR "penetration test") report

Notes: Helps organize complex queries, similar to mathematical equations.

### Type Flters
#### site:
Limits results to a specific website or domain. Usage:
``` html {linenos=inline style=base16-snazzy}
site:example.com keyword
```

Example: site:linkedin.com "security engineer"

Notes: Useful for targeting a specific site or restricting your search to a company's domain.

#### `inurl:`
Searches for pages that have the specified keyword **in the URL**.  
Usage:
```html {linenos=inline style=base16-snazzy}
inurl:keyword
```
Example: `inurl:admin login`

Notes: Great for locating login pages, admin panels, or directories based on URL patterns.

#### `allinurl:`
Searches for pages where **all specified words** appear in the URL.  
Usage:
```html {linenos=inline style=base16-snazzy}
allinurl:keyword1 keyword2
```
Example: `allinurl:admin login portal`

Notes: Forces all words to exist in the URL; broader than `inurl:` when stacking multiple terms.

#### `intitle:`
Searches for pages with the specified keyword **in the title** tag.  
Usage:
```html {linenos=inline style=base16-snazzy}
intitle:keyword
```
Example: `intitle:"index of /backup"`

Notes: Good for finding open directories or specific pages like "Login," "Dashboard," etc.

#### `allintitle:`
Searches for pages where **all specified words** are in the title.  
Usage:
```html {linenos=inline style=base16-snazzy}
allintitle:keyword1 keyword2
```
Example: `allintitle:admin dashboard`

Notes: All words must appear in the title; can be more restrictive than `intitle:`.

#### `filetype:`
Limits results to a specific **file extension**.  
Usage:
```html {linenos=inline style=base16-snazzy}
filetype:extension keyword
```
Example: `filetype:pdf penetration test report`

Notes: Useful for hunting exposed documents like `.pdf`, `.docx`, `.xlsx`, `.txt`, `.csv`, etc.

#### `ext:` (Alternate to `filetype:`)
Another way to specify a file extension for the search.  
Usage:
```html {linenos=inline style=base16-snazzy}
ext:extension keyword
```
Example: `ext:log passwords`

Notes: Functions similarly to `filetype:`, but less officially documented; still effective.

#### `intext:`
Finds pages that contain the specified keyword **in the body text**.  
Usage:
```html {linenos=inline style=base16-snazzy}
intext:keyword
```
Example: `intext:"confidential - do not distribute"`

Notes: Powerful for locating sensitive disclosures buried in web content.

#### `allintext:`
Finds pages where **all specified keywords** are in the text body.  
Usage:
```html {linenos=inline style=base16-snazzy}
allintext:keyword1 keyword2
```
Example: `allintext:"user database" password`

Notes: Forces all terms to appear somewhere in the text, making it stricter than `intext:`.

#### `cache:`
Displays Google's **cached** version of a page.  
Usage:
```html {linenos=inline style=base16-snazzy}
cache:example.com
```
Example: `cache:example.com/login`

Notes: Useful if the original page is down or has been taken offline; reveals older snapshots.

#### `related:`
Finds sites **similar** to a specified domain.  
Usage:
```html {linenos=inline style=base16-snazzy}
related:example.com
```
Example: `related:linkedin.com`

Notes: Handy for identifying competitors, sister companies, or associated sites.

Awesome! Here's the **Advanced Search Tricks** section in the same clean style you're using:

### Advanced Filters

Once you’re comfortable with basic filters, you can **chain multiple operators together** to create powerful, laser-focused queries. This allows you to narrow results dramatically and find exactly what you're looking for.

#### Combining Operators
Use multiple filters together to refine results even further.  
Usage:
```html {linenos=inline style=base16-snazzy}
site:example.com filetype:pdf "internal memo"
```
Example: `site:example.com filetype:pdf "internal memo"`

Notes: Combines `site:`, `filetype:`, and an exact phrase search to target specific documents inside a domain.

#### Stacking `OR` Conditions
Use parentheses to group several `OR` conditions.  
Usage:
```html {linenos=inline style=base16-snazzy}
site:example.com (password OR credential OR login)
```
Example: `site:example.com (password OR credential OR login)`

Notes: Google treats anything inside parentheses together; use this to broaden keyword variations without losing focus.

#### Searching Multiple Sites at Once
Target several domains using `OR` between `site:` operators.  
Usage:
```html {linenos=inline style=base16-snazzy}
(site:example.com OR site:example.org) "private document"
```
Example: `(site:github.com OR site:gitlab.com) "access token"`

Notes: Very effective when investigating companies with multiple brands or domains.

#### Combining `inurl:` and `filetype:`
Find specific files by hunting directory structures.  
Usage:
```html {linenos=inline style=base16-snazzy}
inurl:"/backup" filetype:zip
```
Example: `inurl:"/backup" filetype:zip`

Notes: Helps locate compressed backup files left in accessible locations.

#### Force Fresh Results with `before:` and `after:`
Search pages published within specific date ranges.  
Usage:
```html {linenos=inline style=base16-snazzy}
site:example.com "leak" after:2023-01-01 before:2024-01-01
```
Example: `site:example.com "leak" after:2023-01-01 before:2024-01-01`

Notes: Requires YYYY-MM-DD format. Useful for tracking recent breaches, updates, or announcements.

#### Directory Listing Discovery
Search for open folders on web servers.  
Usage:
```html {linenos=inline style=base16-snazzy}
intitle:"index of /" "parent directory" keyword
```
Example: `intitle:"index of /" "parent directory" backup`

Notes: Often reveals exposed server directories containing files not meant for public viewing.

#### Finding Exposed Login Pages
Locate admin panels and login interfaces.  
Usage:
```html {linenos=inline style=base16-snazzy}
inurl:admin intitle:login
```
Example: `inurl:admin intitle:login`

Notes: Good first step during target mapping and reconnaissance phases.

#### Finding Error Pages and Misconfigurations
Search for default or leaked error messages.  
Usage:
```html {linenos=inline style=base16-snazzy}
intitle:"404 Not Found" OR intitle:"403 Forbidden"
```
Example: `intitle:"403 Forbidden" site:example.com`

Notes: Error pages can leak sensitive path information or server details.

#### Search for Publicly Exposed Cameras
Find unsecured webcams or surveillance systems.  
Usage:
```html {linenos=inline style=base16-snazzy}
inurl:view.shtml OR inurl:viewerframe?mode=motion
```
Example: `inurl:viewerframe?mode=motion`

Notes: Part of "Shodan-style" reconnaissance using Google alone.

## Search Customization

You can add special parameters at the end of your Google URL manually:
- `&num=100` → show up to 100 results per page (default is 10)
- `&hl=en` → force interface language to English
- `&start=0` → control pagination directly (useful for automation/scripting)

## Ready-to-Use Dork Templates
Below are practical Google Dork templates you can directly copy, adapt, and use during reconnaissance or information gathering.

### Find Exposed Login Pages
```html {linenos=inline style=base16-snazzy}
inurl:login intitle:login
inurl:admin intitle:"admin login"
inurl:signin
```
Notes: Locate authentication portals and admin interfaces.

### Discover Open Directories
```html {linenos=inline style=base16-snazzy}
intitle:"index of /" "parent directory"
intitle:"index of /" +backup
intitle:"index of /" site:example.com
```
Notes: Open directory listings often expose sensitive files.

### Search for Exposed Databases
```html {linenos=inline style=base16-snazzy}
inurl:phpmyadmin
intitle:"phpMyAdmin" "Welcome to phpMyAdmin"
inurl:admin/db/
```
Notes: Critical for finding misconfigured database portals.

### Locate Publicly Available Documents
```html {linenos=inline style=base16-snazzy}
site:example.com filetype:pdf "confidential"
site:example.com filetype:docx "internal use only"
site:example.com filetype:xlsx password
```
Notes: Leaked internal reports, budgets, and HR files.

### Find Leaked Credentials
```html {linenos=inline style=base16-snazzy}
filetype:txt intext:"password"
filetype:log intext:"password="
filetype:env DB_PASSWORD
```
Notes: Useful for uncovering mismanaged credentials.

### Search for Security Camera Feeds
```html {linenos=inline style=base16-snazzy}
inurl:view/index.shtml
inurl:"/webcapture.jpg?command=snap&"
intitle:"IP Camera" site:.com
```
Notes: Access unsecured camera feeds.

### Discover Vulnerable Devices and Servers
```html {linenos=inline style=base16-snazzy}
inurl:"/cgi-bin/" intext:"test page"
intitle:"Welcome to nginx!" -github
intitle:"Apache2 Ubuntu Default Page"
```
Notes: Default pages often indicate unfinished setups.

### Find Public User Profiles
```html {linenos=inline style=base16-snazzy}
site:linkedin.com/in "CEO" "Germany"
site:twitter.com "developer" "recruiting"
site:facebook.com intext:"works at"
```
Notes: Profiling key targets for social engineering.

### Find Public API Keys
```html {linenos=inline style=base16-snazzy}
filetype:env intext:API_KEY
filetype:json intext:"private_key"
site:github.com intext:"AWS_SECRET_ACCESS_KEY"
```
Notes: Great for cloud service reconnaissance.

### Discover Backup Files
```html {linenos=inline style=base16-snazzy}
inurl:/backup/ filetype:zip
inurl:/db_backup/ filetype:sql
intitle:"index of" "backup.tar.gz"
```
Notes: Exposed backup files often contain gold.

### Find Server Configuration Files
```html {linenos=inline style=base16-snazzy}
filetype:conf intext:server
filetype:ini intext:"DB_HOST"
filetype:yaml intext:"password:"
```
Notes: Misplaced configs reveal sensitive infrastructure info.

### Search for Exposed Source Code
```html {linenos=inline style=base16-snazzy}
filetype:php inurl:/var/www/
filetype:env intext:APP_ENV
site:github.com intext:"db_password"
```
Notes: Retrieve application source code and secrets.

### Locate Printers and Office Equipment
```html {linenos=inline style=base16-snazzy}
inurl:"/printers" intitle:"Printer Status"
inurl:"hp/device" "status"
inurl:printer
```
Notes: Many office devices are accessible publicly by mistake.

### Discover Misconfigured Email Servers
```html {linenos=inline style=base16-snazzy}
intitle:"WebMail Login"
inurl:/mail/
site:example.com intext:"SMTP server"
```
Notes: Email servers are high-value targets.

### Find Pages Listing Sensitive Internal Paths
```html {linenos=inline style=base16-snazzy}
intext:"Index of /admin"
intext:"Index of /secret"
intext:"Index of /test"
```
Notes: Lists unprotected staging or development areas.

### Search for Software Licenses and Keys
```html {linenos=inline style=base16-snazzy}
filetype:txt intext:"license key"
filetype:pdf intext:"product key"
site:github.com intext:"activation code"
```
Notes: Leaked licenses can lead to piracy or financial loss.

### Search for Medical Records or Hospital Systems
```html {linenos=inline style=base16-snazzy}
site:hospital.com filetype:pdf "patient record"
inurl:"/ehr/" intitle:"Electronic Health Record"
filetype:xls intext:"medical history"
```
Notes: Highly sensitive — proceed with extreme caution.

### Find WordPress Admin Pages
```html {linenos=inline style=base16-snazzy}
inurl:wp-admin
inurl:wp-login.php
site:example.com inurl:wp-content
```
Notes: Common during CMS assessments.

### Discover Payment Portals or Billing Pages
```html {linenos=inline style=base16-snazzy}
inurl:payment intitle:"payment"
inurl:checkout intitle:"checkout"
site:example.com "billing portal"
```
Notes: Useful for mapping financial transaction systems.

### Locate Public Logs or Debug Outputs
```html {linenos=inline style=base16-snazzy}
filetype:log intext:"error"
filetype:txt intext:"debug"
site:example.com filetype:log
```
Notes: Debug logs often disclose system architecture.

### Find Git Repositories Left Open
```html {linenos=inline style=base16-snazzy}
inurl:.git/config
intitle:"Index of" ".git"
site:example.com inurl:.git
```
Notes: Can expose the entire source code repo.

### Discover FTP Servers
```html {linenos=inline style=base16-snazzy}
intitle:"index of" inurl:ftp
inurl:ftp://
```
Notes: Open FTP servers often allow anonymous browsing.

### Search for JSON Sensitive Files
```html {linenos=inline style=base16-snazzy}
filetype:json intext:"password"
filetype:json intext:"credentials"
```
Notes: Modern web apps often accidentally expose `.json` credentials.

### Find Sensitive Environment Files
```html {linenos=inline style=base16-snazzy}
filetype:env "DB_PASSWORD"
filetype:env "AWS_ACCESS_KEY_ID"
```
Notes: `.env` files are a frequent leak vector.

### Locate Jenkins Servers
```html {linenos=inline style=base16-snazzy}
intitle:"Dashboard [Jenkins]"
inurl:8080
```
Notes: Jenkins CI/CD servers can expose internal projects.

### Find GitLab Instances
```html {linenos=inline style=base16-snazzy}
inurl:/users/sign_in intitle:"GitLab"
site:example.com inurl:gitlab
```
Notes: Targets version control systems and DevOps infrastructures.

### Discover Internal IP Address Leaks
```html {linenos=inline style=base16-snazzy}
intext:"10.0.0." | intext:"192.168.1."
filetype:log
```
Notes: Useful during network footprinting.

### Locate S3 Buckets
```html {linenos=inline style=base16-snazzy}
site:s3.amazonaws.com
inurl:s3.amazonaws.com
```
Notes: Public S3 buckets can leak huge amounts of data.

### Find File Upload Pages
```html {linenos=inline style=base16-snazzy}
inurl:upload intitle:upload
inurl:"file_upload"
```
Notes: Important when searching for file upload vulnerabilities.

### Search for Sensitive SQL Dumps
```html {linenos=inline style=base16-snazzy}
filetype:sql intext:"INSERT INTO"
filetype:sql intext:"DROP TABLE"
```
Notes: SQL dumps expose full database structures.

### Find Online MongoDB Admin Panels
```html {linenos=inline style=base16-snazzy}
intitle:"MongoDB" "Welcome"
inurl:27017
```
Notes: Exposed database UIs are high-risk.

### Search for Open Jenkins Scripts
```html {linenos=inline style=base16-snazzy}
inurl:/script intitle:"Jenkins"
inurl:8080/script
```
Notes: Can give access to server-side command execution.

### Find CRM Systems
```html {linenos=inline style=base16-snazzy}
intitle:"SugarCRM"
inurl:"/crm/"
```
Notes: Exposed CRM platforms can leak customer data.

### Discover Internal Company Wiki Pages
```html {linenos=inline style=base16-snazzy}
intitle:"Internal Wiki"
inurl:wiki
site:example.com
```
Notes: Internal documentation sometimes is public by mistake.

### Locate Public Ticketing Systems
```html {linenos=inline style=base16-snazzy}
intitle:"Open Tickets" inurl:/support/
site:example.com
```
Notes: Reveals customer complaints, internal problems.

### Find Jenkins Job Dumps
```html {linenos=inline style=base16-snazzy}
inurl:/job/ intitle:"Jenkins"
filetype:xml inurl:/jobs/
```
Notes: Discover information about internal build systems.

### Discover Public Financial Spreadsheets
```html {linenos=inline style=base16-snazzy}
filetype:xls intext:"salary"
filetype:xlsx intext:"budget"
```
Notes: Internal financial documents accidentally exposed.

### Find Kubernetes Dashboards
```html {linenos=inline style=base16-snazzy}
intitle:"Kubernetes Dashboard"
inurl:/api/v1/
```
Notes: Exposed Kubernetes clusters = total system compromise.


For a deep-dive, refer to [this GitHub](https://github.com/Just-Roma/DorkingDB?tab=readme-ov-file).