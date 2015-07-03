silex-sitemap-service-provider
==============================
[![Build Status](https://travis-ci.org/tommy-muehle/silex-sitemap-service-provider.svg?branch=master)](https://travis-ci.org/tommy-muehle/silex-sitemap-service-provider)

###Installation

composer require tm/silex-sitemap-service-provider

###Example Basic Usage

First you have to register the ServiceProvider:
```php
    $app->register(new TM\Provider\SitemapServiceProvider());
```

Then implement the route for the sitemap.xml with your custom logic:
```php
    $app->get('sitemap.xml', function () use ($app) {
  
      $host = $app['request']->getSchemeAndHttpHost();
      
      $sitemap = $app['sitemap'];
      $sitemap->addEntry($host . '/', 1, 'yearly');
      
      $entities = $app['repository.entity']->findAll(50000);
  
      foreach ($entities as $entity) {
        $entityLoc = $app['url_generator']->generate('entity', array('entity' => $entity->getId()));
        $sitemap->addEntry($host . $entityLoc, 0.8, 'monthly', $entity->getLastModified());
      }
  
      return $sitemap->generate();
    })
    ->bind('sitemap');
```
