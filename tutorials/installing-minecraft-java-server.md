# Installing Minecraft: Java Edition Server
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
|  Purpur  |  Plugins  |  Applies Pufferfish optimizations  |  Fork of - thus compatible with - PaperMC with patches from Pufferfish and a lot of other features  |
|  Pufferfish  |  Plugins  |  Enterprise grade performance  |  Designed for large enterprise servers, fork of - thus compatible with - PaperMC  |
|  PaperMC  |  Plugins  |  Standard optimizations  |  Fork of - thus compatible with - Bukkit  |
|  Spigot  |  Plugins  |  Better than Bukkit  |  Not used too much anymore as PaperMC and it's forks are generally better  |
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

On the first run, the server will probably ask you to change `eula.txt`, that's because you need to accept the Minecraft server End User License Agreement (EULA). We'll use the trip to also configure the server in a file named `server.properties`, which uses similar syntax. You can generate one preconfigured with [this generator](https://mctools.org/server-properties-creator). If you want to proceed without configuring the server, just accept the EULA.

## Managing the server after first run
Just running the server probably isn't enough for you. You probably need to keep the server running after a SSH session ends, or get back to console after detaching the server from your session.

### Keeping the server running when session ends
To keep your server running, for example, when a SSH session ends, you can use the command `nohup`, which wraps around your server and ignores the hangup signal, the one that stops your server when the session ends. Simply prefix your server start command with `nohup`, for example: `nohup java -jar purpur.jar`. Remember that `nohup` **will not** ignore other signals, pressing CTRL+C **will** kill your server.

The server output will be written to a file named `nohup.out`, you can see it live with the command `tail -f nohup.out`.

### Regaining access to console
After loosing access to the console from the session that started the server, one may want to regain access to said console.

If you're running PaperMC, you can use a plugin called [NPanel](https://hangar.papermc.io/nerotvlive/npanel), which provides a web interface with console included.

However NPanel is getting outdated and appears to not be actively maintained anymore, thus one may want to use Minecraft's built-in remote access tool, the *rcon*.

To use rcon, first you need to enable it in `server.properties` and set a password (needed):
```properties
enable-rcon=true
rcon.port=25575
rcon.password=your_rcon_pasword
```

After setting up rcon on the server, you'll need yo compile *mcrcon*, the client for rcon. You can do so by running the following commands:

```bash
git clone https://github.com/Tiiffi/mcrcon
cd mcrcon
make
mv mcrcon $PREFIX/bin
```

After this you can reload your shell (usually by running `bash` or `zrc`) and the command `mcrcon` will be available. Then you can connect to the server with the following command:

```
mcrcon -p your_rcon_pasword
```

You may also run commands directly by appending them to the end of the command, below is an example that stops your server:

```
mcrcon -p your_rcon_pasword stop
```

### Automatic restart and stopping
One may want to restart the server when it shuts down, preventing further downtime and manual intervention, but a way to stop the server from this restart loop would be nice too, below are two scripts that work together to start, automatically restart, and stop on-demand:

```bash
#!/data/data/com.termux/files/usr/bin/sh

cd your_server_location/ # Remember to replace this with your server location

while true; do
    if [[ -f ~/.srv-stop ]]; then
        echo "Stopping server."
        rm ~/.srv-stop
        exit 0
    else
        echo "Starting server."
        # Remember to replace purpur.jar with your server JAR
        nohup java -Xmx1500M -jar purpur.jar -nogui
        echo "Server stopped. Restarting in 5 seconds..."
        sleep 5
    fi
done
```

```bash
#!/data/data/com.termux/files/usr/bin/sh

touch ~/.srv-stop # Creating the magic file
mcrcon -P 25575 -p 'your_rcon_password' stop
```

## Installing mods and plugins
It's really simple, if you're using a plugin loader, put plugins JAR files inside the folder named `plugins`, for modloaders put the JARs inside `mods`. Each loader/plugin/mod may have it's own config file, please refer to their documentation.

## Joining the server
You can get your current IP address using the command `ip addr` or `ifconfig`, both may need superuser rights. You can also see your IP in the network properties in Android's settings app. Once you got your IP (usually looks like `192.168.18.1`) you can enter it in a Minecraft Java client connected on the same network (usually wi-fi) as the server.

*Have fun!*
