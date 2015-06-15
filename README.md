# sbodeps
*Heretically convenient dependency resolver for sbopkg*

When you're trying to install something like SpamAssassin and its 41 multi-layered dependencies, life is too short to bother with hunting down the dependencies manually. The [slackbuilds.org](http://www.slackbuilds.org/) repository includes all the information necessary to automate this in computer-readable form, but there is no convenient tool that utilises it.

[`sbopkg`](http://www.sbopkg.org/) is a high-quality and mature tool that automates compilation and installation of slackbuilds.org packages, but out of respect for Slackware liturgy it forgoes resolving dependencies. So I wrote `sbodeps`, a companion utility for `sbopkg` that makes dependency resolution heretically fast and convenient without taking any control away from the administrator.

## Requires

* A configured and working `sbopkg` installation. See http://www.sbopkg.org/ for more information.

## Features

* Fast, on-the-fly dependency resolution. Unlike with `/usr/doc/sbopkg-0.37.0/contrib/sqg`, there is no need to store build queues in advance, although you can do.
* Given no options, `sbodeps` simply outputs the build queue for the specified package(s) to the terminal. Use output redirection or the `-q` option to store it wherever you want. Use the `-Q` option to store it in `sbopkg`'s queues directory like `sqg` does, so you can use it with `sbopkg`'s dialog interface.
* Unlike `sqg`, `sbodeps` skips dependencies that are already installed. Use the `-a` option to include them.
* To install one or more packages and their dependencies, `sbodeps` does not need a stored build queue at all; instead, it will directly construct a command line for `sbopkg` that installs, in the correct order, the specified packages and all their dependencies that aren't already installed. (If `-a` is given, it will rebuild and reinstall even the already-installed ones.) Installing SpamAssassin and its 41 dependencies is as simple as saying `sbodeps -i spamassassin`.
* To remove a package and its installed slackbuilds.org dependencies, use the `-r` option. Caution is advised; `sbodeps` will build and show a `removepkg` command and ask for confirmation before executing it.
* Shows a pointer to the package's README file if it has are optional dependencies (meaning, if there is a `%README%` tag in the .info file).
* Does not require root just to show or store a build queue.
* Respectful towards the [Slackware philosophy](http://docs.slackware.com/slackware:philosophy): it does not take any control away from the administrator, shows exactly what it will do before doing it, and changes nothing without express prior permission.
* Shamelessly heretical towards the Slackware philosophy: it might tempt someone to admit that automated dependency resolution can be awfully convenient. ;-)

## Usage and options

    Usage: sbodeps [OPTION...] [PACKAGENAME...]
    Options:
      -a, --all         include all dependencies, even those already installed
      -Q, --queue       store build queue in $QUEUEDIR/PACKAGENAME.sqf
      -q FILE, --qfile=FILE, --queuefile=FILE
                        store build queue in FILE
      -i, --install     instead of storing build queue, install packages now
      -r, --remove      remove a package and its dependencies (be careful!)
      -v, --version     show version number  
      -h, --help        show this help
      -L, --licen[sc]e  show license
