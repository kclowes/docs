---
tags: cli
---

## How to install the CLI

You can obtain both versions for Mac OSX and Linux of the CLI at the
following locations:

* [Linux 32bit](http://downloads.wercker.com/cli/stable/linux_386/wercker)
* [Linux 64bit](http://downloads.wercker.com/cli/stable/linux_amd64/wercker)
* [Mac 64bit](http://downloads.wercker.com/cli/stable/darwin_amd64/wercker)

For example downloading the Mac OSX version and
installing it in `/usr/local/bin` (make sure this directory is available
in your PATH):

```sh
# downloads the Mac OSX CLI to /usr/local/bin (YMMV)
curl http://downloads.wercker.com/cli/stable/darwin_amd64/wercker -o /usr/local/bin/wercker
```

And for Linux 64 bit (make sure this directory is available in your
PATH):

```sh
# downloads the Linux CLI to /usr/local/bin (YMMV)
curl http://downloads.wercker.com/cli/stable/linux_amd64/wercker -o /usr/local/bin/wercker
```
Next you want to make the CLI executable:

```sh
chmod +x /usr/local/bin/wercker
```

You can use the `wercker version` command to see if a new version of the
CLI is available. See the [commands section](/docs/cli/commands.html) for more information.
Some commands require Docker to be installed/available, see 
[requirements](/docs/cli/requirements.html) for more information.
