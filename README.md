### Github Action to automatically pack a darktide mod and publish to nexus mods.

Your mod files must be in the root of your repository, the name of the nesting folder and the zip file will be based on the file name of your `.mod` file.

## Inputs
| **Name** | **Type** | **Required?** | **Details** |
|---|---|---|---|
| nexusmods_mod_id | string | true | ID for your mod entry in NExusMods, this is the number at the end when viewing your mod page. |
| mod_version | string | false | The version name to be used when the mod is uploaded. |
| mod_description | string | false | The changelog to be used when the mod is uploaded. |
| checkout_ref | string | true | Checkout reference, can be tag name or commit hash. |                                       |

Secrets for `NEXUSMODS_APIKEY` and `NEXUSMODS_SESSION_COOKIE` may be acquired according to instructions [here](https://butr.github.io/documentation/advanced/publishing-on-github/#nexusmods)

## Example
This will automatically publish your mod with version name equal to your release name and changelog equal to your release body.
```
name: upload-nexus-mods

on:
  release:
    types: [ "published" ]


jobs:
  fetch-upload:
    uses: bountygiver/DarktideModAutoUploader/.github/workflows/pack_mod.yml@v3
    with:
      nexusmods_mod_id: <replace with your nexus mod ID>
      mod_version: ${{ github.event.release.name }}
      mod_description: ${{ github.event.release.body }}
      checkout_ref: ${{ github.event.release.tag_name }}
    secrets:
      NEXUSMODS_APIKEY: ${{ secrets.NEXUSMODS_APIKEY }}
      NEXUSMODS_SESSION_COOKIE: ${{ secrets.NEXUSMODS_SESSION_COOKIE }}
```