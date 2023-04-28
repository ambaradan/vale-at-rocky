# Vale at Rocky

Project to create a custom style of `vale` for Rocky Linux.

The `rocky` folder should be copied to the folder defined in *.vale.ini*:

```ini
StylesPath = ~/.local/share/nvim/vale-styles
```

and should be added to `BaseOnStyle`:

```yaml
BasedOnStyles = Vale, RedHat, alex, rocky
```

