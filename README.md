# Proof of Concept: CLAW Migrate Files

This repository consists of two modules:

1. unlv_image: A local implementation of the Islandora Image module to include additional metadata fields.
2. migrate_cdm: Uses the Migrate API to load Tiff masters, metadata exported from CONTENTdm, and authority records exported from [TemaTres v.2](http://www.vocabularyserver.com/). (We are hoping migrating to TemaTres 3 will give us a better export that preserves the relationships ontology we used.)

*Note:* This proof-of-concept assumes that new content types will be created for each metadata profile/object type pair; ergo, this proof-of-concept includes a new content type called UNLV_image which parallel's islandora_image but adds some node entity references. However, the CLAW team is exploring alternative strategies for managing descriptive metadata which will make portions of this example out of date (hopefully soon).

# Source Data

The source data used for this proof of concept came from the UNLV Special Collections and Archives but are not in the public domain. As such, the metadata files are provided (in the data directory) but not the original Tiffs. This version is being committed for the sake of expediency although I will follow-up with another commit using public domain images ASAP.

# Running

Note: using drush with migrate_tools is optional, but the instructions assumes it is enabled

1. Copy the migrate_cdm and unlv_image directories to your modules directory
2. Copy the data directory to the directory containing your drupal root (drupal root and the data directory will be parallel.) (And add your own source tiffs to data/images using the names in the first column of source.csv.)
3. Enable the modules. E.g. `drush en -y migrate_cdm`
4. Migrate the authorities. E.g. `drush mim tematres_agents`
5. Migrate the images. E.g. `drush mim --all` (Migrate will automatically skip any authorities in tematres_agents we already migrated.)
