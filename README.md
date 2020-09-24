# DruxtJS demo Drupal JSON:API backend.

This is the Drupal 9 codebase for the JSON:API backend used by the DruxtJS demonstration site.

This codebase is derived from the [amazeeio/drupal-example-simple](https://github.com/amazeeio/drupal-example-simple/tree/9.x) template.

## Requirements

* [docker](https://docs.docker.com/install/)


* [pygmy](https://pygmy.readthedocs.io/) `gem install pygmy` (you might need `sudo` for this depending on your Ruby configuration)

**OR**

* [Lando](https://docs.lando.dev/basics/installation.html#system-requirements)

## Local environment setup - pygmy

1. Checkout this project repo and confirm the path is in Docker's file sharing config - https://docs.docker.com/docker-for-mac/#file-sharing

    ```bash
    git clone https://github.com/druxt/demo-api.druxtjs.org.git demo-api.druxtjs.org && cd $_
    ```

2. Make sure you don't have anything running on port 80 on the host machine (like a web server) then run `pygmy up`

3. Build and start the build images:

    ```bash
    docker-compose up -d
    docker-compose exec cli composer install
    ```

4. Visit the new site @ `http://demo-api-druxtjs-org.docker.amazee.io`

* If any steps fail, you're safe to rerun from any point.
Starting again from the beginning will just reconfirm the changes.

Now jump to the section below about Druxt setup.

## Local environment setup - Lando

This repository is set up with a `.lando.yml` file, which allows you to use Lando instead of pygmy for your local development environment.

1. [Install Lando](https://docs.lando.dev/basics/installation.html#system-requirements).

2. Checkout the project repo and confirm the path is in Docker's file sharing config - https://docs.docker.com/docker-for-mac/#file-sharing

    ```bash
    git clone https://github.com/druxt/demo-api.druxtjs.org.git demo-api.druxtjs.org && cd $_
    ```

3. Make sure you have pygmy stopped. Run `pygmy stop` to be sure.

4. We already have a Lando file in this repository, so we just need to run the following command to get Lando up:

 ```bash
lando start
```

5. Install your Drupal site with Drush:

```bash
lando drush si -y
```

6. And now we have a fully working local Drupal site on Lando! For more information on how to deploy your site, check out our documentation or our deployment demo.

## Druxt setup

1. Install Drupal using the demo_umami profile.

```bash
docker-compose exec cli drush si demo_umami -y
```

2. Enable Drupal module/s required by DruxtJS.

 ```bash
docker-compose exec cli drush en druxt jsonapi_views -y
```
This should also enable the jsonapi_menu_items module.

3. Log into the site by generating a one time login link.

 ```bash
docker-compose exec cli drush uli
```

4. Configure druxt required permission.

Navigate to the following path once logged in `/en/admin/people/permissions`

* Enable `Access Druxt JSON API resources.` permission for the Anonymous user.
* Scroll to the bottom of the page and click `Save permissions`
* Now your [demo.druxtjs.org] (https://github.com/druxt/demo.druxtjs.org) frontend will be able to connect and work as expected, be sure to update the .env file with the BASE_URL for the backend you have just built.
