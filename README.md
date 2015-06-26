ModuleSync Configs
==================

This repository is a modified version of [modulesync](http://github.com/puppetlabs/modulesync) that suits the needs of nToggle. It contains default configuration for the nToggle modules. Changes to this repository should be synced with modulesync
across all of the nToggle modules.

A full description of ModuleSync can be found in [ModuleSync's
README](https://github.com/puppetlabs/modulesync). This README describes how
the templates are rendered in the nToggle configuration.

Configuring ModuleSync
----------------------

**`modulesync.yml`**

A key-value store of arguments to pass to ModuleSync. Each key is the name of a
flag argument to the msync command. For example, `namespace: myusername`
represents passing `--namespace myusername` to msync. This file does not appear
in this repository because it only serves to override default configuration. To
override the default configuration, the file may look something like this:

```
---
namespace: yourusername
branch: yourbranch
```

**`managed_modules.yml`**

A YAML array containing the names of the modules to manage.

Defining Module Files
---------------------

**`config_defaults.yml`**

Each first-level key in this file is the name of a file in a module to manage.
These files only appear here if there are templates in the moduleroot/
directory that need to be rendered with some default values that might be
overridden. The files listed do not necessarily represent all the files that
will be managed. The files in moduleroot/ represent all the files that will be
managed, except for unmanaged and deleted files (see [#Special Options]).

**`.sync.yml`**

This file should appear in the module itself if there are any values to
override from the config_defaults.yml file or if there are any additional
values to assign. A description of what optional values can be defined in
.sync.yml follows in the description of each file in moduleroot/. .sync.yml
will have the same format as config_defaults.yml.

#### Note

Each template is rendered in slightly different ways. Your templates to not
need to be identical to these, as long as your config_defaults.yml or .sync.yml
files contain as first-level keys the exact names of the files you are
managing and appropriately handle the data structures you use in your templates
(arrays versus hashes versus single values).

**`moduleroot/CONTRIBUTING.md`**

Flat file, documentation on how to contribute.

**`moduleroot/.gitignore`**

Contains some standard files to ignore. You can pass in additional files as an
array with the key "paths" in your .gitignore section in .sync.yml.

**`moduleroot/circle.yml`**

The circle.yml file is itself a YAML file with values defined in the YAML files
config_defaults.yml and .sync.yml. Since the circle.yml in the modules are very different, it is updated just based on the current git branch. The config_defaults.yml could be left empty. 

**`moduleroot/project/build.properties`**

Contains the build properties required for a module.

You can add additional environments for a specific module to test by adding an
extras: section with the same format to the module's .sync.yml.

Special Options
---------------

### Unmanaged Files

A file can be marked "unmanaged" in .sync.yml, in which case modulesync will
not try to modify it. This is useful if, for example, the module has special
Rake tasks in the Rakefile which is difficult to manage through a template.

To mark a file "unmanaged", list it in .sync.yml with the value `unmanaged:
true`. For example,

```
---
moduleroot/circle.yml:
  unmanaged: true
```

### Deleted Files

Managing files may mean removing files. You can ensure a file is absent by
marking it "delete". This is useful for purging nodesets.

To mark a file deleted, list it in .sync.yml with the value `delete: true`. For
example,

```
---
moduleroot/project/build.properties
  delete: true
```
