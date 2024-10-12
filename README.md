# Rocky Docs - Vale Styles

Project for creating custom dictionaries for vocabulary words after installing the `vale` linter in NvChad.

## Purpose of the project

When checking your document with `vale` you might receive warnings such as this:

> Use correct American English spelling. Did you really mean 'Rocky'?

This term is perfectly fine but not known to `vale` so these messages can clutter up the display, making it difficult to actually see the warnings that you need. Using this repository and customizing it with your own vocabulary words will help you use `vale` within NvChad without all of the clutter.

### Requirements

- NvChad 2.0 properly installed with the [Chadrc Template](https://docs.rockylinux.org/books/nvchad/template_chadrc/) provided by the developers.
- `vale` properly installed with [Mason in NvChad 2.0](https://https://docs.rockylinux.org/books/nvchad/vale_nvchad/)
- Git

## Installation

Your installation is in the Neovim shared folder `~/.local/share/nvim/`, this is to keep it hidden from the user's folders and to conform it to NvChad.

First, clone the repository in the Neovim shared folder:

```bash
cd ~/.local/share/nvim/
git clone https://github.com/ambaradan/vale-styles.git
```

The repository contains a correctly configured `.vale.ini` file  that will replace the one that you installed before. The initialization that follows provides integration of custom dictionaries for Rocky documentation. These dictionaries can be fully customized to include your own words.

```text
StylesPath = ~/.local/share/nvim/vale-styles

Vocab = rockydocs, terminology

MinAlertLevel = suggestion

Packages = RedHat, alex

[*]
BasedOnStyles = Vale, RedHat, alex
```

copy the file to your home directory:

```bash
cp ~/.local/share/nvim/vale-styles/.vale.ini ~/.
```

and initialize the styles and dictionaries:

```bash
cd ~/
~/.local/share/nvim/mason/packages/vale/vale sync
 SUCCESS  Downloaded package 'RedHat'                                                                 
 SUCCESS  Downloaded package 'alex'                                                                    
Downloading packages [2/2] █████████████████████████████████████████████ 100% | 2s
```

The dictionaries will download to `~/.local/share/nvim/vale-styles/` and a **.gitignore** has is already there to prevent them from sharing with Git.

```text
cat ~/.local/share/nvim/vale-styles/.gitignore
alex
RedHat
```

### Dictionaries

Dictionaries are basically a folder containing two files, an **accept.txt** and a **reject.txt**. In *accept.txt* you enter the custom terms one per line. The terms are case-sensitive by default.

Vale allows some dedicated settings for terms:

```text
(?i)linux
[Rr]ocky
```

The entry, (?i)linux, marks the entire pattern as case-insensitive, and the entry [Rr]ocky, provides two acceptable options.

## Conclusion

Using `vale` in NvChad 2.0 with properly populated dictionaries will help when checking your document with `vale`. It will eliminate the screen clutter that comes from words that `vale` does not know to be correct by default.
