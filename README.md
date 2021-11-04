![zew logo](https://raw.githubusercontent.com/z-shell/zplugin-legacy/main/doc/img/zplugin.png)

# Zplugin

Zplugin gives **reports** from plugin load. Plugins are no longer black boxes,
report will tell what aliases, functions, bindkeys, Zle widgets, zstyles,
completions, variables, `PATH` and `FPATH` elements a plugin has set up. Supported is
**unloading** of plugin and ability to list, uninstall, reinstall and selectively
disable, enable plugin's completions. Also, every plugin is compiled and user
can control this function. The system does not use `$FPATH`, it's kept clean!

Code is immune to `KSH_ARRAYS`, `emulate sh`, `emulate ksh`, thoroughly tested to
support any user setup, be as transparent as plain `source` command. Compdef replays
functionality is provided to allow user call `compinit` only once in `.zshrc`.

**Zplugin in action:**

![zplugin](http://imageshack.com/a/img905/5575/n3p47o.gif)

**Completion handling:**

![zplugin](http://imageshack.com/a/img907/2167/CATuag.gif)

**Dtrace:**

![dtrace](http://imageshack.com/a/img924/2539/eCfnUD.gif)

**Code recognition with recently, changes, glance, report, stress:**

![code recognition](http://imageshack.com/a/img923/6404/5mOUl2.gif)

## Introduction

![zplugin-refcard](http://imageshack.com/a/img924/7014/KKkzny.png)

**Example use:**

```
% . ~/github/zplugin/zplugin.zsh
% zplugin load zsh-users/zsh-syntax-highlighting
% zplugin load psprint/zsh-cmd-architect
```

**Example plugin report:**

```
% zpl report psprint/zsh-cmd-architect
Plugin report for psprint/zsh-cmd-architect
-------------------------------------------
Source zsh-cmd-architect.plugin.zsh
Autoload h-list
Autoload h-list-input
Autoload h-list-draw
Autoload zca
Autoload zca-usetty-wrapper
Autoload zca-widget
Zle -N zca-widget
Bindkey ^T zca-widget

Functions created:
h-list             h-list-draw
h-list-input       zca
zca-usetty-wrapper zca-widget

Options changed:
autolist     was unset
menucomplete was unset

PATH elements added:
/Users/sgniazdowski/github/zsh-cmd-architect/bin

FPATH elements added:
/Users/sgniazdowski/github/zsh-cmd-architect

Completions:
_xauth [disabled]
```

![report example](http://imageshack.com/a/img923/4237/OHC0i5.png)

**Example plugin unload:**

```
% zpl unload psprint/zsh-cmd-architect
Deleting function h-list
Deleting function h-list-draw
Deleting function h-list-input
Deleting function zca
Deleting function zca-usetty-wrapper
Deleting function zca-widget
Deleting bindkey ^T zca-widget
Setting option autolist
Setting option menucomplete
Removing PATH element /Users/sgniazdowski/github/zsh-cmd-architect/bin
Removing FPATH element /Users/sgniazdowski/github/zsh-cmd-architect
Unregistering plugin psprint/zsh-cmd-architect
Plugin's report saved to $LASTREPORT
```

![unload example](http://imageshack.com/a/img921/9896/rMMnQ1.png)

**Example `csearch` invocation (completion management):**

```
# zplg csearch
[+] is installed, [-] uninstalled, [+-] partially installed
[+] _local/zplugin                  _zplugin
[-] benclark/parallels-zsh-plugin   _parallels
[+] mollifier/cd-gitroot            _cd-gitroot
[-] or17191/going_places            _favrm, _go
[-] psprint/zsh-cmd-architect       _xauth
[-] psprint/zsh-editing-workbench   _cp
[+] tevren/gitfast-zsh-plugin       _git
```

![csearch example](http://imageshack.com/a/img921/5741/QJaO8q.png)

**Example `compile` invocation:**

```
# zplg compile zsh-users/zsh-syntax-highlighting
Compiling zsh-syntax-highlighting.plugin.zsh...
# zplg compiled
zsh-users/zsh-syntax-highlighting:
zsh-syntax-highlighting.plugin.zsh.zwc
# zplg uncompile zsh-users/zsh-syntax-highlighting
Removing zsh-syntax-highlighting.plugin.zsh.zwc
# zplg compiled
No compiled plugins
# zplg compile-all
zsh-users/zsh-syntax-highlighting
Compiling zsh-syntax-highlighting.plugin.zsh...
```

![compile example](http://imageshack.com/a/img923/6655/gexv8M.png)

**Example `create` invocation:**

```
% zplg create psprint/testplugin
Github user name or just "_local": psprint
Plugin name: testplugin2
Plugin is psprint/testplugin2
Creating Github repository
Enter host password for user 'psprint':
Cloning into 'psprint---testplugin2'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
Remote repository psprint/testplugin2 set up as origin
You're in plugin's local folder
The files aren't added to git
Your next step after commiting will be:
git push -u origin master
% ls
.git                   README.md
LICENSE                testplugin2.plugin.zsh
```

![create example](http://imageshack.com/a/img921/8966/NURP24.png)

## Installation

Execute:

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/z-shell/zplugin-legacy/main/doc/install.sh)"
```

To update run the command again (or just execute `doc/install.sh`).

`Zplugin` will be installed into `~/.zplugin/bin`. `.zshrc` will be updated with
three lines of code that will be added to the bottom (the lines will be sourcing
`zplugin.zsh` and setting up completion).

Completion will be available, for command **zplugin** and aliases **zpl**, **zplg**.

After installing and reloading shell give `Zplugin` a quick try with `zplugin help`.

## Manual installation

To manually install `Zplugin` clone the repo to e.g. `~/.zplugin/bin`:

```sh
mkdir ~/.zplugin
git clone https://github.com/z-shell/zplugin-legacy.git ~/.zplugin/bin
```

and source it from `.zshrc` (**above compinit**):

```sh
source ~/.zplugin/bin/zplugin.zsh
```

If you place the `source` below `compinit`, then add those two lines after the `source`:
```sh
autoload -Uz _zplugin
(( ${+_comps} )) && _comps[zplugin]=_zplugin
```

After installing and reloading shell give `Zplugin` a quick try with `zplugin help`.

### Compilation
It's good to compile `zplugin` into `Zsh` bytecode:

```sh
zcompile ~/.zplugin/bin/zplugin.zsh
```

Zplugin will compile each newly downloaded plugin. You can clear compilation of
a plugin by invoking `zplugin uncompile {plugin-name}`. There are also commands
`compile`, `compile-all`, `uncompile-all`, `compiled` that control the
functionality of compiling plugins.

## Usage

```
% zpl help
Usage:
-h|--help|help           - usage information
man                      - manual
self-update              - updates Zplugin
load {plugin-name}       - load plugin
light {plugin-name}      - light plugin load, without reporting
unload {plugin-name}     - unload plugin
snippet [-f] {url}       - source local or remote file (-f: force - don't use cache)
update {plugin-name}     - update plugin (Git)
update-all               - update all plugins (Git)
status {plugin-name}     - status for plugin (Git)
status-all               - status for all plugins (Git)
report {plugin-name}     - show plugin's report
all-reports              - show all plugin reports
loaded|list [keyword]    - show what plugins are loaded (filter with `keyword')
cd {plugin-name}         - cd into plugin's directory
create {plugin-name}     - create plugin (also together with Github repository)
edit {plugin-name}       - edit plugin's file with $EDITOR
glance {plugin-name}     - look at plugin's source (pygmentize, {,source-}highlight)
stress {plugin-name}     - test plugin for compatibility with set of options
changes {plugin-name}    - view plugin's git log
recently [time-spec]     - show plugins that changed recently, argument is e.g. 1 month 2 days
clist|completions        - list completions in use
cdisable {cname}         - disable completion `cname'
cenable  {cname}         - enable completion `cname'
creinstall {plugin-name} - install completions for plugin
cuninstall {plugin-name} - uninstall completions for plugin
csearch                  - search for available completions from any plugin
compinit                 - refresh installed completions
dtrace|dstart            - start tracking what's going on in session
dstop                    - stop tracking what's going on in session
dunload                  - revert changes recorded between dstart and dstop
dreport                  - report what was going on in session
dclear                   - clear report of what was going on in session
compile  {plugin-name}   - compile plugin
compile-all              - compile all downloaded plugins
uncompile {plugin-name}  - remove compiled version of plugin
uncompile-all            - remove compiled versions of all downloaded plugins
compiled                 - list plugins that are compiled
```

To use themes created for `Oh-My-Zsh` you might want to first source the `git` library there:

```sh
zplugin snippet 'http://github.com/robbyrussell/oh-my-zsh/raw/master/lib/git.zsh'
```

Then you can use the themes either as plugins (`zplugin load {user/theme-name}`) or as snippets
(`zplugin snippet {file path or URL}`; plugin method recommended). Some themes require not only
`Oh-My-Zsh's` `git` library, but also `git` plugin (error about function `current_branch` appears).
Source it as snippet directly from `Oh-My-Zsh`:

```sh
zplugin snippet 'https://github.com/robbyrussell/oh-my-zsh/raw/master/plugins/git/git.plugin.zsh'
```

Such lines should be added to `.zshrc`. Snippets are cached locally, use `-f` option to download
a fresh version of a snippet.

## Clean .zshrc With Compdef Replaying

`Zplugin` provides a feature which brings order into `.zshrc`. When to call `compinit`? Some plugins
require completion being in place, other update `$FPATH` and want `compinit` to be called later.
`Zplugin` is like a pre-initialized completion for the first type of plugins. They will thus work
without real `compinit` in place. You should call `compinit` only once after loading all plugins, in
following way:

```sh
source ~/.zplugin/bin/zplugin.zsh
zplugin load "some/plugin"
...
zplugin load "other/plugin"

autoload -Uz compinit
compinit
zplugin cdreplay -q # -q is for quiet
```

Performance gains are huge, example shell startup time with double `compinit`: **0.980** sec, with
`cdreplay` and single `compinit`: **0.156** sec.

### Ignoring Compdefs

If you want to ignore compdefs provided by some plugins or snippets, place their load commands
before commands loading other plugins or snippets, and issue `zplugin cdclear`:

```sh
source ~/.zplugin/bin/zplugin.zsh
zplugin snippet https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh
zplugin cdclear # <- forget completions provided up to this moment

zplugin load "some/plugin"
...
zplugin load "other/plugin"

autoload -Uz compinit
compinit
zplugin cdreplay -q # <- execute compdefs provided by rest of plugins
zplugin cdlist # look at gathered compdefs
```
