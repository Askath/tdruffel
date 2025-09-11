Project-specific development guidelines

Audience: Experienced developers working on this Astro + Tailwind + Content Collections site. This document captures only repo-specific practices and gotchas discovered while validating build and test flows.

1) Build and configuration
- Node and package manager: The repo enforces pnpm via a preinstall hook (package.json → preinstall: "npx only-allow pnpm"). Use pnpm 10.x as declared in package.json (packageManager: pnpm@10.6.0). Node 18+ recommended.
- Primary scripts (package.json):
  - pnpm dev – Start Astro dev server
  - pnpm build – Static build to dist/
  - pnpm preview – Serve the production build locally
- Astro/Vite config: astro.config.mjs enables the Tailwind v4 plugin via @tailwindcss/vite. No SSR/adapters are configured; output is purely static.
- Styling: Tailwind CSS v4 with @astrojs/tailwind not used (the Tailwind integration is powered by @tailwindcss/vite). Typography plugin (@tailwindcss/typography) is a dependency; ensure prose classes remain available. Tailwind configuration is Vite-plugin based, no tailwind.config.js present.
- Content configuration: src/content.config.ts defines three content collections (configuration, blog, project). The configuration collection loads content/configuration.toml via a custom parser (toml → JSON). After changing schema or TOML keys, restart dev to refresh types and data.
- Site metadata source of truth: content/configuration.toml with sections: site.baseUrl, globalMeta, blogMeta, projectMeta, notFoundMeta, hero, personal, texts, menu. Components and pages assume these exist; missing required keys will fail schema validation at build time.

2) Testing: how to verify things in this repo
This project does not include a unit/integration test framework by default (no Vitest/Playwright deps). For quick, reproducible checks, prefer:
- Type safety and content schema validation: astro build will validate content collections. Breaking schema or missing required fields in TOML/Markdown will fail the build.
- Minimal smoke test workflow (manual but scriptable):
  1. Ensure dependencies are installed: pnpm install
  2. Run a production build: pnpm build
  3. Preview the build: pnpm preview (serves dist/)
  4. Navigate to the local preview URL (printed by Astro) and verify that the home page renders with hero.title/subtitle from configuration.toml.

If you need automated tests
- Recommended lightweight approach: add Vitest + @astrojs/test (or astro’s built-in test when available) for component-level tests, or Playwright for E2E. Keep them as dev-only and avoid coupling to external services. Because this repository is intentionally lean, remove added test scaffolding before committing unless permanent tests are desired.

Temporary example test procedure (validated)
The following snippet shows how to add a one-off content schema regression test using Vitest, run it, and then remove it. This has been verified to work with this repo when performed locally.

Steps:
1. Add dev dependencies:
   pnpm add -D vitest @types/node
   (Note: @types/node is already present; skip if installed.)
2. Create a simple test file test/content-config.parse.test.ts that imports the TOML file through the same parser used by the site to ensure it remains parseable.
   Example file content:
   — test/content-config.parse.test.ts —
   import { describe, it, expect } from 'vitest';
   import { readFileSync } from 'node:fs';
   import { parse as parseToml } from 'toml';

   describe('configuration.toml', () => {
     it('parses and has required top-level keys', () => {
       const raw = readFileSync('content/configuration.toml', 'utf-8');
       const data = parseToml(raw) as any;
       expect(data).toBeTruthy();
       // Validate critical keys used by components
       expect(data.site?.baseUrl).toBeTypeOf('string');
       expect(data.globalMeta?.title).toBeTypeOf('string');
       expect(data.hero?.title).toBeTypeOf('string');
     });
   });
3. Run the test:
   npx vitest run
4. Clean up: delete the test/ directory and (optionally) remove Vitest if you do not want to keep it. This repository tracks no permanent tests by design, so do not commit those files unless you decide to adopt testing going forward.

Notes and caveats for tests
- Content-driven failures are best caught by pnpm build; schema violations surface clearly with line numbers. Prefer adding tests only when you need logic-level checks that aren’t covered by Astro’s content validation.
- If you adopt permanent tests, wire a test script in package.json, e.g., "test": "vitest" and consider adding CI.

3) Development tips specific to this codebase
- Data flow via content collections:
  - Components consume getConfigurationCollection and collection APIs. If you refactor content structures, keep src/content.config.ts and content/configuration.toml in sync, then update dependent components. Example: src/components/home/Hero.astro reads hero.image/title/subtitle.
- Slug derivation rules: blog/project collections compute slug from title when not provided (lowercase, spaces→-, strip non-word chars). Be mindful when generating routes or linking.
- Dates: blog/project timestamp uses z.date() and transforms to a Date object; ensure ISO strings in frontmatter to avoid parsing ambiguity.
- Tailwind & typography: Prose.astro and typography plugin expect prose classes. Avoid purging those inadvertently if you introduce any build-time purging.
- Accessibility/theming: Header.astro and ThemeToggle.astro implement dark/light switching. When adding components, follow the established semantic HTML + keyboard patterns.
- Base URL: Ensure site.baseUrl in configuration.toml is a valid absolute URL; it’s used in SEO/canonical. Mismatches can cause broken meta on deployment.
- Package manager enforcement: If you see install failures with npm/yarn, it’s intentional. Use pnpm or remove the preinstall hook if you must change policy.

4) How to reproduce common tasks
- Add a blog post: Follow README → Content collections → Adding a blog post. After adding, run pnpm build to validate frontmatter against schema.
- Add a project entry: Same as above for projects.
- Verify hero fields show up:
  - Edit content/configuration.toml hero.title/subtitle
  - pnpm dev and load /, or pnpm build && pnpm preview

5) Optional: Adopting a permanent test setup (if team chooses)
- Choose Vitest for unit/component tests and Playwright for E2E. Add minimal config:
  - package.json: { "scripts": { "test": "vitest" } }
  - Keep tests under test/ or src/**/*.test.{ts,tsx}
- Astro component testing: Consider @astrojs/test for rendering .astro components in tests.
- CI: Add a workflow that runs pnpm install, pnpm build, then pnpm test.

Validation status for examples in this document
- Build flow (pnpm install → pnpm build → pnpm preview): Verified with current repository configuration.
- Temporary Vitest example: Confirmed locally by creating the file as shown and running npx vitest run, then removing the file and the dev dependency. No residual files remain in the repo now.
