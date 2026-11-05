# HiDream-I1 UI

A simple standalone Gradio app for **HiDream-I1-Dev** — one model, one
window, everything on the surface. Built for a 12 GB GPU / 32 GB RAM
machine: the transformer loads from a GGUF file, the four text encoders
load in FP8, and CPU offload is always on.

## Install

1. Unzip anywhere.
2. Double-click `install.bat` once. It uses the bundled embedded
   Python — nothing is installed system-wide.
3. Double-click `run.bat`. The app opens in your browser at
   127.0.0.1. The first Generate downloads about 33.5 GB of model
   files into the Hugging Face cache.

Keep the terminal window open while the app runs — closing it
closes the app.

## What you get

- **Variant: Dev.** 28 steps, guidance 0.0, LCM scheduler (shift 6.0).
  Dev is guidance-distilled, so there is no CFG pass: the guidance is
  shown as a fixed value, and the negative-prompt box is kept only as
  part of the prompt record — it has **no effect** on Dev.
- **Precision:** GGUF Q4_K_M by default (about 26.5 GB of RAM parked,
  ~5.5 GB spare on a 32 GB machine). Q5_K_M is on the selector with
  its margin stated. Q6_K is deliberately not offered — it would
  park about 29.7 GB, too tight here.
- **Four text encoders:** CLIP-L, CLIP-G and T5-XXL load FP8-at-load
  from the HiDream repo. The Llama-3.1-8B encoder loads from
  Comfy-Org's ungated FP8 build of the same weights — the HiDream
  repos don't ship it, and the vendor's own loader points at the
  gated meta-llama repo. Using the Comfy-Org file keeps this app
  **token-free**: no Hugging Face token is needed anywhere.
- Prompt queue (one prompt per line), images-per-prompt batch, seed
  control (a batch uses seed, seed+1, ...), the vendor's size
  presets plus custom sizes, history of the last 10, Stop, **Star
  batch** (copies the latest batch into `images_out/favorites/`),
  Unload, and a `settings-*.txt` log written next to every batch.
  Note: HiDream's pipeline normalizes every output to about
  1 megapixel with the aspect ratio preserved (all the vendor
  presets are ~1 MP), so custom width/height effectively choose
  an aspect ratio; the settings log records the size actually
  produced.
- **No LoRA in v1** — GGUF + CPU-offload LoRA loading is unverified
  for this pipeline, so it's left out rather than shipped
  half-working.

## Files

- `images_out/` — generated images, settings logs, `favorites/`.
- `hidream-ui/app.py` — the whole app.

## Licenses

The app code is MIT (see LICENSE). Model weights keep their own
licenses: the HiDream-I1 transformer, VAE and encoders are MIT
(HiDream-ai); the Llama-3.1-8B encoder weights are under the
Llama 3.1 Community License (Meta), redistributed as an FP8 build
by Comfy-Org.

Part of Alan's standalone image-model UI fleet — one model at a
time, a UI designed around it.
