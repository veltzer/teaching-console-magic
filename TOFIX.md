# TOFIX

Findings from a code scan on 2026-10-04.

## High

- `README.md:8` - `sudo apt install youtube-dl` no longer works (the package has no install candidate on current Ubuntu) and `video_download.sh:2` calls `youtube-dl`, which is unmaintained and broken against YouTube. Switch both to `yt-dlp` (`yt-dlp ... -o video.webm`).

## Medium

- `README.md:16` - the image demo requires `npm` and a global `npm install -g console-png` (`image_show.sh:2`, `links.txt:1`), JS tooling in a repo whose subject is not JS. Replace it with a native terminal image viewer available from apt (e.g. `chafa image.png`) and update the README steps and `links.txt`.
- The repo has no `rsconstruct.toml` and no `.github/workflows/build.yml`, so the four shell scripts are never shellchecked and `README.md` is never linted. Add the standard fleet build: an `rsconstruct.toml` with `[processor.shellcheck]` and `[processor.rumdl]` (precise `src_files`/`src_dirs`, not `"."`) plus the shared `build.yml`.
- `image_download.sh:2` and `video_download.sh:2` write `image.png` and `video.webm` into the repo root, where they show up as untracked files. Download into the already-gitignored `download/` directory and have `image_show.sh:2` / `video_play.sh:2` read from there.

## Low

- `README.md:1` - the README is plain text with setext headings and tab-indented commands, not the fleet's generated README; convert it to markdown (fenced code blocks) or to the standard `tera.templates/README.md.tera` with the demo instructions in `tera.snippets/main.md.tera`.
- `config/project.lua:3` - typo in `DESCRIPTION_SHORT`: "Various nice thing on the console" should be "things".
