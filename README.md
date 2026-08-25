# alundorzhu.github.io

My personal site. Svelte 5 + TypeScript + Vite, deployed to
<https://alundorzhu.github.io>.

## Develop

```sh
bun install
bun run dev      # local dev server
bun run check    # svelte-check + tsc
bun run build    # production build to dist/
```

## Resume

`resume/main.tex` is the source of truth. `public/resume.pdf` is a build
artifact that is **committed on purpose** — CI has no LaTeX toolchain, so the
PDF has to be built locally:

```sh
bun run resume   # latexmk -> resume/main.pdf -> public/resume.pdf
```

Everything else under `resume/` is gitignored. If you edit the `.tex`, rerun
that script and commit the PDF in the same change — CI fails the build if the
tex is newer than the PDF.

## Deploy

Pushing to `main` runs `.github/workflows/deploy.yml`, which builds and
publishes to GitHub Pages. Deploys can also be triggered by hand from the
Actions tab.
