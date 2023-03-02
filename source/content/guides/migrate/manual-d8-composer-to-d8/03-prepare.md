---
title: Update a Drupal Site That Is Managed With Composer From Another Platform
subtitle: Prepare
description: Learn how to prepare your site for migration.
contenttype: [guide]
innav: [false]
categories: [migrate]
cms: [drupal]
audience: [development]
product: [--]
integration: [--]
tags: [code, launch, migrate, site, updates, composer]
contributors: [wordsmither]
layout: guide
showtoc: true
permalink: docs/guides/manual-d8-composer-to-d8/prepare
anchorid: prepare
editpath: migrate/manual-d8-composer-to-d8/03-prepare.md
reviewed: "2022-12-13"
---

Complete all steps in this section to ensure that your site is ready to be migrated to Pantheon.

## Create a New Site

<Partial file="migrate/create-new-drupal-site.md" />

### Set Drupal Core Version

The previous step created a site using the latest version of Drupal. To remain on the current version, set the core version as follows: 

<Partial file="drupal/core-version-remain-on-d8.md" />

## Prepare the Local Environment

<Partial file="drupal/prepare-local-environment-no-clone.md" />

### Create a Local Copy of the Old Site's Code

<Partial file="migrate/drupal-create-local.md" />

### Retrieve a Local Copy of the Pantheon Site's Code

<Partial file="migrate/drupal-get-local.md" />
