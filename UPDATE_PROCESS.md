# Explanation
This tutorial explains on how to deploy new updates to the version control of Parabot.

### VC
The Version Control of Parabot is based on a webhook within this repository.

When this repository gets updated, it calls the updater on the web API.

The web API will then receive all updates and see if there's a new jar update.

Once there is a new jar update, the API will than gather all information and download it to its own server.

When the download is finished, the database will be updated with a hash and update time.
This can be used to check hashes and the cache directory.
