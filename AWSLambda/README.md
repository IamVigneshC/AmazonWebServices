## Use Lambda Functions within a Stateless Architecture on AWS

Your company is using Amazon S3 for file storage (using an S3 bucket) where all users can put and delete their files. When a user uploads or deletes a file, your company requires an entry to be immediately created in the database table. This entry contains the file metadata, like the file’s name, creation time, size of the file, Bucket, Event, and so on.

