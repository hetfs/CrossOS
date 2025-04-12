exa command line tool tha work like ls ( exa will be use as ls command )
A modern replacement for ls.
You list files hundreds of times a day. Why spend your time squinting at black and white text?
exa is an improved file lister with more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git. And it’s small, fast, and just one single binary.
======= Installation ========
exa is available for macOS and Linux.

sudo apt intall exa
brew install exa  =======> macOS installation

Some simple usage examples

# List directories
exa /var/
# Long list
exa -l /var/
# Display inode, Permissions, Links, Size, Blocks, User, Group, Data Modified, Name
exa -abghHliS /var/
# Output in tree mode with attributes
exa -al --tree /var/


==============================================

Alternative Installation Method (Compile from source)
curl https://sh.rustup.rs -sSf | sh
git clone https://github.com/ogham/exa/archive/v0.9.0.tar.gz
cd exa
make install

========================================

Display Options
-1, –oneline: display one entry per line
-G, –grid: display entries as a grid (default)
-l, –long: display extended details and attributes
-R, –recurse: recurse into directories
-T, –tree: recurse into directories as a tree
-x, –across: sort the grid across, rather than downwards
-F, –classify: display type indicator by file names
–colo[u]r: when to use terminal colours
–colo[u]r-scale: highlight levels of file sizes distinctly
–icons: display icons
Filtering Options
-a, –all: show hidden and ‘dot’ files
-d, –list-dirs: list directories like regular files
-L, –level=(depth): limit the depth of recursion
-r, –reverse: reverse the sort order
-s, –sort=(field): which field to sort by
–group-directories-first: list directories before other files
-D, –only-dirs: list only directories
–git-ignore: ignore files mentioned in  .gitignore
-I, –ignore-glob=(globs): glob patterns (pipe-separated) of files to ignore
Pass the  --all  option twice to also show the  .  and  ..  directories.

Long View Options
These options are available when running with –long ( -l ):

-b, –binary: list file sizes with binary prefixes
-B, –bytes: list file sizes in bytes, without any prefixes
-g, –group: list each file’s group
-h, –header: add a header row to each column
-H, –links: list each file’s number of hard links
-i, –inode: list each file’s inode number
-m, –modified: use the modified timestamp field
-S, –blocks: list each file’s number of file system blocks
-t, –time=(field): which timestamp field to use
-u, –accessed: use the accessed timestamp field
-U, –created: use the created timestamp field
-@, –extended: list each file’s extended attributes and sizes
–changed: use the changed timestamp field
–git: list each file’s Git status, if tracked or ignored
–time-style: how to format timestamps
–no-permissions: suppress the permissions field
–no-filesize: suppress the filesize field
–no-user: suppress the user field
–no-time: suppress the time field
Valid –color options are always, automatic, and never.
Valid sort fields are accessed, changed, created, extension, Extension, inode, modified, name, Name, size, type, and none. Fields starting with a capital letter sort uppercase before lowercase. The modified field has the aliases date, time, and newest, while its reverse has the aliases age and oldest.
Valid time fields are modified, changed, accessed, and created.
Valid time styles are default, iso, long-iso, and full-iso.

========================================

https://the.exa.website/
https://github.com/ogham/exa
https://linuxhandbook.com/exa-command/

BLOG-D WITHOUT NONSENSE
https://dannyda.com/2022/11/04/exa-pretty-useful-alternative-of-ls-command-how-to-install-exa-on-ubuntu-20-04-1-lts-20-10-and-other-linux-distros/