# doppler-docker-akamai-publish
Docker image to help us to publish releases to Akamai CDN

Travis CI url: https://travis-ci.org/MakingSense/doppler-docker-akamai-publish

# How to use doppler-relay-akamai-publish docker image:

Docker HUB url: https://hub.docker.com/r/dopplerrelay/doppler-relay-akamai-publish/

This image is ready to upload to akamai CDN the resource files. Contains the cdn-uploader script and his dependencies ready to run.

IMPORTANT:
It is necessary to define AKAMAI_CDN_HOSTNAME, AKAMAI_CDN_USERNAME, AKAMAI_CDN_PASSWORD, AKAMAI_CDN_CPCODE, PROJECT_NAME, VERSION_NAME environment variables and a /source volume.

## First Step, run it: ##

```bash
docker run --rm \
	-e "AKAMAI_CDN_HOSTNAME=example.cdn.com" \
	-e "AKAMAI_CDN_USERNAME=example" \
	-e "AKAMAI_CDN_PASSWORD=12345" \
	-e "AKAMAI_CDN_CPCODE=56789" \
	-e "PROJECT_NAME=mseditor" \
	-e "VERSION_NAME=v1.0.0-build1234" \
	-v /`pwd`/build:/source dopplerrelay/doppler-relay-akamai-publish

	 # /`pwd`/ is a reference to your current working directory
	 # by default is /`pwd`/build, change it if you have your source files in a different folder: /`pwd`/[path_to_files] example: /`pwd`/my_folder/my_subfolder
```

**NOTE:**

This image will be updated in docker hub for each release in the master branch of this repository using continuous integration through travis yaml configuration.


# How to use cdn-uploader.js library:

This script upload via ftp resources from the docker container image to Akamai CDN.

Command:

```bash
node cdn-uploader.js arg1 arg2 arg3

#arg1: Is the source path from the docker image where will found the files and folders to upload.

#arg2: Is the base path to load different projects to the cdn, must be unique for each project.

#arg3: Is the destination build path where you found your resources.

```

Example:

```bash
drwxr-xr-x    2 root     root          4096 Jan  9 19:37 .
drwxr-xr-x    1 root     root          4096 Apr 13 13:25 ..
drwxr-xr-x    2 root     root          4096 Jan  9 19:37 cdn-uploader.js
drwxr-xr-x    1 root     root          4096 Apr 13 13:25 source0/example/
drwxr-xr-x    1 root     root          4096 Apr 13 13:25 source2/projects/
drwxr-xr-x    1 root     root          4096 Apr 13 13:25 source2/v1.0.0-build1234/
drwxr-xr-x    1 root     root          4096 Apr 13 13:25 source/
total 8


node cdn-uploader.js source0/example project1 exampleBuild # This will upload all files and folders inside of "example" folder recursively to "project1/exampleBuild" folder on CDN.

node cdn-uploader.js source2/projects project2 other # This will upload all files and folders inside of "projects" folder recursively to "project2/other" folder on CDN.

#Real examples:

node cdn-uploader.js source mseditor v1.0.0-build1234

#Will upload  v1.0.0-build1234 inside of source/ folder

http://cdn.fromdoppler.com/mseditor/v1.0.0-build1234/scripts/example.gif

```

Note: The shell context init in the same folder where cdn-uploader.js is running.
