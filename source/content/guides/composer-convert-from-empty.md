---
title: Convert an Empty Upstream Drupal Site to a Composer Managed Site
description: Upgrade a Drupal site using an empty upstream by converting it to a Composer-managed Drupal site on the new Integrated Composer framework. 
type: guide
permalink: docs/guides/:basename
tags: [composer, site, workflow, D8, D9, D10]
contributors: [dustinleblanc, greg-1-anderson, stovak, kporras07]
reviewed: "2022-12-13"
contenttype: [doc]
innav: [true]
categories: [dependencies, update]
cms: [drupal8]
audience: [development]
product: [--]
integration: [--]
---

Use this guide to convert an empty upstream Drupal site to use Composer to manage deployments and dependencies, then switch from `empty` to the new Integrated Composer `drupal-composer-managed` upstream while remaining on Drupal.

<Partial file="drupal/see-landing.md" />


## Overview

Drupal sites on Pantheon have [Integrated Composer](/guides/integrated-composer) built-in to manage site dependencies.

By converting an empty upstream Drupal site to a Composer-managed site you will do the following:

* Remove dependencies that Composer manages from the existing Drupal site's Git repository, and have Composer manage those dependencies instead.

* Switch to the `drupal-composer-managed` Integrated Composer upstream.

The `drupal-composer-managed` Integrated Composer upstream works with Drupal, and following the `drupal-composer-managed` upstream will help keep your site current with any general configuration changes recommended by Pantheon.

Add Drupal core dependency instructions to `drupal/core-recommended` to keep the site on Drupal until you are ready to upgrade to the latest version of Drupal.

## Will This Guide Work for Your Site?

<Partial file="drupal/upgrade-site-requirements-from-empty.md" />

- You have not set up Continuous Integration or you no longer need it.

## Before You Begin

- This guide is written for users with access to Pantheon's [Multidev](/guides/multidev) feature. Pantheon support is not available to users who avoid the Multidev steps.

- The site owner should ensure the trusted host setting is up-to-date. Refer to the [Trusted Host Setting](/guides/php/settings-php#trusted-host-setting) documentation for more information.

- Source site may or may not be using a [nested docroot](/nested-docroot). If using it, you should prepend the paths in this document with "web" as needed.

<Alert title="Note" type="info">

  The steps in this process migrate a site and its content, but not the commit history. The new site will not maintain the site's existing commit history.

</Alert>

## Prepare the Local Environment

<Partial file="drupal/prepare-local-environment-no-clone.md" />

## Add the Integrated Composer Upstream in a New Local Branch

<Partial file="drupal-8-convert-to-composer-from-empty.md" />

### Troubleshooting

#### Provided host name not valid

If you receive the error message "The provided host name is not valid for this server.", then update your `settings.php` file with a trusted host setting. Refer to the [Trusted Host Setting](/guides/php/settings-php#trusted-host-setting) documentation for more information.

## Change Upstreams

Your Pantheon site is now configured to use the latest version of Drupal Integrated Composer upstream. To continue tracking additional changes to the Pantheon upstream, change the upstream your site is tracking with Composer:

```bash{promptUser:user}
terminus site:upstream:set $SITE drupal-composer-managed
```

Following the `drupal-composer-managed` upstream will help keep your site up to date with any general configuration changes recommended by Pantheon. The dependency you added above on `drupal/core-recommended` will keep you on your current version of Drupal until you are ready to upgrade to the latest version of Drupal.

## Working With Dependency Versions

<Partial file="composer-updating.md" />

## More Resources

- [Composer Fundamentals and Workflows](/guides/composer)

- [WordPress with Composer on Pantheon](/guides/wordpress-composer)
