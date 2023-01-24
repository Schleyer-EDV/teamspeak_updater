# TeamSpeak 3 Server Updater
A lightweight script to install or update a TeamSpeak 3 server on Linux or FreeBSD. Should also work on macOS (currently untested).

## Dependencies
* **jq** 1.4 or newer (https://stedolan.github.io/jq/download/)
* curl
* shasum, sha256, or sha256sum
* tar
* bzip2

All other dependencies (basename, cd, command, cut, dirname, echo, grep, mktemp, printf, read, sed, test, touch, uname, wc, and an sh-compatible shell) are installed by default on most systems.

## How to use
### Install a new TeamSpeak 3 server
Place the script where you want to install the TS server, make it executable with `chmod +x ts3updater.sh` and run it with `./ts3updater.sh`. After the installation, the script moves itself into the installation folder. Run it from there whenever you want to update the server.

### Update an existing TeamSpeak 3 server installation
Place the script in the directory of your existing TS installation. This means, the script has to be in the same directory as for example the file `ts3server_startscript.sh`. Make it executable with `chmod +x ts3updater.sh` and run it with `./ts3updater.sh`.

If you use TSDNS, make sure the service is stopped before you execute this script.

### Update the server automatically
As the server is only updated if a new version is available, you can also use the script to automate updates. Simply create a cronjob which executes the script for instance hourly or daily.
A crontab entry to run ts3updater daily at 13:28 could look like this: `28 13 * * * /home/ts/teamspeak3-server_linux_amd64/ts3updater.sh`. Append ` >/dev/null 2>&1` if you don't want to get a mail each time the script is run.

You must accept the server license agreement before you create the cronjob. To do so, go to the directory of your TS installation, read the file `LICENSE` and create a file called `.ts3server_license_accepted`.

### Commandline parameters
All parameters are passed to `ts3server_startscript.sh` when the server is started. If you don't want to start the server after installing or updating, use the parameter `--dont-start`.

## What this script is doing
1. Determine the OS and CPU architecture
2. Check if there is an existing installation and determine its version
3. Check for newer versions
4. If there is a newer version:
    1. Download the new version
    2. Check whether the checksum is correct
    3. Stop the server
    4. Extract the updated files into the server directory
    5. Start the server
