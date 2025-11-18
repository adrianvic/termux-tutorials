# Installing Copyparty for file sharing
An essential component to every server is file sharing, and for that you can use Copyparty, which is a handy file server contained in a single Python script - making it portable, while still fast and feature rich.

## Getting the files
You can download the latest release of Copyparty on the releases tab of the GitHub repository. [Here's a link to save you time.](https://github.com/9001/copyparty/releases/latest)

You'll usually want to download `copyparty-sfx.py`, but you can refer to the software documentation about the other versions.

## Creating a service
For managing your server more comfortably, we're going to create a service for your file server. This allows the server to run on background, without locking a shell session just to output it's logs, and you can easily start and stop the server without the need to manually kill it's process.

Before continuing, make sure the package `termux-services` is installed. Then you can run this command to create it's service.

```bash
mkdir -p $PREFIX/var/service/copyparty/log
ln -sf $PREFIX/share/termux-services/svlogger $PREFIX/var/service/copyparty/log/run
```

Then edit `$PREFIX/var/service/copyparty/run` with something like this:

```bash
# Replace this path with the directory you put your Copyparty Python file.
cd ~/path/to/your/python/file
# Replace the path after '-c' with the path to where you want to keep your config file.
exec python copyparty-sfx.py -c /sdcard/copyparty.conf
```

**Remember:** by default, Copyparty will only manage subdirectories of the directory it is in.

## Configuring
Create the file you specified in the `-c` parameter of your startup script, then open it with a text editor.

This is a simple config that starts a server in the port `8080`, with a single user called `a dmin` with a password `changeme` and read-write-modify-delete permissions:

```yaml
[global]
  p: 8080
  e2dsa
  e2ts
  z, qr

# create users:
[accounts]
  admin: changeme   # username: password

# create volumes:
[/]         # create a volume at "/" (the webroot), which will
  .         # share the contents of "." (the current directory)
  accs:
    rwmd: admin  # read-write-modify-delete
```

## Starting
If you created a service for Copyparty, you can start it by running the command `sv up copyparty` and stop with `sv down copyparty`. If you want to start it automatically run `sv enable copyparty`.

If you didn't make a service, you can start the server by running Copyparty's Python file with the command `python copyparty-sfx.py`.
