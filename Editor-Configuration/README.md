# Plugins for various editors
## Atom
```
atom-ide-base
auto-indent
busy-signal
cert-tweaks
compare-files
copy-find-results
docblockr
file-types
highlight-bad-chars
highlight-selected
ide-python
indent-detective
indent-guide-improved
intentions
json-converter
language-docker
language-gradle
language-groovy
language-powershell
linter
linter-js-yaml
linter-ui-default
merge-conflicts
minimap
minimap-find-and-replace
minimap-git-diff
minimap-highlight-selected
minimap-linter
minimap-split-diff
multiline-tab
pretty-json
semanticolor
split-diff
sort-lines
tabs-to-spaces
```
```
Other Atom
If you have a Bluecoat or private CA:
NODE_EXTRA_CA_CERTS=<path>
Then install cert-tweaks package
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
VSCode doesn't have good semantic highlighting support.
In other words it does not highlight variables is different colors, but rather highlights language structure.
Could not find Minimap modules - something is built in ("editor.minimap.enabled": true)
Could not find Dockblockr (ctrl+/ maybe)
```
