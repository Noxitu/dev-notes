
To download entire page recursively you can use:

```
    wget -r -np -nH --cut-dirs=0 --no-check-certificate <ULR>
```

If page is a directory listing, e.g. from `python -m http.server` it is good to exclude listings themselves and download only files:

```
wget -r -np -nH -R "index.html*" --cut-dirs=0 --no-check-certificate <ULR>
```
