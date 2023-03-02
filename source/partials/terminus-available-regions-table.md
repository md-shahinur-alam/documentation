---
contenttype: [partial]
categories: [cli]
cms: [--]
product: [terminus]
integration: [--]
tags: [--]
reviewed: ""
---

Available region codes:

| Name           | Code |
|----------------|------|
| Australia      | au   |
| Canada         | ca   |
| European Union | eu   |
| United States  | us   |

For example (replace `my-eu-site-name`, `My EU Site Name`, `WordPress`, `My Workspace Name`, and the `eu` region accordingly):

```bash{outputLines: 2-4}
terminus site:create my-eu-site-name "My EU Site Name" "WordPress" --org "My Workspace Name" --region eu
[notice] Creating a new site...
[notice] Deploying CMS...
[notice] Deployed CMS
```
