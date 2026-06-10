# geocities-y2k-converter

![OpenSpec](https://img.shields.io/badge/OpenSpec-enforced-blueviolet) ![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

> A Claude Code skill that converts modern websites into retro Geocities / Y2K-era style

---

## Quick start

```bash
# 1. Clone and install
git clone https://github.com/arananet/geocities-y2k-converter.git
cd geocities-y2k-converter
bash setup.sh

# 2. Use the skill in Claude Code
/geocities-y2k-converter
```

---

## Usage

Invoke the skill from Claude Code on any website project:

```
/geocities-y2k-converter
```

Claude will inspect your site's stack, add a persisted retro toggle, wire up
`assets/geocities.css`, and build a self-contained `GeoCitiesHome` component
— all without touching the existing modern design.

Copy the bundled assets into your project:
```bash
cp skills/geocities-y2k-converter/assets/geocities.css <your-project>/public/
cp skills/geocities-y2k-converter/assets/template.html <your-project>/  # plain HTML reference
```

### Skill structure

```
skills/geocities-y2k-converter/
├── skill.md              # skill definition & conversion guide
└── assets/
    ├── geocities.css     # drop-in Y2K stylesheet (scoped under .geocities-page)
    └── template.html     # reference page with {{PLACEHOLDER}} slots
```

---

## Contributing

This project uses **OpenSpec** for spec-driven development — every feature
or bugfix starts with a spec file under `.openspec/specs/`. Each spec
includes a `roles` block to assign responsibility (`implementer`,
`reviewer`, `qa`, `product_owner`). See
[`docs/OPENSPEC.md`](docs/OPENSPEC.md) for the full workflow, or
[`CONTRIBUTING.md`](CONTRIBUTING.md) for the contributor checklist.

---

## Documentation

| Topic | Where |
|---|---|
| Spec-driven workflow | [`docs/OPENSPEC.md`](docs/OPENSPEC.md) |
| Branch protection setup | [`docs/BRANCH_PROTECTION.md`](docs/BRANCH_PROTECTION.md) |
| Architecture decisions | [`docs/adr/`](docs/adr/) |
| Security policy | [`SECURITY.md`](SECURITY.md) |
| Support channels | [`SUPPORT.md`](SUPPORT.md) |
| Release history | [`CHANGELOG.md`](CHANGELOG.md) |

---

## License

[MIT](LICENSE)

---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2H51MPWG)
