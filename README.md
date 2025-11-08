## This file is generated from the template [.github/tpl/README.tpl.md](.github/tpl/README.tpl.md)
## Don't modify this file, all the changes will be lost. 
## please make changes to the template file instead.


# render
Render templates The latest release is: v0.0.2

Non defined variables are kept as-is: 

this is a TTTEST .

OK, this IS ANOTHER 

```
#this is code:



int main() {

 //just some code.
}



${{no}}
```

Test

Example usage:

```
name: "Update Readme"
on:
  workflow_dispatch:
  push:
    branches:
      - '*'
    paths:
      - '.github/tpl/*'
      - '.github/data/*'
      - '.github/workflows/readme.yml'

  release:
    types: [ published ]
    
    
jobs:
  readme:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Get latest release
        id: get-latest-release
        uses: InsonusK/get-latest-release@v1.0.1
        with:
          myToken: ${{ github.token }}
          view_top: 1

      - name: Using main branch
        run: git switch main || (git fetch --all && git checkout -b main origin/main)

      - name: Update the readme.md
        uses: vmactions/render@main
        env:
          TAG_NAME: ${{ steps.get-latest-release.outputs.tag_name }}
        with:
          datafile: .github/data/datafile.ini
          files: |
            .github/tpl/README.tpl.md : README.md

      - uses: EndBug/add-and-commit@v9
        with:
          message: "Update version to ${{ steps.get-latest-release.outputs.tag_name }}"
          pull: '--rebase --autostash '
          add: |
            README.md



```

