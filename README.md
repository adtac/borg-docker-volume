### borg-docker-volume

This is a Bash script to backup a Docker volume's contents using [Borg backup](https://borgbackup.readthedocs.io/en/stable/).


#### Requirements

For a Bash script to backup Docker volumes using Borg, I'd be surprised if it had any dependency other than Bash, Docker, and Borg. To install Borg, follow the instructions provided [here](https://borgbackup.readthedocs.io/en/stable/installation.html).

To run this script, you need to be in the list of users who can execute sudo (typically, `wheel`). This is because Docker volumes are read-only to root. See the source code for more comments on where `sudo` is used.

#### Usage

First, create a Borg repository wherever you want your backups to be stored:

```bash
$ borg init --encryption=repokey /path/to/borg/repository
```

You will be prompted for a passphrase. Once you enter that, an encrypted Borg repository will be created at `/path/to/borg/repository`. I strongly recommend encrypting your backups, but if you don't want to, use `--encryption none`, and you won't be prompted for a passphrase. This step may be performed on your local machine or a remote machine with SSH access.

Next, say you have a Docker volume called `sqlite_volume` that you want to backup. Then you'd run this script as follows:

```bash
$ BORG_PASSPHRASE=password borg-docker-volume -b /path/to/borg/repository -v sqlite_volume
```

If your `/path/to/borg/repository` is on a remote server, you can give a remote address such as `user@example.com:/path/to/borg/repository`.

If you're not comfortable entering your password directly (you shouldn't be), you can save it in a file and `source(1)` it. For example, you can create a file called `/etc/borg_passphrase` and give it the desired permissions with the following contents:

```bash
$ cat /etc/borg_passphrase
export BORG_PASSPHRASE=pass
$ source /etc/borg_passphrase
$ borg-docker-volume -b /path/to/borg/repository -v sqlite_volume
```

#### License

```
Copyright 2018 Adhityaa Chandrasekar

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
