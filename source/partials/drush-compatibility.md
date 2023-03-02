---
contenttype: [partial]
categories: [cli]
cms: [drupal]
product: [--]
integration: [drush]
tags: [--]
reviewed: ""
---

Drush only supports one install method. Your Drupal site is required to be built with Composer and your Drush must be compatible and listed as a dependency. 

See the [Drush Drupal Compatibility chart](https://www.drush.org/latest/install/#drupal-compatibility) for version compatibility information. Consider the following information when determining which Drush version is best suited for your site:

* Drush 10 is available with the [addition of the  `pantheon.yml` file](#configure-drush-version), or for [site-local installation](#site-local-drush-usage). It requires Drupal 9 or higher, [Composer](/guides/composer/), and PHP 7.1 or higher.

* [Drush 9](https://docs.drush.org/en/9.x/) and Drush 10 only work on Drupal 8.4 and higher.

* [Drush 8](https://docs.drush.org/en/8.x/) Drush 8 is the only recommended version to use in your `pantheon.yml` file. It is compatible with Drupal 7.

* Drush 5 and Drush 7 are available on Pantheon if needed. However, both versions are listed as [unsupported](https://docs.drush.org/en/8.x/install/#drupal-compatibility) by the Drush maintainers, and should be avoided unless absolutely necessary.

<Alert title="Note" type="info">

We highly recommend running Drush version 8.3.2 or higher when running Drush locally.

</Alert>

### Drush Requirements and Compatibility

| Drush Version  | PHP Version | End of Life     | Drupal 7| Drupal 8 | Drupal (Latest Version)
| ------------- |-------------|-------------|-------------   |------------- |-------------  |
| 11*      | 7.4+     | TBD      |  <span style="color:red">❌ </span>    |<span style="color:red">❌ </span> |<span style="color:green">✔</span>  |
| 10      | 7.1+ | Jan. 2022| ❌ |Compatible, unsupported | Compatible, unsupported| 
| 9      | 5.6+     | May 2020  | ❌      |Compatible, unsupported |<span style="color:red">❌ </span>|
| 8     | 5.4.5+     | Nov. 2022| <span style="color:green">✔</span>    |Supported, not recommended|<span style="color:red">❌ </span>|
| 7      | 5.3.0+     | July 2017| Compatible, unsupported    |<span style="color:red">❌ </span>  |<span style="color:red">❌ </span>|
| 6      | 5.3.0+     | Dec. 2015| Compatible, unsupported    |<span style="color:red">❌ </span> |<span style="color:red">❌ </span>|
| 5     | 5.2.0+    | May 2015 | Compatible, unsupported    |<span style="color:red">❌ </span>  |<span style="color:red">❌ </span> |

*Drush 11 is not available through `pantheon.yml`. Drush 11 can only be used through a [site-local installation](#site-local-drush-usage).

Refer to our guide on [Upgrading PHP Versions](/guides/php/php-versions) for more information.