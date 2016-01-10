---
title: SyncDebianPackage
permalink: /SyncDebianPackage/
---

Sync Debian Package
===================

The Tanglu archive is maintained by the Debian Archive Kit (dak). Tanglu developers can communicate with our dak instance through command-files. Currently, these command-files (at least the ones for syncing packages) have to be created by hand. We will later have a tool for that.

Create a sync-commands file
---------------------------

In order to sync a package, create a file with the following content:

``` text
Archive: janus
Uploader: Werner Heisenberg <example@tanglu.org>

Action: sync
Suite: unstable
Component: main
Packages: foo bar
```

Adjust the fields to your needs. If you want to sync stuff from multiple suites or components, create multiple files. Save the file as *developer-id_sync_unique.dak-commands*, for example for a Developer with the ID "mak", we would have *mak_sync_j739d.dak-commands*.

Sign the file
-------------

Sign the dak-commands file with your key. The key has to be registered in the master developers keyring of the Tanglu archive, maintainer permissions are not enough for syncing packages. Use `gpg --clearsign` for signing.

Upload
------

Upload the dak-commands file to the package incoming queue via FTP. You're done! You will receive an email when the commands are processed. Sometimes the archive caches don't yet have the package you want to sync, especially if it is very new in Debian. In this case, just be patient and upload the commands again at a later time.