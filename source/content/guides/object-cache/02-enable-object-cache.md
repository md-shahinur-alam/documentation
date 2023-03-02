---
title: Object Cache (formerly Redis)
subtitle: Enable Object Cache
description: Learn how to enable Object Cache on your site.
contenttype: [guide]
innav: [false]
categories: [cache]
cms: [--]
audience: [development]
product: [--]
integration: [--]
tags: [cache, plugins, modules, database]
contributors: [cityofoaksdesign, carolynshannon, jms-pantheon, whitneymeredith]
layout: guide
permalink: docs/guides/object-cache/enable-object-cache
anchorid: enable-object-cache
reviewed: "2022-12-13"
---

This section provides information on how to enable Object Cache.

<TabList>

<Tab title="WordPress" id="wp-install" active={true}>

1. Navigate to your Pantheon Site Dashboard, select **Settings**, select **Add Ons**, then select **Add**. It might take a couple of minutes for the Object Cache server to come online.

1. Install the [WP Redis](https://wordpress.org/plugins/wp-redis/) plugin via SFTP or Git. You can also install the plugin via [Terminus](/terminus) if you [set the connection mode to SFTP](/guides/sftp) and run:

  ```bash{promptUser: user}
  terminus wp <site>.<env> -- plugin install wp-redis
  ```

  If you use site networks, you must add the site URL by adding to the command: 

  ```bash{promptUser: user}
  terminus wp <site>.<env> -- plugin install wp-redis --url=<url>
  ```

1. Create a new file named `wp-content/object-cache.php` that contains the following:

  ```php:title="object-cache.php"
  <?php
  # This is a Windows-friendly symlink
  $objectCacheFile = WP_CONTENT_DIR . '/plugins/wp-redis/object-cache.php';
  if (!empty($_ENV['PANTHEON_ENVIRONMENT']) && !empty($_ENV['CACHE_HOST']) && file_exists($objectCacheFile)) {
      require_once $objectCacheFile;
  }
  ```

  This file is a symlink to the `/plugins/wp-redis/object-cache.php` file. Use SFTP or Git to commit the new file to the Dev environment.

1. Navigate to the Dev environment's WordPress Dashboard, and verify the installation by selecting **Drop-ins** from the Plugins section:

  ![The object-cache Drop-In Plugin](../../../images/redis-dropin-plugin.png "The object-cache plugin, visible in the Drop-ins section of Plugins.")

  You can upgrade by the normal Plugin update mechanism in WordPress or via Terminus when a new version of the WP Redis plugin is released:

  ```bash{promptUser: user}
  terminus wp <site>.<env> -- plugin update wp-redis
  ```

<Alert title="Note" type="info">

[WP Redis](https://wordpress.org/plugins/wp-redis/) is loaded via a drop-in file, so there's no need to activate it on your WordPress sites.

</Alert>

<Accordion title="Explore Advanced Install Methods (Optional)" id="advance-installs" icon="lightbulb">

#### Install via Composer

1. Set the Dev environment's connection mode to Git from within the Site Dashboard or via Terminus:

  ```bash{promptUser: user}
  terminus connection:set <site>.<env> git
  ```

1. [Clone the site's codebase](/guides/git/git-config#clone-your-site-codebase) if you have not done so already.

1. Use the following within `composer.json` to install the WP Redis plugin as a drop-in via Composer using [koodimonni/composer-dropin-installer](https://github.com/Koodimonni/Composer-Dropin-Installer):

  ```json:title=composer.json
  "repositories": {
    "wpackagist": {
      "type": "composer",
      "url": "https://wpackagist.org"
    }
  },
  "require": {
    "composer/installers": "^1.0.21",
    "koodimonni/composer-dropin-installer": "*",
    "wpackagist-plugin/wp-redis": "0.6.0"
    },
    "extra": {
      "installer-paths": {
        "wp-content/plugins/{$name}/": ["type:wordpress-plugin"]
        },
      "dropin-paths": {
          "wp-content": [
          "package:wpackagist-plugin/wp-redis:object-cache.php"
        ]
      }
    }
  ```

1. Run `composer install` to install WP Redis into the `wp-content` directory.

1. Use git status to verify your local state, then commit and push your code to Pantheon:

  ```bash{promptUser: user}
  git status
  git commit --all -m "Initiate composer, require custom code"
  git push origin master
  ```

</Accordion>

</Tab>

<Tab title="Drupal 7" id="d7-install">

<Alert title="Note" type="info">

This configuration uses the `Redis_CacheCompressed` class for better performance. This requires the Redis module version 3.13 or later. For versions before 3.13, use `Redis_Cache` in step 4 instead.

</Alert>

1. Navigate to your Pantheon Site Dashboard, select **Settings**, select **Add Ons**, then select **Add** to enable the Redis cache server. It might take a couple of minutes for the Redis server to come online.

1. Add the [Redis](https://www.drupal.org/project/redis) module from Drupal.org. You can install and enable the module from the command line using [Terminus](/terminus):

  ```bash{promptUser: user}
  terminus remote:drush <site>.<env> -- en redis -y
  ```

1. Ignore the directions bundled with the Redis module. Pantheon automatically manages the following `settings.php`/`$conf`/`variable_get` items for you:
   - `redis_client_host`
   - `redis_client_port`
   - `redis_client_password`

1. Edit `sites/default/settings.php` to add the Redis cache configuration. These are the **mandatory**, required Redis configurations for every site.

  ```php:title=settings.php
  // All Pantheon Environments.
  if (!empty($_ENV['PANTHEON_ENVIRONMENT']) && !empty($_ENV['CACHE_HOST'])) {
    // Use Redis for caching.
    $conf['redis_client_interface'] = 'PhpRedis';
    // Point Drupal to the location of the Redis plugin.
    $conf['cache_backends'][] = 'sites/all/modules/redis/redis.autoload.inc';
    // If you've installed your plugin in a contrib directory, use this line instead:
    // $conf['cache_backends'][] = 'sites/all/modules/contrib/redis/redis.autoload.inc';
    $conf['cache_default_class'] = 'Redis_CacheCompressed';
    $conf['cache_prefix'] = array('default' => 'pantheon-redis');
    // Do not use Redis for cache_form (no performance difference).
    $conf['cache_class_cache_form'] = 'DrupalDatabaseCache';
    // Use Redis for Drupal locks (semaphore).
    $conf['lock_inc'] = 'sites/all/modules/redis/redis.lock.inc';
    // Or if you've installed the redis module in a contrib subdirectory, use:
    // $conf['lock_inc'] = 'sites/all/modules/contrib/redis/redis.lock.inc';
  }
  ```

1. Enable the module via `/admin/modules`, if you haven't already done so, using Terminus.

1. Navigate to the dashboard and click **Connection Info** to verify that Redis is enabled. If you see the Redis cache connection string, Redis is enabled.

1. Visit `/admin/config/development/performance/redis` and open **Connection Information** to verify the connection.

<Accordion title="Database Cleanup (optional)" id="database-cleanup-d7" icon="lightbulb">

After enabling Redis, there are cache tables in the database that are no longer being used. Even when the Drupal cache is cleared, these tables will not be emptied. For sites that were live for awhile before Redis was enabled, there could be significant amounts of data in these tables. Removing this data could increase the speed of cloning, exporting, and backing up the database.

1. [Connect directly to MySQL](/guides/mariadb-mysql/mysql-access) and run the command below to view the cache:

  ```sql
  SELECT table_name FROM information_schema.tables WHERE table_name LIKE 'cache%' AND table_name != 'cache_form';
  ```

 This returns a list of all the cache tables in the database. These are safe to empty, but don't remove the tables themselves in case Redis is disabled in the future.

1. Run the command below on each table, replacing `<tablename>` with the name of the cache table, to empty the cache:

  ```sql
  TRUNCATE TABLE `<tablename>`;
  ```

</Accordion>

</Tab>

<Tab title="Drupal 9.3+ / Composer-managed" id="d9-install">

1. Navigate to your Pantheon Site Dashboard, select **Settings**, select **Add Ons**, then select **Add** to enable the Redis cache server. It might take a couple of minutes for the Redis server to come online.

1. Clone the code repository and run the following from the project root:

   ```shell{promptUser: user}
   terminus connection:set $SITE.dev git
   terminus redis:enable $SITE
   composer require drupal/redis
   git add . && git commit -m "Add drupal/redis dependency" && git push origin master
   ```

1. Enable the new Redis module and export configuration:

   ```shell{promptUser: user}
   terminus connection:set $SITE.dev sftp
   terminus drush $SITE.dev -- en redis -y
   terminus drush $SITE.dev -- config:export -y
   terminus env:commit $SITE.dev --message="Enable Redis, export configuration"
   ```

1. Edit `sites/default/settings.php` to add the Redis cache configuration. These are **mandatory**, required Redis configurations for every site:

   ```php:title=sites/default/settings.php
   // Configure Redis

   if (defined(
    'PANTHEON_ENVIRONMENT'
  ) && !\Drupal\Core\Installer\InstallerKernel::installationAttempted(
  ) && extension_loaded('redis')) {
    // Set Redis as the default backend for any cache bin not otherwise specified.
    $settings['cache']['default'] = 'cache.backend.redis';

    //phpredis is built into the Pantheon application container.
    $settings['redis.connection']['interface'] = 'PhpRedis';

    // These are dynamic variables handled by Pantheon.
    $settings['redis.connection']['host'] = $_ENV['CACHE_HOST'];
    $settings['redis.connection']['port'] = $_ENV['CACHE_PORT'];
    $settings['redis.connection']['password'] = $_ENV['CACHE_PASSWORD'];

    $settings['redis_compress_length'] = 100;
    $settings['redis_compress_level'] = 1;

    $settings['cache_prefix']['default'] = 'pantheon-redis';

    $settings['cache']['bins']['form'] = 'cache.backend.database'; // Use the database for forms

    // Apply changes to the container configuration to make better use of Redis.
    // This includes using Redis for the lock and flood control systems, as well
    // as the cache tag checksum. Alternatively, copy the contents of that file
    // to your project-specific services.yml file, modify as appropriate, and
    // remove this line.
    $settings['container_yamls'][] = 'modules/contrib/redis/example.services.yml';

    // Allow the services to work before the Redis module itself is enabled.
    $settings['container_yamls'][] = 'modules/contrib/redis/redis.services.yml';

    // Manually add the classloader path, this is required for the container
    // cache bin definition below.
    $class_loader->addPsr4('Drupal\\redis\\', 'modules/contrib/redis/src'); 

    // Use redis for container cache.
    // The container cache is used to load the container definition itself, and
    // thus any configuration stored in the container itself is not available
    // yet. These lines force the container cache to use Redis rather than the
    // default SQL cache.
    $settings['bootstrap_container_definition'] = [
      'parameters' => [],
      'services' => [
        'redis.factory' => [
          'class' => 'Drupal\redis\ClientFactory',
        ],
        'cache.backend.redis' => [
          'class' => 'Drupal\redis\Cache\CacheBackendFactory',
          'arguments' => [
            '@redis.factory',
            '@cache_tags_provider.container',
            '@serialization.phpserialize',
          ],
        ],
        'cache.container' => [
          'class' => '\Drupal\redis\Cache\PhpRedis',
          'factory' => ['@cache.backend.redis', 'get'],
          'arguments' => ['container'],
        ],
        'cache_tags_provider.container' => [
          'class' => 'Drupal\redis\Cache\RedisCacheTagsChecksum',
          'arguments' => ['@redis.factory'],
        ],
        'serialization.phpserialize' => [
          'class' => 'Drupal\Component\Serialization\PhpSerialize',
        ],
      ],
    ];
}

   ```

   <Alert title="Note" type="info">

   The above Redis cache configuration should be placed in `sites/default/settings.php`, rather than `settings.pantheon.php`, to avoid conflicts with future upstream updates.

   </Alert>

</Tab>

</TabList>

## More Resources

- [Caching in Drupal Views](/drupal-caching-views)
- [Frontend Performance Caching](/guides/frontend-performance/caching)
