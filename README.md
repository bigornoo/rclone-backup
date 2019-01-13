# rclone-backup

## About
Just a fork of the [rclone](https://rclone.org/) Windows batch scripts to backup / sync some parts of my QNAP NAS (Linux) to [pCloud](https://www.pcloud.com/) (**NOT** encrypted).

It's the same schema except for the paths.
![](.github/rclone-backup.png)

## Usage
### 1. Make local source and remote destination identical :
Example :
```
QNAP /                                         pCloud: /QNAP/dns_name :
├── Homes                                      ├── Homes
├── Multimedia                                 ├── Multimedia
├────────────── Music          sync            ├────────────── Music
├────────────── Video                          ├────────────── Music
├────────────── Photo                          ├────────────── Music
├── Public/Logiciels           --->            ├── Public/Logiciels
├── Web/famille                                ├── Web/famille
(local source)                              (remote destination)
````
- Edit [configuration variables](https://rclone.org/commands/rclone_sync/) in `rclone-sync.sh`:
```bash
# Configuration variables
RCLONE_EXE_PATH="%~dp0rclone.exe"
RCLONE_CONFIG_PATH="%~dp0rclone.conf"
RCLONE_LOG_FILE_PATH="%~dp0rclone-sync-log.txt"
# Log level : DEBUG|INFO|NOTICE|ERROR (default NOTICE)
RCLONE_LOG_LEVEL=INFO
# Read exclude patterns from file
RCLONE_FILTER_FILE_PATH="%~dp0rclone-filters.txt"
# Number of checkers to run in parallel (default 8)
RCLONE_CHECKERS_NUMBER=8
# Number of file transfers to run in parallel (default 4)
RCLONE_FILE_TRANSFERS_NUMBER=4
# Number of low level retries to do (default 10)
RCLONE_LOW_LEVEL_RETRIES_NUMBER=10
# Retry operations this many times if they fail (default 3)
RCLONE_RETRIES_NUMBER=10
# Interval between retrying operations if they fail, e.g 500ms, 60s, 5m (0 to disable)
RCLONE_RETRIES_SLEEP=5s
# Paths
RCLONE_LOCAL_PATH=D:\
RCLONE_REMOTE_PATH=pCloudEncrypted:
# Local directory names to sync, comma separated
RCLONE_DIRECTORIES_TO_SYNC=Backups,Documents,Ebooks,Games,Movies,Music,Pictures,Softwares
# Additional rclone flags
RCLONE_ADDITIONAL_FLAGS=--delete-excluded --progress --stats-one-line
```
- Edit your [filter file](https://rclone.org/filtering/) : `rclone-filters.txt`
- Start `rclone-sync.sh` with your rclone config file (`rclone.conf`) password (as argument or not):  
`rclone-sync.sh your-Rclone-Config-Password`  
:bulb: Note that you can use the `shutdown` or `hibernate` argument to shutdown / hibernate after sync:  
`rclone-sync.sh your-Rclone-Config-Password shutdown`
- Wait for rclone sync to complete

### 2. Mount remote as a mountpoint:
It's not possible for the QNAP (QTS 4.3) : fuse library is too old (2.6). Unless you use a docker container.  

### 3. Check the integrity of a remote (not crypted):
Example:
```
QNAP /                                         pCloud: /QNAP/dns_name :
├── Homes                                      ├── Homes
├── Multimedia                                 ├── Multimedia
├────────────── Music          MD5             ├────────────── Music
├────────────── Video         check            ├────────────── Video
├────────────── Photo         <--->            ├────────────── Photo
├── Public/Logiciels                           ├── Public/Logiciels
├── Web/famille                                ├── Web/famille
(local source)                                 (remote destination)
````
- Edit [configuration variables](https://rclone.org/commands/rclone_cryptcheck/) in `rclone-check.sh`:
```bash
# Configuration variables
RCLONE_EXE_PATH="%~dp0rclone.exe"
RCLONE_CONFIG_PATH="%~dp0rclone.conf"
RCLONE_LOG_FILE_PATH="%~dp0rclone-check-log.txt"
# Log level : DEBUG|INFO|NOTICE|ERROR (default NOTICE)
RCLONE_LOG_LEVEL=NOTICE
# Read exclude patterns from file
RCLONE_FILTER_FILE_PATH="%~dp0rclone-filters.txt"
# Number of checkers to run in parallel (default 8)
RCLONE_CHECKERS_NUMBER=8
# Number of file transfers to run in parallel (default 4)
RCLONE_FILE_TRANSFERS_NUMBER=4
# Number of low level retries to do (default 10)
RCLONE_LOW_LEVEL_RETRIES_NUMBER=10
# Retry operations this many times if they fail (default 3)
RCLONE_RETRIES_NUMBER=10
# Interval between retrying operations if they fail, e.g 500ms, 60s, 5m (0 to disable)
RCLONE_RETRIES_SLEEP=5s
# Paths
RCLONE_LOCAL_PATH=D:\
RCLONE_REMOTE_PATH=pCloudEncrypted:
# Local directory names to sync, comma separated
RCLONE_DIRECTORIES_TO_SYNC=Backups,Documents,Ebooks,Games,Movies,Music,Pictures,Softwares
# Additional rclone flags
RCLONE_ADDITIONAL_FLAGS=--delete-excluded
```
- Edit your [filter file](https://rclone.org/filtering/) : `rclone-filters.txt`
- Start `rclone-check.sh` with your rclone config file (`rclone.conf`) password (as argument or not):  
`rclone-check.sh your-Rclone-Config-Password`  
:bulb: Note that you can use the `shutdown` or `hibernate` argument to shutdown / hibernate after check:  
`rclone-check.sh your-Rclone-Config-Password shutdown`
- Wait for rclone cryptcheck to complete
- Search for errors in `rclone-check-log.txt` log file

### 4. Get quota information from the remote:
Example:
```
Total:   500G
Used:    260G
Free:    240G
```
- Edit [configuration variables](https://rclone.org/commands/rclone_about/) in `rclone-about.sh`:
```bash
# Configuration variables
RCLONE_EXE_PATH="%~dp0rclone.exe"
RCLONE_CONFIG_PATH="%~dp0rclone.conf"
RCLONE_REMOTE_PATH=pCloudEncrypted:
```
- Start `rclone-about.sh` with your rclone config file (`rclone.conf`) password (as argument or not):  
`rclone-about.sh your-Rclone-Config-Password`
- Remote quota information is displayed

## Requirements
- [rclone](https://rclone.org/)
- shell

## Todo
- Add more usefull rclone flags for sync / check / mount commands
  
## License
rclone-backup is released under the [Unlicense](http://unlicense.org).
