---
title: Minecraft Python Installer
date: 2023-01-11 20:58:00 -0500
categories: [Programming, Lesson, Other]
tags: [Minecraft, scripting, Linux, python]
---

So just fooling around and wanted to make an installation script to install a Minecraft dedicated server on Linux. I've done a little research so going to try to put together some sort of an application.

## Tools

| Tools | Description |
|-------|-------------|
| [Pyarmor](https://wiki.python.org/moin/Pyarmor) | Pyarmor is a command line tool used to obfuscate python scripts, bind obfuscated scripts to fixed machine or expire obfuscated scripts. |
| [Makeself](https://makeself.io/) | Allows creating a self-extracting distribution package in Unix systems. |
| [PyInstaller](https://pyinstaller.readthedocs.io/en/stable/usage.html) | It Bundles a Python application and all its dependencies into a single package. |

## Application Structure

Going to start off with the application structure.

```bash
minecraft_installer
├── build.sh
├── dist
│   └── server.jar
└── makeself_stage
    └── installer.sh
```

The main folder is going to contain the nessecary files to build the application, and best practice is to have a script for the package creation seperated in a `build.sh` file.

This file will have multiple steps. We will start by declaring the pyarmor path.

```bash
PYARMOR="$HOME/.local/bin/pyarmor"
```
{: file="minecraft/build.sh" }

We are going to use this file to build our dependent modules, like copying binaries and libraries to a folder.

Now we will generate distribution packages with pyarmor. This option uses pyinstaller for creating the package bundle. Also, you can use the option “-e” to pass extra options to pyinstaller

```bash
#!/bin/sh
PYARMOR="$HOME/.local/bin/pyarmor"
${PYARMOR} pack --clean -e '--paths='path_to_search_imports:.' --hidden-import='PIL._tkinter_finder' --add-binary='path_to_lib.so:.' --exclude-module=modules_not_needed --add-data='./Config:Config' --add-data='./*.png:.'' install.py
```
{: file="minecraft/build.sh" }

Pyarmor will create the default bundled package in `/dist`. We can change this by using the `-O` or output option.

Setup makeself so we can create self-extracting archives from the package folder created by pyarmor.

```bash
sudo nala install makeself
```

## Payload

So for the payload we need to create an archive of the preconfigured minecraft server files. So download the minecraft server files first.

```bash
wget https://piston-data.mojang.com/v1/objects/c9df48efed58511cdd0213c56b9013a7b5c9ac1f/server.jar
```
{: file="dist/" }

Then we need to extract the jar files then delete the server.jar

```bash
java -Xmx1024M -Xms1024M -jar server.jar nogui
```
{: file="dist/}

This will allow us to modify and automate the process of setting up the server. After we have configured everything we can tar for delivery.

## Create Package for delivery
tar -cvf serverfiles.tar *

## User Configuration 

It's time to create the installation script that will install the `dist/server.jar` file. We can pretty much modify this script to do anything for us. 

We setup an agreement for the installation and also a configuration for the port to use for the installation.

```bash
install_main ()
{
	echo "Configuring server.properties"
	#Minecraft server properties
	#Thu Jan 12 03:46:57 PST 2023
	echo "enable-jmx-monitoring=false" > dist/server.properties
	echo "enable-jmx-monitoring=false"
	echo "############################################################"
	echo "#		CHOOSE PORT 										 "
	echo "############################################################"
	
	process=$((0))
	port_check ()
	{
		echo "Enter port for the server between 20000-30000 but not default: "
		read port_choice
		
		if [[ $port_choice != '^[0-9]+$' ]] && [[ $port_choice -ge 20000 && $port_choice -le 30000 ]];	then 
			echo "Checking $port_choice."
		else
			echo "Is not a valid number, or choice is empty."
				port_check
		fi
		if netstat -tuln | grep -q ":$port_choice "; then
			echo "Port $port_choice is in use."
			port_check
		else
			echo "Port $port_choice is free."
			echo "Port check completed successfully."
			process=$(($process + 1))
		fi
		sleep 0.2
	}
	# Check port is good 
	if [[ $process = 0 ]]; then
		port_check
	fi
	echo "rcon.port=$port_choice" >> dist/server.properties
	echo "rcon.port=$port_choice"
	echo "level-seed=" >> dist/server.properties
	echo "level-seed="		
	echo "gamemode=survival" >> dist/server.properties
	echo "gamemode=survival"
	echo "enable-command-block=false" >> dist/server.properties
	echo "enable-command-block=false"
	echo "enable-query=false" >> dist/server.properties
	echo "enable-query=false"
	echo "generator-settings={}" >> dist/server.properties
	echo "generator-settings={}"
	echo "enforce-secure-profile=true" >> dist/server.properties
	echo "enforce-secure-profile=true"
	echo "level-name=world" >> dist/server.properties
	echo "level-name=world"
	echo "motd=A Minecraft Server" >> dist/server.properties
	echo "motd=A Minecraft Server"
	echo "query.port=$port_choice" >> dist/server.properties
	echo "query.port=$port_choice"
	echo "pvp=true" >> dist/server.properties
	echo "generate-structures=true" >> dist/server.properties
	echo "generate-structures=true"
	echo "max-chained-neighbor-updates=1000000" >> dist/server.properties
	echo "max-chained-neighbor-updates=1000000"
	echo "difficulty=easy" >> dist/server.properties
	echo "difficulty=easy"
	echo "network-compression-threshold=256" >> dist/server.properties
	echo "network-compression-threshold=256"
	echo "max-tick-time=60000" >> dist/server.properties
	echo "max-tick-time=60000" 
	echo "require-resource-pack=false" >> dist/server.properties
	echo "require-resource-pack=false" 
	echo "use-native-transport=true" >> dist/server.properties
	echo "use-native-transport=true" 
	echo "max-players=20" >> dist/server.properties
	echo "max-players=20" 
	echo "online-mode=true" >> dist/server.properties
	echo "online-mode=true"
	echo "enable-status=true" >> dist/server.properties
	echo "enable-status=true"
	echo "allow-flight=false" >> dist/server.properties
	echo "allow-flight=false"
	echo "initial-disabled-packs=" >> dist/server.properties
	echo "initial-disabled-packs="
	echo "broadcast-rcon-to-ops=true" >> dist/server.properties
	echo "broadcast-rcon-to-ops=true"
	echo "view-distance=10" >> dist/server.properties
	echo "view-distance=10"
	echo "server-ip=" >> dist/server.properties
	echo "server-ip="
	echo "resource-pack-prompt=" >> dist/server.properties
	echo "resource-pack-prompt="
	echo "allow-nether=true" >> dist/server.properties
	echo "allow-nether=true" 
	echo "server-port=$port_choice" >> dist/server.properties
	echo "server-port=$port_choice"
	echo "enable-rcon=false" >> dist/server.properties
	echo "enable-rcon=false" 
	echo "sync-chunk-writes=true" >> dist/server.properties
	echo "sync-chunk-writes=true"
	echo "op-permission-level=4" >> dist/server.properties
	echo "op-permission-level=4" 
	echo "prevent-proxy-connections=false" >> dist/server.properties
	echo "prevent-proxy-connections=false"
	echo "hide-online-players=false" >> dist/server.properties
	echo "hide-online-players=false" 
	echo "resource-pack=" >> dist/server.properties
	echo "resource-pack=" 
	echo "entity-broadcast-range-percentage=100" >> dist/server.properties
	echo "entity-broadcast-range-percentage=100"
	echo "simulation-distance=10" >> dist/server.properties
	echo "simulation-distance=10" 
	echo "rcon.password=" >> dist/server.properties
	echo "rcon.password=" 
	echo "player-idle-timeout=0" >> dist/server.properties
	echo "player-idle-timeout=0"
	echo "force-gamemode=false" >> dist/server.properties
	echo "force-gamemode=false" 
	echo "rate-limit=0" >> dist/server.properties
	echo "rate-limit=0" 
	echo "hardcore=false" >> dist/server.properties
	echo "hardcore=false"
	echo "white-list=false" >> dist/server.properties
	echo "white-list=false"
	echo "broadcast-console-to-ops=true" >> dist/server.properties
	echo "broadcast-console-to-ops=true"
	echo "spawn-npcs=true" >> dist/server.properties
	echo "spawn-npcs=true" 
	echo "spawn-animals=true" >> dist/server.properties
	echo "spawn-animals=true" 
	echo "function-permission-level=2" >> dist/server.properties
	echo "function-permission-level=2"
	echo "initial-enabled-packs=vanilla" >> dist/server.properties
	echo "initial-enabled-packs=vanilla"
	echo "level-type=minecraft\:normal" >> dist/server.properties
	echo "level-type=minecraft\:normal"
	echo "text-filtering-config=" >> dist/server.properties
	echo "text-filtering-config=" 
	echo "spawn-monsters=true" >> dist/server.properties
	echo "spawn-monsters=true" 
	echo "enforce-whitelist=false" >> dist/server.properties
	echo "enforce-whitelist=false"
	echo "spawn-protection=16" >> dist/server.properties
	echo "spawn-protection=16" 
	echo "resource-pack-sha1=" >> dist/server.properties
	echo "resource-pack-sha1="
	echo "max-world-size=29999984" >> dist/server.properties
	echo "max-world-size=29999984"

	# server.properties setup completed tar and extract to designated folder
	echo "server.properties setup completed."
}

build ()
{
	install_agree
	install_main
}

build
```
{: file="./build.sh" }

## Folder Check

I was going to make this installation process more abstract, but I am going to try to make a locally hoster webserver and document that where the variables will be completely customizable. 

But for now we need to create a process for the program to check for and create a valid directory. Then extract to the directory.





## Tar and extract

With our package and installation ready we can use makeself to package.

```bash
makeself ./minecraft_installer/makeself_stage/
```
{: file=/minecraft_installer}


