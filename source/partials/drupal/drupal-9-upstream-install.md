---
contenttype: [partial]
categories: [dependencies, custom-upstream]
cms: [drupal9]
product: [integrated-composer]
integration: [--]
tags: [--]
reviewed: ""
---

1. Use this link to create a new Drupal Integrated Composer site from the Upstream: [https://dashboard.pantheon.io/sites/create?upstream_id=897fdf15-992e-4fa1-beab-89e2b5027e03](https://dashboard.pantheon.io/sites/create?upstream_id=897fdf15-992e-4fa1-beab-89e2b5027e03). On the **Create Your Pantheon Site** page:

   - Enter a name for the site.

   - Choose an workspace if applicable.

   - Choose a region for the site. Click **Continue**.

   After you click Continue, it will take several minutes for the platform to set everything up.

1. In the **<span class="glyphicons glyphicons-wrench"></span> Dev** tab on the Dashboard, set the site's Development Mode to [SFTP](/guides/sftp).

1. Still in the Dev tab, click **Visit Development Site** and follow the prompts to complete the CMS installation.

1. Return to the Dev tab, set the site's Development Mode to Git, and [clone the site locally](/guides/local-development#get-the-code).

1. In your local terminal, from the project root directory, run `composer install`.
