# dayflow-screenshots

Auto-rename and index screenshots with a vision model.

Takes the `screenshot-*.png` files a Wayland compositor drops in `~/Pictures`
(e.g. via `grim`/`omarchy-capture-screenshot`), captions each one with a vision
model, and moves them into `~/Pictures/Screenshots/` as
`YYYY-MM-DD_HH-MM-SS_<snake_case_caption>.png` — plus a markdown `index.md`
catalog of everything processed.

Before:

```
~/Pictures/screenshot-2026-09-05_01-55-46.png
```

After:

```
~/Pictures/Screenshots/2026-09-05_01-55-46_dayflow_standup_log.png
~/Pictures/Screenshots/index.md
```

## How it works

- One OpenRouter vision request per image (`detail: low`, ~30 output tokens)
  with retries; falls back to `screenshot` if the caption is unusable.
- Reads the API key and model from your existing
  `~/.config/dayflow/config.json` (`openrouter_api_key`, `model`), falling back
  to the `OPENROUTER_API_KEY` env var.
- Default model: `google/gemma-4-31b-it`. Any OpenRouter vision model works.

## Requirements

- Python 3 with `Pillow` and `requests` (`pip install pillow requests`)
- An OpenRouter API key
- A [dayflow](https://github.com/duketopceo/dayflow-linux) config, or set
  `OPENROUTER_API_KEY` and edit `CONFIG`/`model` for standalone use

## Install

```sh
install -Dm755 dayflow-screenshots ~/.local/bin/dayflow-screenshots
dayflow-screenshots
```

## Usage

```sh
dayflow-screenshots   # processes all ~/Pictures/screenshot-*.png
```

Tip: wrap your screenshot hotkey so every capture gets organized
automatically — call `dayflow-screenshots` in the background after your
screenshot tool saves the file.

## Privacy

Screenshots are resized to 640px JPEG thumbnails before being sent to
OpenRouter for captioning. The renamed files and index stay local in
`~/Pictures/Screenshots/`.

MIT licensed.
