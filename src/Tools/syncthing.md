I use syncthing to sync files between devices.

**Note**: On mobile, make sure syncthing is actually running. I was puzzled to find why syncthing wasn't running on my phone after I had installed it. Since battery saving mode was ON, the service hadn't actually started running. Just keep this in mind, if syncthing doesn't actually connect or sync properly.

Syncthing supports encryption for folders shared with devices. The data in the folder will be encrypted using a password and also can be decrypted using the same password.

For more info, refer to the [syncthing docs](https://docs.syncthing.net/users/untrusted.html)

For decrypting the folder, use the command:

`syncthing decrypt --to <plaintext-destination> --password <your-password> <encrypted-folder-path>`