# PROJECT_TRACKING

## Objective

Create a reusable release automation pattern for this KiCad project that can later be moved into a project template.

## Scope (current)

- [x] Add a single script entrypoint for fabrication deliverables.
- [x] Generate Gerber files with `kicad-cli`.
- [x] Generate drill files with `kicad-cli`.
- [x] Write outputs to `gerber/`.
- [x] Trigger generation from Git tag pattern `r#.#.#`.
- [x] Propagate release version from tag to KiCad text variable `RELEASE`.
- [x] Add user-configurable Gerber include layer list via JSON config.

## Scope (next)

- [ ] Export schematic PDF during release automation.
- [ ] Export PCB layout PDF during release automation.
- [ ] Add KiCad DRC/ERC checks in CI before artifact generation.
- [ ] Standardize artifact naming conventions for manufacturing handoff.

## Template readiness tasks

- [ ] Move automation files into a dedicated KiCad template skeleton.
- [ ] Parameterize project file name discovery for multi-project repositories.
- [ ] Add a template onboarding checklist (toolchain, paths, variables).
- [ ] Document compatibility matrix for KiCad versions used in CI.

## Notes

- The source of truth for release version is the Git tag (`r#.#.#`).
- The script maps this tag to `RELEASE=#.#.#` in the `.kicad_pro` file before generation.
- This keeps release identity in one place while preserving KiCad text variable usage.
