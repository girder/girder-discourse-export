# Information about database encryption

## Edern_Haumont on 2019-03-07T10:33:52.493Z

Good morning,


We have a project where we store sensitive information in MongoDB that we access through girder.  

We should encrypt most if not all information stored in the database.


I read that we could use a built in feature of mongodb entreprise (not free though) to encrypt data “at rest”. How would that work with girder ?


We are pretty much all novices in database security in my team so it is unclear for us what our options are. Are there other ways to achieve encryption of stored data ? It is possible for us to repopulate entirely the database if needed, all population, accesses, etc. are done through girder plugins.


Thanks a lot.


---

## Edern_Haumont on 2019-03-07T13:51:23.663Z

Related question: what about the encryption of the filesystem assetstore ?


---

## Jonathan_Beezley on 2019-03-07T14:02:44.971Z

Girder itself doesn’t have any built\-in encryption, but I don’t see any reason why you couldn’t configure mongodb for “at rest” encryption. From the docs, the application layer doesn’t need any additional changes to support that.


For an encrypted assetstore, I a custom plugin might work to encrypt/decrypt data as it is read. An easier solution would be to create the asset store in an encrypted partition on the server if you can get away with leaving the encrypted partition mounted while Girder is running.


---

