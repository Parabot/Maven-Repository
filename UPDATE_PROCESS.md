Explanation
===================
This tutorial explains on how to deploy new updates to the version control of Parabot.

[TOC]

VC
------------
The Version Control of Parabot is based on a webhook within this repository.

When this repository gets updated, it calls the updater on the web API.
The web API will then receive all updates and see if there's a new jar update.
Once there is a new jar update, the API will than gather all information and download it to its own server.

When the download is finished, the database will be updated with a hash and update time.
This can be used to check hashes and the cache directory.
> **Note:**
> When Github caches the latest.json, the API can't gather the latest version.
> For this reason we build a cron job that checks for new versions every 30 minutes.
> In this way we are sure something like no-update never happens.

The process
------------
For the updating process, we have a different way of working. If you follow this tutorial you'll be ensured that your changes gets pushed.

> **There are basically three files that you need to edit:**

> - ```latest.json``` - This file is being used by the API to see if there's a new version to publish
> - ```pom.xml``` - This file is located in the project itself (client/provider) and contains the version that will be published
> - ```Configuration.java``` - This file is located in the client and contains a string that should match the current version.

If any of these do not match each other, ie the version in latest.json does not match the version in Configuration.java, then the client will detect a different version and asks the user to download the latest version. But because there the Configuration.java does not match the latest.json, it will keep repeating this process.

#### Update the client
Updating the client is the most complicated, but also the most important one.

**When updating the client, make sure the version in either ```latest.json```, ```pom.xml``` and ```Configuration.java``` match each other.**

To update the version do the following:
0. Clone the Maven-Repository into the same parent directory as the client/provider directory. 
1. Take a quick look at the versioning, make sure each version in all files of above match each other
2. Start cleaning your Maven directories with ```mvn clean```
3. Now go into the pom.xml and make sure the excludes are commented out and the includes are not commented out
4. Build the project using ```mvn package```
5. Now go into target/classes, in here you'll find the .bat files. Copy **the first line** of **at least** deploy.bat into another file.
6. Now go back into the pom.xml and comment out the includes and include the excludes
7. You are now allowed to package the project again, you can do this with ```mvn package```
8. Once this process is done, you should open your console
9. Now ```cd``` into the deploy directory of the project.
10. When you are in the deploy directory, you can perform the command copied from step #5 - *(deploy.bat)* - *You are allowed to perform the other commands, but then you should be in the parent directory of deploy.*
11. Once this is done, you can push the files from the Maven-Repository to the Git repository.

#### Update a/the provider
Updating a/the provider has almost the same process as updating the client, the only difference is that the versioning doesn't really matter. This is because the provider doesn't contain a check for the actual version, instead it simply cleans the cache when a new update came out.

**When updating the client, make sure the version in either ```latest.json``` and ```pom.xml``` match each other.**

0. Clone the Maven-Repository into the same parent directory as the client/provider directory. 
1. Take a quick look at the versioning, make sure each version in all files (except for ```Configuration.java```) of above match each other
2. Start cleaning your Maven directories with ```mvn clean```
3. Now go into the pom.xml and make sure the excludes are commented out and the includes are not commented out
4. Build the project using ```mvn package```
5. Now go into target/classes, in here you'll find the .bat files. Copy **the first line** of **at least** deploy.bat into another file.
6. Now go back into the pom.xml and comment out the includes and include the excludes
7. You are now allowed to package the project again, you can do this with ```mvn package```
8. Once this process is done, you should open your console
9. Now ```cd``` into the deploy directory of the project.
10. When you are in the deploy directory, you can perform the command copied from step #5 - *(deploy.bat)* - *You are allowed to perform the other commands, but then you should be in the parent directory of deploy.*
11. Once this is done, you can push the files from the Maven-Repository to the Git repository.

You can check the latest update for the client [here](http://bdn.parabot.org/api/v2/bot/information/client) and for the provider [here](http://bdn.parabot.org/api/v2/bot/information/317-api-minified), if this matches a timestamp within the last 30 minutes of your push then you're fine.

Questions
------------
If you have any questions regarding this process. Please contact an administrator, whereas preferably this would be [JKetelaar](https://www.parabot.org/community/profile/1-jketelaar/).
