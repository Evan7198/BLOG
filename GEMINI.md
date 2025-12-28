# Gemini Context: Tech Blog
## Must reply Chinese
## Project Overview
This project is a technical blog built using the **Astro** web framework. It is based on the official Astro Blog Starter Kit and utilizes **MDX** for content authoring. The project focuses on high performance, SEO friendliness, and a clean, minimal design.

## Tech Stack
*   **Framework:** Astro (v5.x)
*   **Language:** TypeScript
*   **Content:** Markdown (`.md`) and MDX (`.mdx`)
*   **Styling:** Global CSS (`src/styles/global.css`)
*   **Integrations:**
    *   `@astrojs/mdx`: For component support in Markdown.
    *   `@astrojs/sitemap`: For automatic sitemap generation.
    *   `@astrojs/rss`: For RSS feed generation.

## Development Commands

All commands are run from the project root using `npm`:

| Command | Description |
| :--- | :--- |
| `npm run dev` | Starts the local development server at `http://localhost:4321`. |
| `npm run build` | Builds the production site to the `./dist/` directory. |
| `npm run preview` | Previews the production build locally. |
| `npm run astro -- [command]` | Runs Astro CLI commands (e.g., `astro add`, `astro check`). |

## Project Structure & Architecture

### Key Directories
*   `src/content/blog/`: **Content Collections**. Contains all blog post source files (Markdown/MDX).
*   `src/pages/`: **Routing**.
    *   `index.astro`: The homepage.
    *   `blog/[...slug].astro`: Dynamic route handling individual blog post rendering.
    *   `rss.xml.js`: Generates the RSS feed.
*   `src/components/`: **UI Components**. Reusable Astro components (Header, Footer, Meta tags).
*   `src/layouts/`: **Layouts**. Wrapper components defining page structures (e.g., `BlogPost.astro`).
*   `public/`: **Static Assets**. Images, fonts, and favicon served at the root.

### Configuration
*   `astro.config.mjs`: Main Astro configuration. Currently sets up:
    *   `site`: 'https://example.com' (Needs update for production).
    *   Integrations: MDX, Sitemap.
*   `src/consts.ts`: Global application constants.
    *   `SITE_TITLE`: Configures the global site title.
    *   `SITE_DESCRIPTION`: Configures the global site description.

### Content Management
Blog posts are managed via **Astro Content Collections**.
1.  Create a new file in `src/content/blog/`.
2.  Use frontmatter to define metadata (title, description, pubDate, heroImage).
3.  The schema is likely defined in `src/content.config.ts`.
