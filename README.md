WTF NEXTCLOUD
=============

Version: 2.0  

## Why?

I'd say a good 90% of the time I upgrade Nextcloud, even while set to the "Stable" branch, I am presented with errors galore.  Without fail, when I update/upgrade from the web interface I am inevitably forced to crack open a terminal and perform some task or un-brick my instance.  I have used and administered Nextcloud for a long time, and have multiple instances that I take care of.  So when a update or upgrade comes along (I think the stable branch is at about 2-3 upgrades every couple weeks now... WTF! I have to keep checking to make sure I'm not set to beta...  honestly, I think the stable branch is the new beta branch... lame!).  Anyway, as I was saying, when a upgrade or update comes along, I procrastinate the changes because I know it's gunna take me a hot hour to un-screw some "improvement" made to the system.

Because I know I will have to drop to the command line anyway, I might as well just start there.  

This script contains the most common commands I found that tend to need ran.  I also found that if I just perform upgrades from the command line and finish it off with the "repair steps", I can save a boat load of time and headaches.  With my latest upgrade from 30 to 31, I had to roll tables to DYNAMIC row format.  I about had a come apart.  Well, this script now contains the fix for that too.

It's not a huge save the planet kind of thing.  But it does save me time, saves me from having to manually plug in all the various commands and having to remember them, and it does provide a little peace of mind.


## What Next?

Stupid freak'n Group Folders!  WTF!  
Hopefully changing the name to "Team Folders" made everything better.  


## Script Functions

The primary and default function of this script is to perform the repair steps (listed below).  If no flags are given the script will search for a nextcloud instance, and if found will perform the repair steps automatically.  If multiple instances are found, the script will present them and allow for a selection to be made.  When using the convert or upgrade options, the script will perform a database backup before doing anything else.  There are 3 variables at the top of the script that can be (should be) reviewed before running it.

```
NCPATH=""               # set the nextcloud path manually (will not search for all instances if defined)
WEBROOT="/var/www"      # directs the 'find' instances function (no need to scan the whole system)
DBBAK="/var/www/dbb"    # db backups location (will be created if it don't exist)
```


## CLI Options  

```
Usage: ./wtf_nc [-p|--path <nextcloud_path>] [-l|--list] [-c|--convert] [-u|--upgrade] [-n|--no-repair] [-v|--version] [-h|--help]

Options:
  -p, --path PATH     Path to Nextcloud installation
  -l, --list          List available Nextcloud installations
  -c, --convert       Backup and convert tables to DYNAMIC row format
  -u, --upgrade       Upgrade Nextcloud (will convert db first)
  -n, --no-repair     Skip final repair steps
  -v, --version       Show script version
  -h, --help          Show this help message
```

## Repair Steps

  __"EMPTY NEXTCLOUD LOG"__  
  `> data/nextcloud.log`  

  __"ENSURING OCC IS EXECUTABLE"__  
  `chmod +x ./occ`  

  __"FINISH UPGRADING NC IF NEEDED"__  
  `sudo -u nginx php ./occ upgrade`  

  __"UPDATE ALL APPS"__  
  `sudo -u nginx php ./occ app:update --all`  

  __"TURNING OFF MAINTENANCE MODE"__  
  `sudo -u nginx php ./occ maintenance:mode --off`  

  __"REPAIRING EXPENSIVE TABLES"__  
  `sudo -u nginx php ./occ maintenance:repair --include-expensive`  

  __"ADDING MISSING COLUMNS"__  
  `sudo -u nginx php ./occ db:add-missing-columns`  

  __"ADDING MISSING INDICES"__  
  `sudo -u nginx php ./occ db:add-missing-indices`  

  __"ADDING MISSING PRIMARY KEYS"__  
  `sudo -u nginx php ./occ db:add-missing-primary-keys`  

  
