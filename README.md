# GregTech Modern Horizons
This is a reiminagition of the old modpack, GregTech: New Horizons, for minecraft 1.20.1. I made this in response to some issues I had with GTNH, such as the magic mods and boring end game. The difficulty of this pack is planned to be very similar to GTNH, but with actually difficult processes rather than tediousness/repetitiveness.I made this pack in response to a couple issues I had with the original GTNH, such as repetitive processing during the end-game and poorly-integrated magic mods. The progress in my pack starts off difficult, but not as bad as GTNH, the true difficultly begins later down the line, in which I want to focus more on actual difficulty rather than repetitive and tedious processes. I've been working on this pack for about 4 months now and I've made a pretty good amount of progress. It's just been me developing the pack, with some help of others of course. The pack is in a pre-pre-pre-alpha state, as I'm taking it slow and playtesting it to insure that it is balanced and user-friendly.I've removed some key-mods from GTNH, such as the magic mods and TCon. I originally was going to integrate TCon into the progression line, but I replaced it with the auto-generated tool system from GtceuM. In my opinion, this increases difficulty for it forces players to create new tools instead of upgrading/repiaring their existing tools infinitly.I'm trying to make the quest book as good as I possibly can, similarly to GTNH where the quest book helps the players understand what they have to do without much confusion on what to do next. I've yet to actually post my pack on curseforge or modrinth, which is why I'm posting this in screenshots instead of pack-advertisement. I'll post updates here, along with what I already have.

NOTE:
This is a REIMAGINATION OF GTNH, NOT a port for modern versions(1.20.1)




# QSMC Template Modpack

This repository contains the source for a Minecraft 1.20.1 modpack.  It uses
metadata files to describe which mods belong in the pack and a small Python
script to download them and bundle everything into a ZIP.  The workflow will
build the pack for you automatically on GitHub.



## Directory layout

```
.
├── .github/workflows/build.yml       # GitHub Actions workflow
├── README.md
├── compile.py                       # build script (packages mods/config)
├── update.py                        # downloads mods based on metadata
├── metadata/                        # TOML index files for each mod
│   └── mods/
├── minecraft/                       # local launcher instance (ignored)
├── mods/                            # optional alternate mod folder
├── build/                           # output from `compile.py`
└── mmc-pack.json                    # MultiMC/Prism instance descriptor
```

## Usage

1. **Maintain a mod list, not jar files.**

   Keep one TOML file per mod under `metadata/mods/`.  Each file looks like:

   ```toml
   name = "Example Mod"
   filename = "example-mod-1.0.jar"

   [update.curseforge]
   file-id = 1234567
   ```

   or use `update.modrinth.url = "..."` for Modrinth packages.  The
   `update.py` script reads these files and downloads missing jars into
   `minecraft/mods/` before building.

2. **Populate mod files in the instance.**

   You can either let `update.py` fetch mods automatically (see above) or
   manually drop jar files into `minecraft/mods/`.  The builder will package
   whatever is present regardless of metadata.

3. **Local build**

   ```powershell
   py update.py      # optional, downloads jars
   py compile.py     # creates build/modpack-latest.zip (contains manifests)
   ```

   The `compile.py` script is now robust to being invoked from any subdirectory.
   It always uses the directory where the script lives as the base path, so you
   don't have to `cd` back to the repo root before running it – for example you
   can execute `python minecraft\compile.py` if you happen to be in the
   launcher instance.  Regardless, the resulting ZIP will have the correct
   layout.
   The builder will automatically generate both an `mmc-pack.json` and a
   minimal `instance.cfg` (if you don't already have one) so that Prism,
   MultiMC and other launchers can recognise the archive.  The pack now leaves
   *everything except those manifests* under a `minecraft/` prefix, mirroring
   the structure of a normal Prism instance.  That ensures that when you
   import the ZIP the `mods` and `config` folders end up inside the
   `minecraft` directory rather than sitting beside it.

   Inspect the ZIP to make sure it contains `mmc-pack.json` and `instance.cfg`
   at the root and that the remainder of the contents are in `minecraft/` –
   e.g.: 

   ```
   mmc-pack.json
   instance.cfg
   minecraft/mods/…
   minecraft/config/…
   minecraft/resourcepacks/…
   ```

4. **Commit & push**

   ```powershell
   git add metadata/mods/*.toml compile.py update.py .github/workflows/bui
ld.yml
   git commit -m "update mods"
   git push
   ```

5. **Continuous integration**

   The workflow triggers on every `push` to `main` and can also be run
   manually.  It runs `update.py` followed by `compile.py`, then uploads the
   resulting ZIP as an artifact.  

   **Important:** GitHub's artifact service always bundles whatever you upload
   into a second ZIP file before you download it.  That means when you click
   **Artifacts → modpack** you receive a container archive that itself
   contains `modpack-latest.zip`.  You must unzip twice (or extract the
   inner file) before handing the result to your launcher.  This double‑zipping
   is a platform limitation and cannot be avoided.  That is why we also copy
   the ZIP to the GitHub release in the next step – release assets are served
   directly and are ready to import without any extra extraction.

   To avoid confusion, it’s usually easiest to download the pack from the
   **GitHub release** associated with the most recent tag; the workflow
   attaches `build/modpack-latest.zip` as an asset, and that file is
   importable straight away.
```bash
git tag v1.2.3
git push origin v1.2.3
```

When the workflow runs it will not only create the release, it’ll also attach
`build/modpack-latest.zip` as an asset.  If you push the same tag again the
`overwrite: true` option tells the action to update the existing release
instead of erroring with "already_exists".

Example snippet in your workflow:

```yaml
- name: Create GitHub release
  if: startsWith(github.ref, 'refs/tags/')
  uses: ncipollo/release-action@v1
  with:
    tag: ${{ github.ref_name }}       # drop the refs/ prefix
    name: ${{ github.ref_name }}
    token: ${{ secrets.GH_PAT }}   # <--- supply PAT here
    assets: build/modpack-latest.zip
    overwrite: true                # update the release if it already exists
```


This step is optional; you can simply download the ZIP artifact instead of
using releases.

---

See the [Dimension‑Gateway repo](https://github.com/TeamDimensional/Dimension-Gateway)

I looked at it for information. It's quite nice.


