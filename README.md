# The rsync for the cloud era: Rclone

> Lightning Talk, [Leipzig Gophers](https://golangleipzig.space) [#26](https://golangleipzig.space/posts/meetup-26-invitation/), 2022-04-26 19:00 CEST

## Things past

> This chapter describes the rsync algorithm, an algorithm for the efficient remote up-
date of data over a high latency, low bandwidth link. -- [Efficient Algorithms for Sorting and
Synchronization](https://maths-people.anu.edu.au/~brent/pd/Tridgell-thesis.pdf) (1999)

A glimpse into how practical software projects originate:

> The rsync algorithm was developed out of my **frustration** with the long time it took
to send changes in a source code tree across a dialup network link. I have spent a
lot of time developing several large software packages during the course of my PhD
and **constantly finding myself waiting** for source files to be transferred through my
modem for archiving, distribution or testing on computers at the other end of the link
or on the other side of the world. **The time taken to transfer the changes gave me
plenty of opportunity to think about better ways of transferring files**.

* So dial up and slow network links

An annoyance in the 2010s is the variety of cloud storage providers.

* in [2014](http://web.archive.org/web/20141119184433/http://rclone.org/), rclone supported 6 providers (S3, GCP and GDrive, Dropbox, OpenStack, local)
* in [2022](...), rclone supports over 40 storage systems (e.g. QingStor, Storj, put.io, Koofr, ..., also meta things like Union or Chunker)

But 23 years after rsync, rclone still mentions:

> Transfers over **limited bandwidth**; intermittent connections ...

## Example Usage

Interactive configuration of a remote. Setup remote.

![](1-gcp-screen.png)

Rclone has a bit unusual interactive config, which comes in handy for copy-and-pasting ids, secrets and tokens.

![](2-rclone-interactive-config.png)

Finally, temporary rclone server reports: Success.

![](3-rclone-success.png)

## Subcommands

* [ ] about           Get quota information from the remote.
* [ ] authorize       Remote authorization.
* [ ] backend         Run a backend-specific command.
* [ ] bisync          Perform bidirectonal synchronization between two paths.
* [ ] cat             Concatenates any files and sends them to stdout.
* [ ] check           Checks the files in the source and destination match.
* [ ] checksum        Checks the files in the source against a SUM file.
* [ ] cleanup         Clean up the remote if possible.
* [ ] completion      Generate the autocompletion script for the specified shell
* [ ] config          Enter an interactive configuration session.
* [ ] copy            Copy files from source to dest, skipping identical files.
* [ ] copyto          Copy files from source to dest, skipping identical files.
* [ ] copyurl         Copy url content to dest.
* [ ] cryptcheck      Cryptcheck checks the integrity of a crypted remote.
* [ ] cryptdecode     Cryptdecode returns unencrypted file names.
* [ ] dedupe          Interactively find duplicate filenames and delete/rename them.
* [ ] delete          Remove the files in path.
* [ ] deletefile      Remove a single file from remote.
* [ ] genautocomplete Output completion script for a given shell.
* [ ] gendocs         Output markdown docs for rclone to the directory supplied.
* [ ] hashsum         Produces a hashsum file for all the objects in the path.
* [ ] help            Show help for rclone commands, flags and backends.
* [ ] link            Generate public link to file/folder.
* [ ] listremotes     List all the remotes in the config file.
* [ ] ls              List the objects in the path with size and path.
* [ ] lsd             List all directories/containers/buckets in the path.
* [ ] lsf             List directories and objects in remote:path formatted for parsing.
* [ ] lsjson          List directories and objects in the path in JSON format.
* [ ] lsl             List the objects in path with modification time, size and path.
* [ ] md5sum          Produces an md5sum file for all the objects in the path.
* [ ] mkdir           Make the path if it doesn't already exist.
* [ ] mount           Mount the remote as file system on a mountpoint.
* [ ] move            Move files from source to dest.
* [ ] moveto          Move file or directory from source to dest.
* [ ] ncdu            Explore a remote with a text based user interface.
* [ ] obscure         Obscure password for use in the rclone config file.
* [ ] purge           Remove the path and all of its contents.
* [ ] rc              Run a command against a running rclone.
* [ ] rcat            Copies standard input to file on remote.
* [ ] rcd             Run rclone listening to remote control commands only.
* [ ] rmdir           Remove the empty directory at path.
* [ ] rmdirs          Remove empty directories under the path.
* [ ] selfupdate      Update the rclone binary.
* [ ] serve           Serve a remote over a protocol.
* [ ] settier         Changes storage class/tier of objects in remote.
* [ ] sha1sum         Produces an sha1sum file for all the objects in the path.
* [ ] size            Prints the total size and number of objects in remote:path.
* [ ] sync            Make source and dest identical, modifying destination only.
* [ ] test            Run a test command
* [ ] touch           Create new file or change file modification time.
* [ ] tree            List the contents of the remote in a tree like fashion.
* [ ] version         Show the version number.


## Plugin Architectures in Go

Go has limited support for hot-loading of Go code, e.g. via
[plugin](https://pkg.go.dev/plugin), and HashiCorp built a system for [plugins
over RPC](https://github.com/hashicorp/go-plugin) (which only appeared in
2016).

Rclone needs backends be in the source tree and compiled into the binary to
work. Not great, not terrible.

## Writing a new backend

Currently, I'm writing an Rclone backend for a digital preservation system
(DPS), which we are developing at the Archive. The DPS provides an API and we
can map a directory structure to a set of API calls to present a filesystem
like interface.

## Interface architecture


