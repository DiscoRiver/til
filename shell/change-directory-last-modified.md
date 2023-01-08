Sometimes I find myself needing to change directory to what was last modified, and I'm too lazy to remember the name of that directory.

Typically I'll create this as an alias in my bash profile;

```
cd  "$(\ls -1dt ./*/ | head -n 1)"
```

Arguments;

```
-d, --directory
              list directories themselves, not their contents
-t     sort by time, newest first; see --time
-1     list one file per line
```