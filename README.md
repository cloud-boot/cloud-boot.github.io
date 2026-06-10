# cloud-boot/pages

Sources for **cloud-boot.github.io** — the cloud-boot landing page.
Built by [Hugo](https://gohugo.io) — same toolchain as
[openweft/pages](https://github.com/openweft/pages), same blue
palette, sibling-project page.

## Layout

```text
.
├── hugo.toml                       Site config + per-page params
├── content/
│   └── _index.md                   Homepage marker (empty)
├── data/
│   └── mesh.toml                   Mesh visualisation : arches / hub / distros
├── layouts/
│   ├── _default/baseof.html        Outer HTML shell
│   ├── index.html                  Homepage body (cloud-boot specific)
│   └── partials/
│       ├── nav.html                Topnav with brand + menu
│       ├── footer.html             Footer
│       └── mesh.html               Animated SVG (reads data/mesh.toml)
├── static/
│   └── css/main.css                Blue-dominant palette + mesh styling
│                                   (byte-identical to openweft/pages —
│                                   future move : extract to a shared theme)
└── public/                         Hugo build output (gitignored — built by CI)
```

## Build locally

```sh
# On-demand via pkgx (no global install) :
pkgx hugo server -D                          # live reload at http://localhost:1313/
pkgx hugo --gc --minify                      # production build → ./public/

# Or install Hugo system-wide :
brew install hugo                            # macOS
sudo apt install hugo                        # Debian / Ubuntu
sudo dnf install hugo                        # Fedora
```

Tested with Hugo 0.161 (the version pkgx ships at the time of
writing). The GitHub Actions workflow pins a specific minor
version — see `.github/workflows/hugo.yml`.

## Edit the mesh

[`data/mesh.toml`](data/mesh.toml) holds the cloud-boot mesh : 4 arch
nodes at the top (x86_64, arm64, riscv64, loongarch64), a central
hub (the multi-arch ISO), and 7 OS family nodes at the bottom
(Ubuntu, Debian, Rocky, Alpine, FreeBSD, NetBSD, OpenBSD). Spheres
flow hub → arch → distro encoding "one ISO, many targets".

Convention :

- `kind = "arch"` / `"distro"` / `"hub"` picks the CSS palette.
- `label_y` is the *final* y offset for the node's text label
  (negative = above, positive = below). Arch labels go above ;
  distro + hub labels go below.

## Edit the content

- Hero meta in [`hugo.toml`](hugo.toml) → `[params]`.
- Section bodies inlined in [`layouts/index.html`](layouts/index.html) —
  five sections : Why, Three paths, Architecture × hypervisor matrix,
  Components, Sibling : Weft.

## Deploy

`.github/workflows/hugo.yml` builds + deploys on every push to `main`,
same workflow as openweft/pages. Configure GitHub Pages on the repo
with Source = "GitHub Actions" (not "Deploy from a branch").

## Legacy files

This directory used to ship a hand-rolled `index.html` + `styles.css` +
`script.js`. They're left in place during the migration — Hugo's
`public/index.html` is the new authoritative version. Delete the
legacy files when the Hugo output is validated.

## Sibling

[Weft](https://openweft.github.io) is the cloud platform that uses
cloud-boot to boot its VMs. Same Hugo setup, same blue palette,
different mesh metaphor. Both pages are intentionally cohesive.
