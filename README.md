# EasyXdc Blog

Personal blog built with [Astro](https://astro.build) and the [Fuwari](https://github.com/saicaca/fuwari) template.

## Features

- Built with Astro + Tailwind CSS
- Light / dark mode with customizable theme color
- Responsive design
- Search powered by [Pagefind](https://pagefind.app/)
- Markdown extended features (admonitions, GitHub cards, expressive code blocks)
- Table of contents
- RSS feed

## Local Development

```sh
pnpm install
pnpm dev
```

Dev server runs at `localhost:4321`.

## Create a New Post

```sh
pnpm new-post <filename>
```

Posts go in `src/content/posts/`. Frontmatter:

```yaml
---
title: Post Title
published: 2026-05-15
description: Short description
image: ""
tags: [Tag1, Tag2]
category: Category
draft: false
---
```

## Configuration

Edit `src/config.ts` for site title, profile, navbar, banner, license, etc.

## Build & Deploy

```sh
pnpm build
```

Output goes to `./dist/`. Update `site` in `astro.config.mjs` before deploying.

## Commands

| Command                    | Action                                        |
|:---------------------------|:----------------------------------------------|
| `pnpm install`             | Install dependencies                          |
| `pnpm dev`                 | Start dev server at `localhost:4321`          |
| `pnpm build`               | Build production site to `./dist/`            |
| `pnpm preview`             | Preview build locally                         |
| `pnpm new-post <filename>` | Create a new post                             |

## License

Content is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).
Template (Fuwari) is licensed under the MIT License.
