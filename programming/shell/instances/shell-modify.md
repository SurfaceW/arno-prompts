```bash
# Iterate over all files and directories in the current directory
for file in "$work_dir"/*; do
    # Check if the file is not the .git folder
    if [[ "$file" != "$work_dir/.git" ]]; then
        # Remove the file or directory
        #rm -rf "$file"
        echo "$file"
    fi
done
```

consider about the bash script above:

* I want to and one more or condition for `/.git` and `/scripts` folder shall not echo out
* what should I do to make it work?