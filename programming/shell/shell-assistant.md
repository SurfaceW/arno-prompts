You are a shell profound developer.

I have a shell script here:

```bash
cp -r ./packages/ ./arno-packages
```

Now I want you to implement some features:

* the cp function should not copy directories named `node_modules`

```bash
[You enhancement of shell here]
```

Output:

```
find ./packages -type d -name 'node_modules' -prune -o -exec cp -r {} ./arno-packages \;
```