---
title: Upgrade a Drupal Site with Multidev to the Latest Version of Drupal Using Multidev
subtitle: Introduction
description: Learn how to migrate a site to the latest version of Drupal using Multidev
tags: [code, launch, migrate, site, updates]
contributors: [wordsmither]
layout: guide
showtoc: true
permalink: docs/guides/drupal-hosted-md
anchorid: drupal-hosted-md
editpath: drupal/drupal-hosted-md/01-introduction.md
reviewed: "2021-03-31"
contenttype: [guide]
innav: [true]
categories: [update]
cms: [drupal8, drupal9, drupal]
audience: [development]
product: [composer, terminus]
integration: [--]
---

This guide will show you how to migrate a site that meets the following criteria to the latest version of Drupal:

| <i class="fa fa-cloud"></i><br/> Current Host | <i class="fa fa-wrench"></i><br/> How Site Was Created <Popover title="Site Creation" content="What is the method you used to create the site?" /> | <i class="fa fa-exclamation-circle"></i><br/> Additional Requirements <Popover title="Additional Requirements" content="Any other features that must be in place, or that are desired." /> |
|:---------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|                   Pantheon                    |                                                                     Dashboard                                                                      |                                                                                          Multidev                                                                                          |

<Partial file="drupal/see-landing.md" />

<Alert title="Note" type="info" >

This upgrade will not maintain your site’s commit history.

</Alert>

## Requirements

<Alert title="Multidev Required" type="danger">

To maintain best practices and to avoid difficult, time-consuming repairs to the site, this doc is written for users with access to Pantheon's [Multidev](/guides/multidev) feature.

Pantheon support is not available to users who avoid the Multidev steps.

</Alert>

<Partial file="drupal/upgrade-site-requirements-new.md" />

## More Resources

- [Composer Fundamentals and Workflows](/guides/composer)
