# radarr-sqlite-csv-to-postgres
This repository contains a script that makes migrating from radarr using sqlite to postgres a little easier.

I'm not very good at explaining things so let me try to do my best.

This script is if pgloader doesn't work for you like in the guide mentioned below on wiki.servarr.com

Backup your radarr database by going here: IP_OF_RADARR_INSTANCE:PORT/system/backup and click Backup Now.
Click the latest backup to download it to your computer.
You're going to need the radarr.db file later.
So follow this guide like normal: [https://wiki.servarr.com/radarr/postgres-setup](https://wiki.servarr.com/radarr/postgres-setup).
When you get to the migrating data step, come here and follow the rest of this guide.
Download this program that allows you to dump an sqlite database/tables to csv files: [https://sqlitebrowser.org/dl/](https://sqlitebrowser.org/dl/)
Open up radarr.db in sqlite browser. Go to File -> Export -> Table(s) as csv files.
Select all tables except for sqlite_sequence. 
Make sure Column Names in First Line is checked and click OK.
Once it finishes dumping all of the tables to csv.

Okay here is where it is trial and error until it completes without issue. You may have to modify your files a little to get it to not complain:
Go into your database via pgadmin or some other tool and run this query:

<code>delete from "AlternativeTitles";
delete from "AutoTagging";
delete from "Blocklist";
delete from "Collections";
delete from "Commands";
delete from "Config";
delete from "Credits";
delete from "CustomFilters";
delete from "CustomFormats";
delete from "DelayProfiles";
delete from "DownloadClients";
delete from "DownloadClientStatus";
delete from "DownloadHistory";
delete from "ExtraFiles";
delete from "History";
delete from "ImportExclusions";
delete from "ImportListMovies";
delete from "ImportLists";
delete from "ImportListStatus";
delete from "Indexers";
delete from "IndexerStatus";
delete from "Metadata";
delete from "MetadataFiles";
delete from "MovieFiles";
delete from "MovieMetadata";
delete from "Movies";
delete from "MovieTranslations";
delete from "NamingConfig";
delete from "Notifications";
delete from "NotificationStatus";
delete from "PendingReleases";
delete from "QualityDefinitions";
delete from "QualityProfiles";
delete from "ReleaseProfiles";
delete from "RemotePathMappings";
delete from "RootFolders";
delete from "ScheduledTasks";
delete from "SubtitleFiles";
delete from "Tags";
delete from "Users";
delete from "VersionInfo";</code>

Make sure the username, password, and host is configured on this line:
<code>engine = create_engine('postgresql://username:password@127.0.0.1:5432/radarr-main')</code>
Run the python module using idle or terminal if it doesn't make it to the end, then run all of the delete statements again, fix the error in one of the csv files and try running the python module once more.
I have this working with postgres 17. 
If you have an issue, please create an issue and I will try to fix the problem. I'm slow to fix things in my hobby projects so please be aware of this.
