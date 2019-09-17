# Proof of Concept: Islandora 8 Migrate Files (Apollo Edition)

This repository consists of two modules:

1. migrate_cdm: Uses the Migrate API to load Tiff masters, metadata from a CSV, and MADS authority records from the Library of Congress.
2. data: the source data we will migrate (see below).

# Source Data

The source data used for this proof of concept came from the [Project Apollo Archive Flickr Albums](https://www.flickr.com/photos/projectapolloarchive/albums) and are in the public domain although some of the descriptive metadata was supplemented and the original Jpegs were converted to Tiff.

# Running

Note: using drush with migrate_tools is optional, but the instructions assume it is installed.

0. Install Islandora 8, including islandora_defaults.
0. Copy the data directory to your drupal web root (e.g. for islandora the default drupal web root is `/var/www/html/drupal/web` and the data directory is `/var/www/html/drupal/web/data`).
0. Clone this git repo to your modules directory. (`git clone https://github.com/seth-shaw-unlv/claw-migrate-files-poc.git`)
0. Enable the modules. E.g. `drush en -y migrate_apollo`.
0. Run the migration. E.g. `drush -l http://localhost:8000 mim --userid=1 --all`. *Note: drush must be run by the webserver user because the claw_file migration copies files to the "public://directory". E.g.* `sudo -u www-data drush -l http://localhost:8000 mim --userid=1 --all` *if you are using Ubuntu. Also, the userid flag is specific to the migrate:import command, it provides the necessary user information to the JWT authentication to enable derivatives.*
0. See a wonderful list of the newly migrated images on your Drupal site's front page!

# Combining People and Subject entities in a single column

This example splits out people that are subjects from topics that are subjects
into separate columns. This allows us to perform entity lookups on each column
for the matching content type.

In some cases, however, topic columns include items that could be either people or topics.
This requires us to perform a single lookup across multiple content types,
something the existing migrate_plus module doesn't support. I've
[created a patch and issue](https://www.drupal.org/project/migrate_plus/issues/2960251) to address the issue.
Until it is merged or some other solution is found, we will either have to
patch migrate_plus, or extend the process plugin for this small modification.
