# Contributing to Universitario

Thanks for taking the time to contribute! This is an Obsidian theme built almost entirely with CSS, so most contributions come down to improving the look, fixing rendering bugs, or keeping the docs clear.

## Table of contents

- [Ways to contribute](#ways-to-contribute)
- [Development setup](#development-setup)
- [Project structure](#project-structure)
- [CSS guidelines](#css-guidelines)
- [Testing checklist](#testing-checklist)
- [Branching and commits](#branching-and-commits)
- [Pull requests](#pull-requests)
- [Versioning and releases](#versioning-and-releases)
- [License](#license)

## Ways to contribute

- **Bug reports**: something looks broken or renders incorrectly.
- **Feature ideas**: new callouts, layouts, or theme options.
- **CSS fixes and improvements**: colors, spacing, typography, compatibility with plugins.
- **Documentation**: README, screenshots, examples, or this guide.

For bugs, please include:

1. Obsidian version and platform (Windows, macOS, Linux, mobile).
2. Theme version from `manifest.json`.
3. A screenshot or short recording of the problem.
4. The Markdown or `cssclasses` involved, when relevant.
5. Any community plugins that might be affecting the rendering.

> [!TIP]
> Try to reproduce the issue with the default theme and with all snippets/plugins disabled. That helps separate theme bugs from plugin conflicts.

## Development setup

1. Clone the repository:

   ```bash
   git clone https://github.com/wulflo/obsidian-3Sumaq.git
   cd obsidian-3Sumaq
   ```

2. Create a test vault (or use an existing one).

3. Copy or symlink the theme into the vault's themes folder:

   ```bash
   # macOS / Linux
   ln -s "$(pwd)" "/path/to/your-vault/.obsidian/themes/Universitario"

   # Windows (PowerShell, run as Administrator)
   New-Item -ItemType SymbolicLink -Path "C:\path\to\your-vault\.obsidian\themes\Universitario" -Target "C:\path\to\obsidian-3Sumaq"
   ```

   If symlinks are not an option, copy `manifest.json` and `theme.css` into `.obsidian/themes/Universitario/` after each change.

4. In Obsidian, open **Settings → Appearance → Themes**, select **Universitario**, and reload the theme after edits (toggle it off and on, or use the **Reload app without saving** command).

> [!NOTE]
> The theme folder name must match the `name` field in `manifest.json` (`Universitario`).

## Project structure

| File | Purpose |
| --- | --- |
| `theme.css` | The entire theme: variables, layout, typography, callouts, and snippets. |
| `manifest.json` | Theme metadata. `version` drives releases (see below). |
| `README.md` | User-facing documentation and screenshots. |
| `CONTRIBUTING.md` | This guide. |
| `LICENSE` | GNU GPL v3. |

There is no build step: Obsidian loads `theme.css` and `manifest.json` directly.

## CSS guidelines

- Keep everything in `theme.css`; do not add build tooling or preprocessors.
- Use tabs for indentation to match the existing file.
- Prefer Obsidian CSS variables (`--text-normal`, `--background-primary`, `--interactive-accent`, etc.) over hard-coded colors, so the theme keeps working with any accent color and in light/dark mode.
- Scope rules to the relevant component (`.markdown-preview-view`, `.workspace-leaf-content`, etc.) instead of styling bare elements globally.
- Avoid `!important` unless it is the only way to override Obsidian's defaults. The current theme deliberately uses very few.
- Keep selectors as simple as possible and group related rules under the numbered section banners already present in the file.
- When adding a new section, update the index at the top of `theme.css`.
- Test changes in both light and dark mode and with at least two accent colors.
- Do not reformat or reorder unrelated parts of the file; it makes review harder.

Example of the preferred style:

```css
/* 8. EXAMPLE SECTION
   ========================================================================== */

.theme-dark .my-component {
	--my-component-bg: var(--background-secondary);
	background-color: var(--my-component-bg);
}
```

## Testing checklist

Before opening a pull request, confirm that:

- [ ] The theme loads without errors in **Settings → Appearance**.
- [ ] The change looks correct in light and dark mode.
- [ ] It works with at least two accent colors.
- [ ] Headings, callouts (including `supercallout`), tables, code blocks, and checkboxes still render correctly.
- [ ] The `collage` cssclass and `#flashcard` styles still work.
- [ ] Nothing breaks on mobile (or note that mobile was not tested).
- [ ] `theme.css` has no syntax errors (a quick way is to run `npx csstree-validator theme.css`, or paste the file into a CSS validator).

## Branching and commits

- Fork the repository and create a branch from `main`.
- Use a descriptive branch name, for example `fix/callout-spacing` or `feat/collage-layout`.
- Write commit messages in the imperative mood and keep them short, for example:
  - `Fix callout padding in reading view`
  - `Add compact table styles`
  - `Update README screenshots`
- Keep one logical change per commit when possible.

## Pull requests

1. Describe **what** changed and **why**.
2. Include before/after screenshots for visual changes.
3. Reference related issues (`Closes #123`).
4. Keep pull requests focused; split unrelated changes into separate PRs.
5. Make sure the **Validate** workflow passes. It checks `theme.css` with `csstree-validator` and verifies the required fields in `manifest.json`.
6. Be open to feedback. Maintainers may ask for adjustments to match the theme's visual language.

Small fixes (typos, obvious CSS bugs) are welcome as direct PRs. For larger changes, open an issue first so we can agree on the approach.

## Versioning and releases

The project follows [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`):

- **PATCH** (`2.1.30` → `2.1.31`): bug fixes, small visual corrections.
- **MINOR** (`2.1.30` → `2.2.0`): new features, new callout styles, new optional layouts.
- **MAJOR** (`2.1.30` → `3.0.0`): breaking changes, such as removed classes or renamed options.

Releases are automated. To publish a new version:

1. Update `version` in `manifest.json`.
2. Commit the change and merge it into `main`.
3. The [`Release`](.github/workflows/release.yml) workflow detects the new version, creates the matching Git tag (for example `2.1.31`), and publishes a GitHub release with `manifest.json` and `theme.css` attached.
4. If the tag already exists, the workflow skips the release. Never reuse a version number.

Do not create tags or releases manually unless the automation fails.

## License

By contributing, you agree that your contributions are licensed under the [GNU General Public License v3.0](LICENSE).
