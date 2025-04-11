# Plugins for various editors

## Zed

Similar to Atom but not as many plugins yet. A lot of configuration via command pallet - including ai assistant.

## Atom

---

### RIP Atom Dec. 15 2022

Basically the same day MS purchased GitHub new commits to Atom fell to almost 0, likely because MS was pushing VSCode.

#### Historical Atom Plugins, which because of core editor internal design decisions, some are still better than what can be found in VSCode.

```
atom-ide-base (Generic IDE framework required for language specific IDEs)
auto-indent (Automatically align code within scope using current tab stop character)
busy-signal (part of linter package)
compare-files (select two files from side bar, and open them for differences)
copy-find-results (copy results from finding accross files)
docblockr (quickly put in skelton of javadoc)
file-types (add file types so that the language does not have to be mannually selected)
highlight-bad-chars (highlight invisible unicode characters, which could cause problems)
highlight-selected (high light all instances of a selected word in a document)
ide-python (a python ide)
indent-detective (automatically determine the type if indentation used in a file)
indent-guide-improved (show vertical lines for scope matching)
intentions (part of the linter package)
json-converter (convert from json to yaml and back)
language-docker
language-gradle
language-groovy
language-powershell
linter (a generic linter framework which linters build upon)
linter-js-yaml
linter-ui-default (part of linter framework)
merge-conflicts (mergetool functionality)
minimap (miniture of entire document on right side)
minimap-find-and-replace (results displayed in minimap)
minimap-git-diff (results displayed in minimap)
minimap-highlight-selected (results displayed in minimap)
minimap-linter (results displayed in minimap)
minimap-split-diff (results displayed in minimap)
multiline-tab (wrap open document tabs, instead of scrolling to the right forever)
pretty-json (minifie <-> prettifie JSON)
semanticolor (make each variable a different color using tree sitter - with a fall back to pre tree sitter)
split-diff (view file differences side by side)
sort-lines
tabs-to-spaces (convert delemiter, for instance auto indent to align and then convert)
```

```
Other Atom
If you have a Bluecoat or private CA:
NODE_EXTRA_CA_CERTS=<path>
Then install cert-tweaks package
cert-tweaks (for using a private CA)
Also:
text-manipulation
font
minimap-autohide
```

### Atom with iPad

```
Atom on an iPad with Citrix with an external bluetooth keyboard (and an Apple Pencil too)
Install atom-touch-events package
On the iPad:
(Pair the keyboard and make sure it is not asleep)

Important: Disable Smart Punctuation (in Settings -> General -> Keyboard) to disable serifed quotes
  Change other settings as desired
Settings -> General -> Keyboard -> Hardware Keyboard (Only appears when keyboard is paired and not asleep)
 Change settings as desired.

Settings -> Accessibility (This contains the various keyboard shortcuts)
Settings -> Accessibility -> Enable Full Keyboard Access (Enabling this intercepts a few more key strokes, but doesn't make a huge difference)

Press Control-Option-Command-P to put the keyboard into pass through mode (Otherwise some keys, like arrow keys, will not work)

The Apple Pencil is very handy for pointing to small menu icons (as well as Scribble).


Citrix seems to intercept a lot of characters:
Control is not correctly intercepted for all keystrokes
^-] and ^-\ act as if a key was not pressed
^ with any numeric key, or ; ' ` / act as if ^ was not pressed (and just shows the key)
That ^-/ does not work is very annoying for block comments, and needs to be remapped.

Within Citrix, Atom seems like ^ by itself is not passed through, but rather passed when a combination of keys has been pressed. Web browser key checking sites via Citrix perform even worse.

```

## VSCode

```
Java (By RedHat)
Python (By Microsoft)
```

```
The default linter pylint only runs on save, and is slow.
pyflakes would be much better but is only in the pylama package which is not enabled by default.
Todo: Find packages for yaml linting, json <-> yaml conversion, tab to space conversion, method to set tab length, multiline tabs, sorting, diffing, finding across files and copying find results.
```

```
Semanticolor like extension for VSCode: Color Identifiers by Mathew Nespor or Semantic Highlighting by Malcommielle. (The logic is fairly simple, but relies on a 3rd party language plugin - for each language - to do the right thing. Thus weird things happen such as the document only tokenizing if it is opened directly, rather than as a part of a project.)
VS DocBlockr
```
