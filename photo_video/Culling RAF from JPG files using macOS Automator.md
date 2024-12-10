---
created: 2024-12-09T22:05
updated: 2024-12-09T22:08
---
Open Automator.

- Create a new document of type **Quick Action**
- Set the workflow to "Workflow receives current **folders** in **Finder**"

- Add the action **Run Shell Script**
- Set the Shell to /bin/bash
- Set Pass input: to as arguments

Use the following script:

```
#!/bin/bash

# Get the input folder from Automator
sourceFolder="$1"

# Define the path for the "Culled" folder
culledFolder="$sourceFolder/Culled"

# Check if the "Culled" folder exists, if not, create it
if [ ! -d "$culledFolder" ]; then
    mkdir "$culledFolder"
fi

# Loop through all .RAF files in the source folder
find "$sourceFolder" -type f -name "*.RAF" | while read rafFile; do
    # Get the base name without the extension
    baseName=$(basename "$rafFile" .RAF)
    
    # Define the corresponding .jpg file
    jpgFile="$sourceFolder/$baseName.jpg"
    
    # Check if the corresponding .jpg file does not exist
    if [ ! -f "$jpgFile" ]; then
        # Move the .RAF file to the "Culled" folder
        mv "$rafFile" "$culledFolder"
    fi
done
```