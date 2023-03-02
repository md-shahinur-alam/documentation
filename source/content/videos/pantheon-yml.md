---
title: The Pantheon.yml Configuration File
description: Configure our platform for your needs.
contributors:  [davidneedham]
permalink:  docs/videos/:basename
tags: [site, code]
layout: video
type: video
searchboost: 50
contenttype: [doc]
innav: [true]
categories: [config]
cms: [drupal, wordpress]
audience: [development]
product: [--]
integration: [nginx, varnish]
---

<Youtube src="SM3QlNGgyBo" title="Pantheon.yml" />

The [pantheon.yml](/pantheon-yml) file provides a way to configure platform options for any project. This file is version controlled and can be configured differently for each environment.

Configurable items include: PHP version, Drush version, protected paths, nested docroot, and [Quicksilver](/guides/quicksilver) platform hooks.

Items such as Nginx configuration or Varnish settings are not editable.

Let's look at an example of upgrading a site to PHP7 with pantheon.yml.

We’ll start by creating a [Multidev](/guides/multidev) environment for testing in a separate and safe space.

Now we’ll create a pantheon.yml file for the Multidev branch, set the PHP version to 7, and commit the pantheon.yml file to the Multidev environment.

The Multidev environment should be thoroughly tested to make sure there are no issues with PHP7 and the codebase.

It's also a good idea to check the [PHP error logs](/guides/logs-pantheon) to see if any issues are reported.

When the site has been tested to your satisfaction, the Multidev environment can be merged into Dev and eventually deployed into Test and Live.

The pantheon.yml file allows you to change platform configuration, is tracked in version control, and can be specific to any environment.
