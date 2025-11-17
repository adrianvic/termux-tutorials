Minecraft is so popular that it feels like after installing Termux, the temptation to get a Minecraft server up and running is natural. Whether you're going to host your server on a phone, or you are just inaugurating your phone server and want to see it in action, this guide should cover your use case. Although older versions can be lightweight, be aware that newer versions of Minecraft server demand a beefy amount of computation and RAM.

## Requirements
Minecraft Java Edition is really portable, you'll just need:

- Minecraft server JAR file of the version you want.
- Java installed on the server.

Also for newer versions you'll need a good amount of RAM and CPU power.

## Choosing a version/loader
The vanilla Minecraft server lacks optimizations, which are usually present in plugin/modloaders, you should usually avoid using that.
|  Name  |  Type  |  Performance  |  Notes  |
|----|----|----|----|
|  Vanilla  |  No mods/plugins  |  Lacks optimizations  |    |
|  Purpur  |  Plugins  |  Optimizations over PaperMC  |  Compatible with PaperMC but has more features  |
|  PaperMC  |  Plugins  |  Standard optimizations  |  Compatible with Bukkit  |
|  Bukkit/CraftBukkit  |  Plugins  |  Better than vanilla, worse than other plugin loaders  |  Foundation of Minecraft plugins  |
|  Project Poseidon  |  Plugins  |  Better than CraftBukkit  |  A fork of CraftBukkit for beta 1.7.3, adds a lot of features. Works with existing CraftBukkit plugins. |
|  UberBukkit  |  Plugins  |  Probably the same as Poseidon  |  A fork of Project Poseidon, adds backward compatibility  |
|  Fabric  |  Mods  |  Good, also good as replacement for vanilla  |    |
|  Forge  |  Mods  |  Good, may even beat Fabric depending of your scenario, but can be too heavy for less modded servers |  Foundation of Minecraft modding  |

You don't need to chose a permanent server software right away, feel free to test one, then if you feel like trying other, keep an eye in the interchangeable ones.

Once you made your choice, it's time to download them from their official website.

## Installing Java
Java 17 and 21 are available as packages, install them, respectively, with the commands `pkg install openjdk-17` or `pkg install openjdk-21`.

If you need Java 8, you can use the [installation script from MasterDevX](https://github.com/MasterDevX/Termux-Java).

In the case you don't know what version you need, just install the latest.

## Running the server
Place the server JAR file inside a dedicated folder, it will create it's folder structure in the directory you start the server, possibly messing up the organization of an non dedicated folder.

Start the server with the command `java -Xmx 1024M -Xms 512M -jar server.jar` , replace `server.jar` with your actual file name, the `Xmx` and `Xms` arguments control the server maximum and the initial RAM allocation. The initial RAM allocation still needs to be an amount enough for the server to run, otherwise the server will crash before it can allocate more.

On the first run, the server will probably ask you to change `server.properties`, that's because you need to accept the Minecraft server End User License Agreement (EULA). We'll use the trip to also configure the server in the same file, you can generate one preconfigured with [this generator](https://mctools.org/server-properties-creator). If you want to proceed without configuring the server, just set `eula=true`.

## Installing mods and plugins
It's really simple, if you're using a plugin loader, put plugins JAR files inside the folder named `plugins`, for modloaders put the JARs inside `mods`. Each loader/plugin/mod may have it's own config file, please refer to their documentation.

##  Joining the server
You can get your current IP address using the command `ip addr` or `ifconfig`, both may need superuser rights. You can also see your IP in the network properties in Android's settings app. Once you got your IP (usually looks like `192.168.18.1`) you can enter it in a Minecraft Java client connected on the same network (usually wi-fi) as the server.

*Have fun!*
