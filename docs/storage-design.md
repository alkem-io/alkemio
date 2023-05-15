# Storage and Document Management in Alkemio

This document is created to capture the design requirements and decisions made for Document Management in Alkemio.


## Requirements
The following are high priority desired functionalities related to document management on Alkemio:
* **Secure Access**: Allow uploading and having links to documents that are protected by Alkemio's authorization framework
* **Management**: Allow admins to see documents that have been uploaded in their Space, as well as meta information about thos documents
* **Secure**: Be able to track who uploaded a file and when, plus potentially additional meta information (e.g. upload IP)
* **Uploading rights per context**: Allow users to upload documents where they have relevant authorization privileges
* **Accessible via a URL**: the files need to be requestable via a http request whereby the response is the actual document
* **Quotas**: there should be limits on the amount of storage space used within a given context. This can then later be controlled by license rights. 
* **Single protocol**: it should be possible to disable the external ipfs protocol usage

The following non-functional requirements apply:
* **Performant**: The files access should be as fast as feasible given we are serving the content from our own servers. 
  
The following security requirements should be designed for:
* **Encryption at rest**: allowing the files to be encrypted at Rest

### Out of Scope
An important consideration is that File **contents** are *not modified* after they are uploaded. This means that we do not consider in the work 
stream the need to be able to collaborate with other users on Files via Alkemio. Meta information about files, such as the title / description / tags can be modified. 


## Design
The core of the envisaged solution is a new concept called a `Storage Bucket` which contains `Documents`.

### Storage Bucket

A Storage Bucket has the following properties:
* It is authorizable, so that there can be a policy specified for who is allowed to upload Documents to it, who can manage the contents in it etc.
* It contains a set of Documents (more detail shortly)
* It specifies the mime types supported for storing in the Storage Bucket. 
* It specifies the maximum file size that can be uploaded
* It can optionally have a "parent" Storage Bucket, whose resources are then used by the child Storage Bucket

In addition there are then derived properties such as:
* Total Bucket taken up by the content in the StorageBucket
* Amount of Documents
* ...

The Storage Bucket is also then the place where there can be both quotas on the total amount of storage being used, as well as allowing control to Space admins 
for a given Storage Bucket to adjust:
* What file types can be stored 
* The maximum file size can be uploaded

Note also each "community scope" will essentially need its own Storage Bucket. In particular, a Challenge will need to have its own StorageBucket as the Documents 
contained in it will be subject to different access rules than Documents in the containing Hub. 


### Document
The second core concept is that of a Document. 

A Document has the following properties:
* ExternalID: the identifier for the Document in the actual storage system being used
* CreatedBy: The user that uploaded the Document
* CreatedOn: When it was uploaded
* DisplayName
* Description
* Tags (to allow filtering)
* Size
* Mime type (required so that requests know how to handle it)
* ...

Other properties that can be added to expand the capabilities of storage / security around storage include:
* Encrypted at rest or not
* Uploading source IP
* ...

Important to note is that the platform in essence adds a bunch of meta data around the underlying storage of the file. 

### Accessing the Files
The files will be accessed via Rest requests.

The Rest api for documents will then check the request matches the 

### Usage
The following entities in the Alkemio domain model are expected to have a Storage Bucket:
* Hub
* Challenge: essentially a child of the parent Space Storage Bucket.
* Platform
* Library
* Organization

In the first version of Storage Bucket usage there are no quotas on files that are uploaded, but the 
Note that Platform and Library are essentially non-managed storage Buckets; we can look later if we need 
to put restrictions around content that is stored there.  

**Note**: as part of this we would allow all Registered users to be able to upload files on their Profile, which then 
uses the Platform StorageBucket. This is somewhat of a risk (and the reason we did not support this initially) in terms of providing unlimited resource accessibility, but a key 
difference is that we now have an audit trail of who uploaded each file - so we can also easily aggregate to see how much content each user is uploading if so desired.

### Knowing the StorageBucket to use ==> Profile
The Storage Bucket to use is tied to each Profile entity. This is chosen as that is where visuals are uploaded, and also references are essentially how we currently work with attachments.

This does mean that usage of image uploading or adding files are references depends on Profile entity - which means we should only use Visual entities as part of a Profile. This has implications for e.g. InnovationBucket usage with its Branding. It maybe this needs to use a Profile internally and expose the visual logically via Branding entity on the api.

## Known issues
* Deletion of files: a side effect of content addressed storage is that if two users upload the same document then it will get the same externalID. 
  * This raises the potential issue of two users uploading the same Document, and one deletes it. If we immediately deleted the file on IPFS then the second user's document contents would no longer be accessible
  * For now we do *not* delete the file directly on IPFS if a Document is deleted, but would need to add in a check if another Document has the same externalID or not before removing it. Once we have ensured there is no direct access to 
  the file via IPFS (directly) then we can consider also removig the file on IPFS after a check. 
  * Note: if the file is encrypted at rest in IPFS then this will also impact this, meaning the risk would not go across StorageBuckets - but the real solution is to move to another storage mechanism than IPFS.
  
### Encryption at rest (later)
For files that are to be protected via the api, the contents of the files should:
* be encrypted at rest
* decrypted as part of returning the response to the http request

Key management for this encryption is a topic to be taken carefully.

In the first version the suggestion is to not take on encryption at rest, but to rely on potential access to need the actual IPFS identifier for the file. 

So the Document itself stores the identifier in IPFS, but that would never be exposed via the api or otherwise. Then the access to the document goes through the Rest api which retrieves the document from IPFS and serves it out. A follow on step would then be to have the document encrypted / decrypted by the containing Storage Bucket. 

### Storage of Documents
In the first version of Storage Buckets it is expected that the content will simply be stored on IPFS.

This has the following benefits:
* Allows the implementation to re-use existing proven uploading
* Allows to immediately benefit from having the newly uploaded files backed up (via existing ipfs backup mechanism)
* Allows the team to focus on understanding the full implications of the new setup.

It is expected that it is a fairly incremental task to move to storing documents that are uploaded in another storage location - but that is not critical for the first version.


## Client
Client will need to:
* be able to choose the right Storage Bucket to upload to.
* be able to check that the user has the privilege to upload before attempting to do so

Otherwise the existing functionality should be not that much impacted.

Plus to provide a management interface for working with the files in a given Storage Bucket.

Note that we can consider having a "virtual" folder structure displayed based on tags that are added to the Documents. 


### Future Work
The following future work streams have been identified: 
* Moving to another storage mechanism that IPFS
* Migrate Whiteboard storage to be via Documents
    * This would drastically shrink the production database
* Encrypting the documents at rest
    * Key management is clearly a topic that requires due attention; in a first instance the suggestion is to have an encryption / decryption key per Storage Bucket. 
* Putting limits on each Storage Bucket that the platform enforces
    * This is obviously also a key subscription adjustment point
