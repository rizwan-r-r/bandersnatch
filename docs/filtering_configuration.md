## Mirror filtering

The mirror filter configuration settings are in the same configuration file as the mirror settings.

There are different configuration sections for the different plugin types.

### Blacklist filtering settings

The blacklist settings are in a configuration section named **\[blacklist\]**,
this section provides settings to indicate packages, projects and releases that should
not be mirrored from pypi.

This is useful to avoid syncing broken or malicious packages.

### plugins

The plugins setting is a list of plugins to enable.

``` eval_rst
.. note: If this setting is missing, all installed plugins are enabled.
```

Example (enable all installed filter plugins):
``` ini
[blacklist]
plugins = all
```

Example (only enable filtering of whole projects/packages):
``` ini
[blacklist]
plugins =
    blacklist_project
```

### packages

The packages setting is a list of python [pep440 version specifier](https://www.python.org/dev/peps/pep-0440/#id51) of packages to not be mirrored.

Any packages matching the version specifier will not be downloaded.

Example:
``` ini
[blacklist]
packages =
    example1
    example2>=1.4.2,<1.9,!=1.5.*,!=1.6.*
```

### Prerelease filtering

Bandersnatch includes a plugin to filter our pre-releases of packages. To enable this plugin simply add `prerelease_release` to the enabled plugins list.

``` ini
[blacklist]
plugins =
    prerelease_release
```

### Regex filtering

Advanced users who would like finer control over which packages and releases to filter can use the regex Bandersnatch plugin.

This plugin allows arbitrary regular expressions to be defined in the configuration, any package name or release version that matches will *not* be downloaded.

The plugin can be activated for packages and releases separately. For example to activate the project regex filter simply add it to the configuration as before:

``` ini
[blacklist]
plugins =
    regex_project
```

If you'd like to filter releases using the regex filter use `regex_release` instead.

The regex plugin requires an extra section in the config to define the actual patterns to used for filtering:

``` ini
[filter_regex]
packages =
    .+-evil$
releases =
    .+alpha\d$
```

Note the same `filter_regex` section may include a `packages` and a `releases` entry with any number of regular expressions.