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

* about, help, version, selfupdate
* authorize
* backend
* bisync, sync
* cat
* completion
* config
* copy, copyto, copyurl
* cryptcheck, cryptdecode
* dedupe
* delete, deletefile, purge, cleanup
* genautocomplete
* gendocs
* check, checksum, hashsum, md5sum, sha1sum
* link
* listremotes
* ls, lsd, lsf, lsjson, lsl, size
* mount
* move, moveto
* ncdu, tree
* obscure
* rc
* rcat
* rcd
* mkdir, rmdir, rmdirs
* serve
* settier
* test
* touch


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


