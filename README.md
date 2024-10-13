<!-- vale off -->
# Rocky vocabularies

Project for creating custom dictionaries for vocabulary words after installing the `vale` linter in NvChad.

## Purpose of the project

The purpose of the project is to create a set of vocabularies for use when writing Rocky Linux documentation. The vocabularies are needed to remove warnings issued by *Vale* for unknown words as in the following example:

> Use correct American English spelling. Did you really mean 'Rocky'?

This term is perfectly fine but not known to `vale` so these messages can clutter up the display, making it difficult to actually see the warnings that you need.

To remove these kinds of warnings, it is necessary to provide *Vale* with a list of words that should be considered correct, and for this purpose Vale's developers introduced *vocabularies*.  
Vocabularies are folders containing two files, the *accept.txt* and *reject.txt* files, which contain the words to be considered correct and those to be considered incorrect, respectively. The words are entered one per line and may have wildcards that affect the choice result.

## Structure of a Vale configuration

In a standard Vale structure the *styles* folder has the selected styles (in this case `RedHat` and `alex`):

```text
styles
├── alex
└── RedHat
```

The configuration file set the search path for the styles, the level the alerts should have (warning, suggestion, and so on) and the styles used:

```ini
StylesPath = ~/.local/share/vale/styles

MinAlertLevel = suggestion

Packages = RedHat, alex

[*]
BasedOnStyles = Vale, RedHat, alex
```

## Structure of a dictionary

The dictionary consists of two files in a single folder, an *accept.txt* file that has the words that Vale should consider as correct, in this case *nvchad/accept.txt*:

```txt
Devicons
formatters
(?i)chadrc
(?i)nvchad
(?i)nvim
[?i]yaml
....
```

The second *reject.txt* file unless there are special needs is usually empty. It is for flagging words not to be used, which in the Rocky documentation is already handled by *RedHat* styles for the technical part and by *alex* for identifying polarizing words in Markdown files.

## Installation of dictionaries

The dictionaries are ready to use and are easily integrated into an existing configuration, to do this you need to create a *config* folder in your *styles*, assuming the previous initialization downloaded the styles in `.local/share/vale/styles` you will first proceed to create the folder:

```bash
mkdir ~/.local/share/vale/styles/config
```

Once you have created the folder you have to populate it with vocabularies, to do this the quickest thing is to clone the repository into the *config* folder:

```bash
git clone https://github.com/ambaradan/vale-at-rocky ~/.local/share/vale/styles/config/
```

After the copy is finished, the *config* folder will have the following structure:

```txt
.local/share/vale/styles/config/
└── vocabularies
    ├── brands
    │   ├── accept.txt
    │   └── reject.txt
    ├── nvchad
    │   ├── accept.txt
    │   └── reject.txt
    ├── rockydocs
    │   ├── accept.txt
    │   └── reject.txt
    └── terminology
        ├── accept.txt
        └── reject.txt
```

The next step is to edit the *.vale.ini* configuration file in the home folder, so the entries in *accept.txt* are added to each exception list in all styles listed in *BasedOnStyles*, and in addition all words found in the *accept.txt* vocabulary files are added to the substitution rule (Vale.Terms) for matching checks.  
Instead, entries in *reject.txt* are automatically added to an existence rule (Vale.Avoid) that flags all occurrences as errors.

**NOTE:** Inclusion of vocabularies in all styles listed in *BasedOnStyles* offers the advantage of making vocabularies independent, this means that you only need to update the project vocabulary to customize third-party styles instead of updating the styles themselves.

The string to be entered in the configuration file is as follows:

```txt
Vocab = rockydocs, nvchad, terminology
```

Vocabularies available in the project can be inserted as needed by going to add or remove them from *Vocab*, e.g. if you do not write documentation on NvChad it is unlikely that the corresponding vocabulary will be useful.  
When finished, the *.vale.ini* file will look as follows:

```ini
StylesPath = ~/.local/share/vale/styles

Vocab = rockydocs, nvchad, terminology

MinAlertLevel = suggestion

Packages = RedHat, alex

[*]
BasedOnStyles = Vale, RedHat, alex
```

Now words such as *Rocky*, *Ansible*, *Incus* will no longer produce warnings from Vale allowing correction of the real problems encountered.

## WIP Contributing vocabularies

Vale allows some dedicated settings for terms:

```text
(?i)linux
[Rr]ocky
```

The entry, (?i)linux, marks the entire pattern as case-insensitive, and the entry [Rr]ocky, provides two acceptable options.

## Conclusion

Using `vale` in NvChad with properly populated dictionaries will help when checking your document with `vale`. It will end the screen clutter that comes from words that `vale` does not know to be correct by default.
