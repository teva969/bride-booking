# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file static bridal photography booking website (`index.html`). All HTML, CSS, and JavaScript are inline in that one file — there is no build system, no dependencies, and no separate asset files.

## Running Locally

Because the booking form uses `fetch()` to POST to formsubmit.co, it must be served over HTTP (not opened as a `file://` URL, which browsers block for CORS reasons):

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Architecture

`index.html` is structured in document order:

1. **`<style>` block** — all CSS, using CSS custom properties (`--blush`, `--rose`, `--dark-rose`, `--gold`, `--cream`) for the blush-pink + gold palette. Responsive breakpoints at 768px.
2. **`<body>`** — six sections in order: `nav`, `#hero`, `#about`, `#packages`, `#gallery`, `#booking`, `#contact`, `footer`.
3. **`<script>` block** — two behaviours:
   - Form submit handler on `#bookingForm`: collects field values, POSTs JSON to `https://formsubmit.co/ajax/qingru@gmail.com`, shows `#successMessage` on success.
   - Smooth-scroll handler on all `nav a` links.

## Form Submission

- Service: [formsubmit.co](https://formsubmit.co) AJAX endpoint (no backend required).
- The recipient email is hardcoded in the fetch URL. Change `qingru@gmail.com` in the `<script>` block to update it.
- formsubmit.co sends an activation email to the recipient on the very first submission — that link must be clicked before real submissions are delivered.

## Images

All images are Unsplash URLs with `?w=&h=&fit=crop` query params for sizing. Swap URLs directly in the HTML to change photos.
