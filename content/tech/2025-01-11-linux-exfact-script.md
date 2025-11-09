---
title: "Fixing Linux Backup Sync Issues for exFAT Compatibility"
date:  "2025-01-11T11:38:55+09:00"
tags:
    - Linux
    - Scripts
---


A recent issue with synchronizing my home directory to my exFAT-formatted external SSD surfaced while using KDE’s Kup Backup application. Errors occurred because some filenames contained characters not supported by the exFAT file system.

**Understanding exFAT's Filename Restrictions**

Many external drives use exFAT for cross-platform compatibility. However, exFAT prohibits certain characters in filenames, which can cause issues when backing up files from Linux filesystems.

Filenames [can include all Unicode characters except](https://en.wikipedia.org/wiki/ExFAT):

- `"` (double quote)
- `*` (asterisk)
- `:` (colon)
- `<` (less than)
- `>` (greater than)
- `?` (question mark)
- `\` (backslash)
- `/` (forward slash)
- `|` (pipe)

Filenames also cannot end with a space or a period, but these cases are not currently handled by the script below.

**Identifying Problematic Files**

Kup Backup logs errors related to such files in `~/.cache/kup/kup_plan1.log`. Reviewing this log allowed me to isolate the problematic files. If only a few files are affected, the simple solution is to manually rename them, removing or replacing any unsupported characters.

**Automating the Renaming Process with a Bash Script**

For a large number of files, the renaming process can be automated. The following Bash script (corrected from an initial version created with ChatGPT) identifies and renames files in given directories that contain invalid characters, replacing each with an underscore.

```bash
#!/bin/bash

# Directories to scan
directories=(
    "/path/to/your/directory1"
    "/path/to/your/directory2"
)

# Array of characters to replace
declare -A char_map=(
    ['"']='_'
    ['*']='_'
    [':']='_'
    ['<']='_'
    ['>']='_'
    ['?']='_'
    ['\\']='_'
    ['/']='_'
    ['|']='_'
)

# Function to rename files
rename_files() {
    local dir="$1"
    # Use find to search for files in "$dir" with names containing restricted characters, 
    # then pass each result to the while loop for processing.
    find "$dir" -depth -name '*["*:<>?\\|]*' | while IFS= read -r file; do
        # Get the directory and base name of the file
        dir_path=$(dirname "$file")
        base_name=$(basename "$file")
        
        # Rename the file
        new_name="$base_name"
        for char in "${!char_map[@]}"; do
            new_name="${new_name//"$char"/${char_map[$char]}}"
        done

        # If the filename has changed, rename it
        if [[ "$base_name" != "$new_name" ]]; then
            mv -v -n "$file" "$dir_path/$new_name"
        fi
    done
}

# Iterate over directories and rename files
for dir in "${directories[@]}"; do
    if [[ -d "$dir" ]]; then
        rename_files "$dir"
    else
        echo "Directory $dir does not exist."
    fi
done
```

**How the Script Works**

The script iterates over each specified directory, checks if it exists, and calls the `rename_files` function.

The `rename_files` function uses `find` to locate files with restricted characters in their names. It then iteratively replaces each restricted character with an underscore and renames the file using `mv`, with verbose flag to explain what is being done. The `-n` flag makes sure it will not overwrite an existing file.

**Usage Instructions**

I’ve tested the script, but remember to take precautions to prevent data loss when running it.

After running the process, verify that the files were renamed correctly. Manual post-processing is needed for files that would have caused duplicate names (these files are skipped).

If you encounter any issues or have suggestions for improvements, feel free to reach out and let me know.  