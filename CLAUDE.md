# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repo is Dominik Harmim's personal CV/portfolio website: a single static HTML page deployed to
`www.harmim.cz` via Apache. There is no build system, package manager, or test suite — the entire
site is [index.html](index.html), a self-contained document with inline `<style>` and `<script>`
blocks (no external JS/CSS files, no bundler, no framework).

## Development

There are no build/lint/test commands — edit `index.html` directly and open it in a browser to
preview. Since there's no local server requirement, opening the file directly (`file://`) works
for most checks, but do a real HTTP preview (e.g. `python3 -m http.server`) before relying on
anything that behaves differently over `file://`.

## Architecture

`index.html` is organized top-to-bottom as:

- **`<head>`**: meta/SEO tags, Open Graph tags, a data-URI SVG favicon, and Google Fonts
  (`DM Serif Display`, `DM Sans`, `JetBrains Mono`).
- **Inline `<style>` block**: CSS custom properties under `:root` (`--bg`, `--accent`,
  `--font-*`, `--radius-*`, `--transition`, etc.) act as the design tokens — reuse these instead
  of hardcoding colors/fonts/spacing when editing styles. Sections are marked with banner comments
  (`DESIGN TOKENS`, `RESET & BASE`, `LAYOUT`, etc.).
- **`<body>`**: a fixed nav bar, then page content as `<section id="...">` elements in reading
  order: `hero`, `about`, `experience`, `education`, `publications`, `skills`, `awards`,
  `certifications`, followed by a `<footer id="contact">`.
- **Inline `<script>` block** at the end of the body (vanilla JS, no dependencies): sets the
  footer year, mobile nav toggle, scroll-triggered `.reveal` animations via `IntersectionObserver`,
  active-nav-link highlighting based on section visibility, and the "back to top" button.

## Deployment / server config

[.htaccess](.htaccess) controls the Apache-hosted production site: forces HTTPS, redirects the
bare domain (`harmim.cz`) to `www.harmim.cz`, strips trailing slashes, disables directory listing,
blocks access to dotfiles, and enables gzip compression. Changes to routing/security behavior for
the live site belong here, not in application code.

## Content edits

Most changes to this repo are content edits (job history, skills, publications, certifications)
directly inside the relevant `<section>` in `index.html`, keeping existing markup/class structure
and design tokens consistent.
