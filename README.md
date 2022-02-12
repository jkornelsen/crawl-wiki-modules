# crawl-wiki-modules
C++ tool and Lua modules for generating Crawl Wiki content (http://crawl.chaosforge.org).

This is an update of the project at https://github.com/plampila/crawl-wiki-modules.

## Instructions
* Follow https://github.com/crawl/crawl/crawl-ref/INSTALL.md to clone and build crawl. I found the build process to go smoothly with Ubuntu on WSL.
* Create a directory `crawl-ref/source/util/json-data/` and put `json-data-main.cc` and `json-data-main.h` in it.
* Modify `source/Makefile` and perhaps `Makefile.obj` to build the added code. It is similar to the `monster` utility, so duplicate the Makefile lines for that utility and change them for `json-data`. Look at the `Makefile` on this site to compare.
* Enter commands to build and run.

```sh
make monster
make json-data
util/json-data/json-data > data.json
```
* If the code is out of date, make changes to `json-data-main.cc`. I found that many changes were needed. Look for similar code in various parts of the crawl source to see how to write the updates, starting with `monster-main.cc`.
* Download `json_to_table.lua`. I used Windows PowerShell for this part, which worked well although the syntax is ugly (seen below). Lua binaries are at http://luabinaries.sourceforge.net/download.html.
* This module requires serializer and json packages. I used https://github.com/pkulchenko/serpent and https://github.com/rxi/json.lua. They did not handle Unicode, but other than that they worked. And the only Unicode ended up being EM dashes which became `???` at the end. A quick search and replace fixed this.
* Run the Lua code.

```sh
Get-Content data.json | & "C:\Program Files\lua\lua54.exe" json_to_table.lua spells > Table_of_spells.lua
Get-Content data.json | & "C:\Program Files\lua\lua54.exe" json_to_table.lua spellbooks > Table_of_spellbooks.lua
```

* When the files look ready, copy to [http://crawl.chaosforge.org/Special:PrefixIndex/Module:](http://crawl.chaosforge.org/Special:PrefixIndex/Module:) and then check dependent pages to see if it works.
