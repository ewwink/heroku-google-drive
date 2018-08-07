

# Heroku Google Drive
Remote [Google Drive client](https://github.com/ewwink/heroku-google-drive) on Heroku using Rclone and Aria2

## Installation
Create new app

```
heroku create myapp -b https://github.com/ewwink/heroku-google-drive.git
heroku git:clone -a myapp
```

Existing app, use: `add|set`

```
heroku buildpacks:set https://github.com/ewwink/heroku-google-drive.git -a myapp
```

go to `myapp` directory, create or copy `rclone.conf` and winrar registraton key `.rarreg.key` (optional) then commit the change

```
cd myapp
git add .
git commit -am "add config"
git push heroku master
```
if you don't have `rclone.conf` download `rclone` and run locally `rclone config` generated config file will be

```
Windows: %userprofile%\.config\rclone\rclone.conf
Linux: $HOME/.config/rclone/rclone.conf
```
## Usage
**Open remote Heroku**
```
cd myapp
heroku run bash
# or
heroku run bash --remote origin
```

**Upload to Google Drive**

assume `gdrive_config` is your Google drive config name that generated above 
```
rclone -v copy local_dir gdrive_config:remote_drive_dir
```

**Speed up upload**

If you want to upload many files smaller than 8mb increase only `--transfers` option

```
rclone -v --transfers=16 --drive-chunk-size=16384k --drive-upload-cutoff=16384k copy local_dir gdrive_config:remote_drive_dir
 ```
`--transfers=N`  number parallel of connection. `default: 4`

` --drive-chunk-size=N` if file bigger than this size it will splits into multiple upload, increase if you want better speed. `default: 8192k or 8mb`

`--drive-upload-cutoff=N` should be same with chunk size

`-v` option to view upload progress stats 

**view file on Google drive**
```
rclone lsd gdrive_config:remote_drive_dir
```
view option:

`lsd` only show file in current directory

`ls` show file including in subdirectory (recursvely)

## Bonus
**Download file using `Aria2`**

Aria2 is command-line download accelerator
```
aria2c -x4 http://host/file.rar
```
`-x4` mean download using 4 connection

**To extract `.rar` file**

to current directory
```
unrar e file.rar
```

with full path

```
unrar x file.rar
```
