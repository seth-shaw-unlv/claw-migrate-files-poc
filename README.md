# Proof of Concept: CLAW Migrate Files (Apollo Edition)

This repository consists of two modules:

1. unlv_image: A local implementation of the Islandora Image module to include additional metadata fields.
2. migrate_cdm: Uses the Migrate API to load Tiff masters, metadata from a CSV, and authority records from the Library of Congress.

*Note:* This proof-of-concept assumes that new content types will be created for each metadata profile/object type pair; ergo, this proof-of-concept includes a new content type called UNLV_image which parallel's islandora_image but adds some node entity references. However, the CLAW team is exploring alternative strategies for managing descriptive metadata which will make portions of this example out of date (hopefully soon).

# Source Data

The source data used for this proof of concept came from the [Project Apollo Archive Flickr Albums](https://www.flickr.com/photos/projectapolloarchive/albums) and are in the public domain although some of the descriptive metadata was supplemented and the original Jpegs were converted to Tiff.

# Running

Note: using drush with migrate_tools is optional, but the instructions assume it is installed.

1. Copy the data directory to your drupal web root (e.g. in my tests the drupal web root is `/var/www/drupalvm/drupal/web` and the data directory is `/var/www/drupalvm/drupal/web/data`).
2. Copy the migrate_cdm and unlv_image directories to your modules directory.
3. Enable the modules. E.g. `drush en -y migrate_tools migrate_apollo`.
4. Run the migration. E.g. `drush mim --all`.
5. See a wonderful list of the newly migrated images on your Drupal site's front page!
