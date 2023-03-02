---
title: Update a Drupal Site with Integrated Composer to the Latest Version of Drupal
description: Upgrade Pantheon Drupal sites with Integrated Composer to the latest version of Drupal.
type: guide
permalink: docs/guides/:basename
contenttype: [doc]
innav: [true]
categories: [update]
cms: [drupal9]
audience: [development]
product: [integrated-composer]
integration: [--]
tags: [code, launch, migrate, site, updates]
contributors: [dustinleblanc, greg-1-anderson, stovak]
reviewed: "2022-12-13"
---

This page shows how to upgrade an existing Pantheon-hosted Drupal site using Integrated Composer to the latest version of Drupal using Integrated Composer.

<Partial file="drupal/see-landing.md" />


## Overview

The goal of this upgrade is to set the Drupal core dependency to Drupal (Latest Version). This upgrade allows Composer to manage dependencies in the new site.

<Alert title="Note"  type="info" >

Follow the steps in the [Composer Conversion Guide](/guides/composer-convert) to ensure that your site is ready for upgrade to the latest version of Drupal.

</Alert>

## Will This Guide Work for Your Site?

You must be on Drupal using Integrated Composer to use this guide. You can confirm that Integrated Composer is set up by checking that your site has the [Pantheon drupal-composer-managed repo](https://github.com/pantheon-upstreams/drupal-composer-managed) in its upstream. 

Confirm the site requirements in the sections below before upgrading to the latest version of Drupal.

### Verify the Repository

1. Check that your site has the [Pantheon drupal-composer-managed repo](https://github.com/pantheon-upstreams/drupal-composer-managed) in its upstream.

1. Use Terminus to Confirm the drupal-composer-managed Upstream:

  ```bash
  terminus site:info $SITE 
  ```

  The following values indicate that a site is using the `drupal-composer-managed` upstream:

  - The `Framework` is `drupal8`
  - The `Upstream` includes `https://github.com/pantheon-upstreams/drupal-composer-managed`

  The following is an abridged example of the output for the `terminus site:info $SITE` command, if the site upstream is set to `drupal-composer-managed`:

  ```bash{outputLines:2-18}
  terminus site:info $SITE
  ------------------ -------------------------------------------------------------------------------------
  ID                 abdc3ea1-fe0b-1234-9c9f-3cxeAA123f88
  Name               anita-drupal
  Label              AnitaDrupal
  Created            2019-12-02 18:28:14
  Framework          drupal8
  ...
  Upstream           897fdf15-992e-4fa1-beab-89e2b5027e03: https://github.com/pantheon-upstreams/drupal-composer-managed
  ...
  ------------------ -------------------------------------------------------------------------------------
  ```

## Prepare the Local Environment

<Partial file="drupal/prepare-local-environment-no-clone.md" />

### Apply All Available Upstream Updates

<Partial file="drupal-apply-upstream-updates-drupal-recommended.md" />

<Partial file="drupal-8-to-drupal-upgrade.md" />

## Troubleshooting

<Partial file="composer-updating.md" />

## More Resources

- [Composer Fundamentals and Workflows](/guides/composer)
- [Drupal 10 on Pantheon](/drupal-10)
