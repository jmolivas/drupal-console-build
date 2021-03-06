# Drupal Console Build




## Setup


Include this library in your Drupal project:

````sh
composer require "smalot/drupal-console-build:*"
````

Must be added in your `settings.yml` file.

````yaml
services:
  console.deploy_run:
    class: Smalot\Drupal\Console\Build\Command\RunCommand
    arguments: []
    tags:
      - { name: drupal.command }

  console.deploy_list:
    class: Smalot\Drupal\Console\Build\Command\ListCommand
    arguments: []
    tags:
      - { name: drupal.command }
````


## Sample config file


````yaml
stages:
    - compile
    - cache
    - features

compile_css:
    stage: compile
    script:
        - echo "Compile SASS file into CSS"
        - compass build
    allow_failure: true

cache_rebuild:
    stage: cache
    script:
        - echo "Cache Rebuild"
        - cd web && drush cache-rebuild

features_revert:
    stage: features
    script:
        - echo "Features revert all"
        - drush fra -y
    except:
        - master
````


## Usage

Run all tasks

````sh
drupal build:run
````

Run all tasks of one stage

````sh
drupal build:run --stage=cache
````

Run speficic tasks

````sh
drupal build:run compile_css cache_rebuild
````
