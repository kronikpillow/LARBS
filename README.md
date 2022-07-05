# why this fork of LARBS?

I have been a long time LARBS user and I find that the [default (LARBS) repo](https://github.com/LukeSmithxyz/LARBS)
comes with missing dependencies & missing functionality compared to the settings
located in [Luke's (voidrice) repo](https://github.com/lukesmithxyz/voidrice), and that his "rice" isn't really a rice since it
doesn't come with a uniform look, or any look at all, instead it comes as a
complete mess of various colors and looks ugly by default ...

This is a attempt to fix that ...

Apart from covering the missing dependencies of Luke's dotfiles this fork
comes with a few extras that i find essential to my Linux experience, and is
riced with the Nord colorscheme everywhere possible.

Instead of using Luke's dotfiles the installer installs [my own (voidrice) fork](https://github.com/kronikpillow/voidrice)
which is riced with the Nord colorscheme and contains a few extras to complement
the [my (voidrice) fork](https://github.com/kronikpillow/voidrice).

Currently I don't maintain compatibility with Artix on [my (voidrice) fork](https://github.com/kronikpillow/voidrice) as
I don't use Artix or other distro's or init systems and don't care about
maintaining compatibility, as I'm not a Linux guru and I'v found my experience
on other init systems unpleasant due to lack of support, tutorials & examples,
so for me living with systemd atm is just fine.

## Luke's Auto-Rice Bootstrapping Scripts (LARBS)

### Installation:

On an Arch-based distribution as root, run the following:

```
curl -LO https://raw.githubusercontent.com/kronikpillow/LARBS/master/larbs.sh
sh larbs.sh
```

That's it.

### What is LARBS?

LARBS is a script that autoinstalls and autoconfigures a fully-functioning
and minimal terminal-and-vim-based Arch Linux environment.

LARBS can be run on a fresh install of Arch or Artix Linux, and provides you
with a fully configured diving-board for work or more customization.

### Customization

By default, LARBS uses the programs [here in progs.csv](progs.csv) and installs
[my fork of (voidrice) here](https://github.com/kronikpillow/voidrice),
but you can easily change this by either modifying the default variables at the
beginning of the script or giving the script one of these options:

- `-r`: custom dotfiles repository (URL)
- `-p`: custom programs list/dependencies (local file or URL)
- `-a`: a custom AUR helper (must be able to install with `-S` unless you
  change the relevant line in the script

#### The `progs.csv` list

LARBS will parse the given programs list and install all given programs. Note
that the programs file must be a three column `.csv`.

The first column is a "tag" that determines how the program is installed, ""
(blank) for the main repository, `A` for via the AUR or `G` if the program is a
git repository that is meant to be `make && sudo make install`ed.

The second column is the name of the program in the repository, or the link to
the git repository, and the third column is a description (should be a verb
phrase) that describes the program. During installation, LARBS will print out
this information in a grammatical sentence. It also doubles as documentation
for people who read the CSV and want to install my dotfiles manually.

Depending on your own build, you may want to tactically order the programs in
your programs file. LARBS will install from the top to the bottom.

If you include commas in your program descriptions, be sure to include double
quotes around the whole description to ensure correct parsing.

#### The script itself

The script is extensively divided into functions for easier readability and
trouble-shooting. Most everything should be self-explanatory.

The main work is done by the `installationloop` function, which iterates
through the programs file and determines based on the tag of each program,
which commands to run to install it. You can easily add new methods of
installations and tags as well.

Note that programs from the AUR can only be built by a non-root user. What
LARBS does to bypass this by default is to temporarily allow the newly created
user to use `sudo` without a password (so the user won't be prompted for a
password multiple times in installation). This is done ad-hocly, but
effectively with the `newperms` function. At the end of installation,
`newperms` removes those settings, giving the user the ability to run only
several basic sudo commands without a password (`shutdown`, `reboot`,
`pacman -Syu`).
