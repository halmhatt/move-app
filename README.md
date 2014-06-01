move-app
========

An app that moves files to remotes. It syncs but then removes the files localy

*This does not work yet! Under development*

## Usage
Start server

```bash
move-app ~/move-app/
```

This starts the server to listen for changes in the directory `~/move-app/`. If you put files in there these will be synced to each *remote* listed in `~/move-app/.move-app.json`. When all remotes are updated the files will **be removed locally**. 

The *file structure will be respected on each remote*. This means that the path `~/move-app/pictures/new-york/` will be synced to each remote on `<remote move app dir>/pictures/new-york/`.

## Configure
Put a file called `.movie-app.json` inside your *move-app dir*. This file will not be removed when files has been moved

Example: 
```json
{
	"name": "My pictures",
	"remotes": [
		{
			"base_dir": "/home/user/move-app/pictures",
			"user": "username",
			"uri": "example.com"
		}
	],
	"ignore": [
		".move-app.json"
	]
}
```

### name
The name property will be used in CLI interface and logs
Type: `String`
Default: folder name

### remotes
The remotes here will *all be synced* before files are removed on local machine. This means that if you list 2 remotes, both of them will have the resulting files in the end, but not your local machine.

The remotes will be synced with `rsync` using something like `username@uri:base_dir/<local relative file structure>`

Type: `Array`
Default: `[]`

#### base_dir
The directory on the remote that files will be synced to.
Type: `String`

#### user
The username that will be used to login with `ssh`
Type: `String`

#### uri
The *uri* that will be used for `ssh`
Type: `String`

### ignore
Files to ignore, these will not be synced. Please feel free to ignore the `.move-app.json`. 


## CLI
If you use the flag `--help` you will get help about all the commands and flags available.

```bash
Usage: move-app [flags][subdir/|start]
	--help	Show this help message

	start 	Start move-app as a daemon
	ls 		List remote files in this directory
	test	Run a test checks that all steps are working correctly
	init	Initialize a .move-app.json config file
```