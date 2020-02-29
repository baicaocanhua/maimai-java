```
{
    "cmd": ["groovy","$file"],
    "selector": "source.groovy",
    "file_regex": "[ ]*at .+[(](.+):([0-9]+)[)]",

    "windows": {
        "shell": "cmd.exe"
    }

}
```

```
{
  "cmd":["python","-u","$file"]
}
```

