# Rocky vocabularies

Project for creating custom dictionaries for vocabulary words after installing the `vale` linter in NvChad.

## Purpose of the project

The purpose of the project is to create a set of vocabularies for use when writing Rocky Linux documentation. The vocabularies are needed to remove warnings issued by *Vale* for unknown words as in the following example:

> Use correct American English spelling. Did you really mean 'Rocky'?

This term is perfectly fine but not known to `vale` so these messages can clutter up the display, making it difficult to actually see the warnings that you need.

To remove these kinds of warnings, it is necessary to provide a list of words so that *Vale* will consider them correct, and for this purpose Vale's developers introduced *vocabularies*.  
Vocabularies are folders containing two files, the *accept.txt* (words that are correct) and *reject.txt* (words that are incorrect). When changing these files, enter the words one per line. Using wild cards is possible to widen the hits for various words.

## Structure of a Vale configuration

In a standard Vale structure the *styles* folder has the selected styles (in this case `RedHat` and `alex`):

```text
styles
├── alex
└── RedHat
```

The configuration file sets the search path for styles. The level of alerts should have (warning, suggestion, and so on) and the styles used:

```ini
StylesPath = ~/.local/share/vale/styles

MinAlertLevel = suggestion

Packages = RedHat, alex

[*]
BasedOnStyles = Vale, RedHat, alex
```

## Structure of a dictionary

The dictionary consists of two files in a single folder, an *accept.txt* file that has the words that Vale should consider as correct, in this case the *nvchad* folder's *accept.txt* contents are:

```txt
Devicons
formatters
(?i)chadrc
(?i)nvchad
(?i)nvim
[Yy]aml
....
```

The second *reject.txt* file, unless there are special needs, is usually empty. It is for flagging words that are wrong in every context, which in the Rocky documentation is already handled by *RedHat* styles for the technical part and by *alex* for identifying polarizing words in Markdown files.

## Installation of dictionaries

The dictionaries are ready to use. Integration into an existing configuration is done with the *config* folder in your *styles* path. Assuming the initialization downloaded the styles in `.local/share/vale/styles`, you will first proceed to create the folder:

```bash
mkdir ~/.local/share/vale/styles/config
```

Once you have created the folder you have to populate it with vocabularies. To do this the quickest thing is to clone the repository into the *config* folder:

```bash
git clone https://github.com/ambaradan/vale-at-rocky ~/.local/share/vale/styles/config/
```

After finishing the copy, the *config* folder will have the following structure:

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

The next step is to edit the *.vale.ini* configuration file in the home folder. This facilitates adding the entries in *accept.txt* to each exception list in all styles listed in *BasedOnStyles*, and also adding all words found in the *accept.txt* vocabulary files to the substitution rule (Vale.Terms) for matching checks.  
Instead, entries in *reject.txt* are automatically added to an existence rule (Vale.Avoid) that flags all occurrences as errors.

**NOTE:** Inclusion of vocabularies in all styles listed in *BasedOnStyles* offers the advantage of making vocabularies independent. This means that you only need to update the project vocabulary to customize third-party styles instead of updating the styles themselves.

To add vocabularies to the configuration file, just add a line that includes the vocabularies you want to use:

```txt
Vocab = rockydocs, nvchad, terminology
```

Insert vocabularies available in the project as needed, by adding or removing them from *Vocab*. So, if you do not write documentation on NvChad it is unlikely that the corresponding vocabulary will be useful.  
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

Vocabularies are continuously updated documents, and contribution is an important part of their construction.

Vale allows some dedicated settings for terms:

```text
(?i)linux
[Rr]ocky
```

The entry, (?i)linux, marks the entire pattern as case-insensitive, and the entry [Rr]ocky, provides two acceptable options, either capitalization or no capitalization.

## Initializing and updates using `vale sync`

**NOTE:** These next commands assume the creation of the `.vale.ini` file in the root of your home folder. The styles path should be adjusted to match your styles path. To do this, replace the `<nvim_name_config>` with what matches your setup.

The `vale sync` command creates the styles folder initially, and keeps all styles updated on your system when used periodically. In *NvChad*, the `vale` binary exists in:

```bash
~/.local/share/<nvim_name_config>/mason/bin/vale
```

To run `vale sync`, since that folder is not in your PATH, you need to run it from that folder. The easier way is to add the vale binary to your PATH so that you can run `vale sync` from anywhere. To do this, change your `.bashrc` file by appending this line to the end of it:

```bash
PATH="~/.local/share/<nvim_name_config>/bin/vale:$PATH"
```

Save your `.bashrc` and then run the `bash` command to read the file in, and update your PATH.

Verify that the updating of the PATH by running:

```bash
echo $PATH
```

If the PATH shows the line you just added, run `vale sync` to either initialize or update your styles.

## Conclusion

Using `vale` in NvChad with properly populated dictionaries will help when checking your document with `vale`. It will end the screen clutter that comes from words that `vale` does not know to be correct by default.
