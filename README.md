# Proof of Concept: CLAW Migrate Files (Apollo Edition)

This repository consists of two modules:

1. unlv_image: A local implementation of the Islandora Image module to include additional metadata fields.
2. migrate_cdm: Uses the Migrate API to load Tiff masters, metadata from a CSV (patterned after the [Move to Islandora Kit sample metadata CSV](https://github.com/MarcusBarnes/mik/blob/master/tests/assets/csv/sample_metadata.csv)), and MADS authority records from the Library of Congress.

*Note:* This proof-of-concept assumes that new content types will be created for each metadata profile/object type pair; ergo, this proof-of-concept includes a new content type called UNLV_image which parallel's islandora_image but adds some node entity references. However, CLAW is under active development which will cause this example to break from time to time. I intend to continue updating this example as the current dev-version of CLAW develops.

# Source Data

The source data used for this proof of concept came from the [Project Apollo Archive Flickr Albums](https://www.flickr.com/photos/projectapolloarchive/albums) and are in the public domain although some of the descriptive metadata was supplemented and the original Jpegs were converted to Tiff.

# Running

Note: using drush with migrate_tools is optional, but the instructions assume it is installed.

0. Install the prerequisite modules (islandora_image, migrate_plus, and migrate_source_csv) and their dependencies. E.g. `composer require islandora/islandora_image drupal/migrate_tools:^4.0 drupal/migrate_source_csv`.
1. [Patch migrate_plus to allow looking up entities across multiple content types](https://www.drupal.org/project/migrate_plus/issues/2960251).
2. Copy the data directory to your drupal web root (e.g. in my tests the drupal web root is `/var/www/drupalvm/drupal/web` and the data directory is `/var/www/drupalvm/drupal/web/data`).
3. Copy the migrate_cdm and unlv_image directories to your modules directory.
4. Enable the modules. E.g. `drush en -y migrate_tools migrate_apollo`.
5. Run the migration. E.g. `drush mim --all`.
6. See a wonderful list of the newly migrated images on your Drupal site's front page!

# The migrate_plus Patch

Previously this example split out people that were subjects from topics that
were subjects. In that case we could perform entity lookups on each column for
the matching content type.

The [Move to Islandora Kit sample metadata](https://github.com/MarcusBarnes/mik/blob/master/tests/assets/csv/sample_metadata.csv),
however, combines them into a single column. This requires us to perform a
single lookup across multiple content types, something the existing migrate_plus
module doesn't support. I've created a patch and issue to address the issue.
Until it is merged or some other solution is found, we will either have to
patch migrate_plus, or extend the process plugin for this small modification.
