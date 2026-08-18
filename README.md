# lsc (ls comments)

A Python-based `ls -l` alternative with support for displaying and exporting file comments from `descript.ion`, macOS Finder, and zip archives.

## Features

- **Standard `ls -l` layout**: Lists permissions, owner, group, size, modification date, and filename.
- **Comment/Description Column**: Displays comments in a dedicated column, retrieved from:
  - `descript.ion` files (cross-platform plain-text description files).
  - **macOS Finder Comments** (stored in `com.apple.metadata:kMDItemFinderComment` extended attributes).
  - **ZIP file descriptions** (extracted from `file_id.diz` inside `.zip` files).
- **Git Friendly**: Export macOS Finder comments into version-controlled `descript.ion` files.
- **Readable Colors**: Optimized CLI colors for dark themes (like iTerm2).
- **Locale-aware Date Formatting**: Localized month names and intelligent date formatting.

## Installation

Ensure you have Python 3 installed. You can place the `lsc` script in your shell path (e.g., `/usr/local/bin`):

```bash
chmod +x lsc
mv lsc /usr/local/bin/lsc
```

## Usage

List contents of the current directory:
```bash
lsc
```

List all files (including hidden files starting with `.`):
```bash
lsc -a
```

### Adding or Modifying Comments

You can set or clear a comment for a file or directory using the `--set-comment` option. This will save the comment directly into the local `descript.ion` file and, if you are running macOS, it will also write the comment to the file's macOS Finder Comment attribute:

```bash
lsc --set-comment "My awesome file" example.txt
```

To clear a comment:
```bash
lsc --set-comment "" example.txt
```

### Saving Comments to Git

Since macOS Finder comments are stored in filesystem extended attributes, they cannot be tracked by Git directly. You can export them into standard `descript.ion` files:

```bash
lsc --export-comments [paths...]
```

This will generate or update a `descript.ion` file in the target directory, which you can then add to your Git repository:

```bash
git add descript.ion
git commit -m "Save file comments"
```

## CLI Options

```
usage: lsc [--help] [-l] [-a] [-A] [-h] [-d] [-r] [-S] [-t] [--color {always,never,auto}] [--export-comments] [--set-comment COMMENT] [paths ...]

positional arguments:
  paths                 files or directories to list (default: '.')

options:
  --help                show this help message and exit
  -l                    long listing format (always enabled)
  -a, --all             do not ignore entries starting with .
  -A, --almost-all      do not list implied . and ..
  -h, --human-readable  print sizes in human readable format
  -d, --directory       list directories themselves, not their contents
  -r, --reverse         reverse order while sorting
  -S                    sort by file size
  -t                    sort by modification time
  --color {always,never,auto}
                        colorize the output (default: auto)
  --export-comments     export macOS Finder comments to descript.ion files
  --set-comment COMMENT
                        set comment for specified files or directories
```
