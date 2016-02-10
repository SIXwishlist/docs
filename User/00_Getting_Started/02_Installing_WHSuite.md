Before installing WHSuite, make sure your hosting environment meets the [System Requirements](./System_Requirements). Once you've done this, you can jump in and start installing.

You'll first want to create your directory structure. We recommend installing WHSuite on it's own domain or sub-domain where possible.

##Uploading your files
The first step is to upload the provided WHSuite files. These are files located in the 'upload' directory, and should be uploaded to the publicly accessible web directory on your server. Depending on your web server setup this is usuall called something like public_html, http_docs, htdocs or www. 

##File Permissions
Once your files have been uploaded, you'll need to set some permissions. WHSuite has a storage directory located at /whsuite/app/storage.

Your WHSuite installation needs to be able to read and write files in this directory. Genrally setting the permissions of the directory recursively to 0755 will be sufficient. During the web install, WHSuite will check permissions and let you know if they need changing.

##Web Installer
You should now be ready to run the web installer. Browse to the URL where you've uploaded the files and the WHSuite installer should start automatically. At this point a system check will be run to see if you've got the correct PHP extensions and file permissions. To continue with the installation, follow the on screen instructions.


##Troubleshooting

If you're having issues installing WHSuite please first read through the [system requirements](./System_Requirements) section as it's likely that you're missing a required PHP module or your permissions are incorrectly set. 
