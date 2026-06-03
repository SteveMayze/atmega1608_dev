# ATMega1608 Dev KiCad Automation

This project includes release automation for fabrication deliverables.

The automation entrypoints are intentionally stored at repository root so GitHub Actions can discover workflows correctly.

## Current automation goal

When a Git tag matching `r#.#.#` is pushed (example: `r0.1.0`), GitHub Actions will:

1. Set the KiCad text variable `RELEASE` in the project file.
2. Generate Gerber and drill files using `kicad-cli`.
3. Write deliverables to `gerber/`.
4. Upload artifacts and attach a ZIP to the GitHub release.

## Files added for automation

- `.github/workflows/release-gerbers.yml`: Release workflow triggered by version tags.
- `scripts/generate_release_artifacts.py`: Single entrypoint script for local and CI generation.
- `release-artifacts.json`: User-editable release configuration, including Gerber include layers.
- `PROJECT_TRACKING.md`: Task and roadmap tracking.

## Local usage

### Prerequisites

- KiCad installed with `kicad-cli` available on PATH.
- Python 3.9+.

If `kicad-cli` is not on PATH, set:

- macOS/Linux: `export KICAD_CLI=/full/path/to/kicad-cli`
- Windows (PowerShell): `$env:KICAD_CLI="C:\\Path\\To\\kicad-cli.exe"`

### Configure Gerber include layers

Edit `release-artifacts.json` and set:

- `project.path`: Relative path to your KiCad `.kicad_pro` file.
- `gerber.include_layers`: Layers to include in Gerber export.

Example:

```json
{
	"project": {
		"path": "kicad/ATMega1608_Dev/ATMega1608_Dev.kicad_pro"
	},
	"gerber": {
		"include_layers": [
			"F.Cu",
			"B.Cu",
			"F.Mask",
			"B.Mask",
			"F.SilkS",
			"B.SilkS",
			"Edge.Cuts"
		]
	}
}
```

This list is passed to `kicad-cli` as the `--layers` argument.
The script validates configured layer names against the board's `(layers ...)` list in the PCB file and fails early if any configured layer is invalid.

### Generate Gerbers for a release tag

```bash
python3 scripts/generate_release_artifacts.py --tag r0.1.0
```

This will update `RELEASE` to `0.1.0` and generate output in `gerber/`.

If needed, `--project` can still be passed to override `project.path` from config.

### Generate without updating `RELEASE`

```bash
python3 scripts/generate_release_artifacts.py --tag r0.1.0 --no-update-release-variable
```

### Dry run (no generation, just prints commands)

```bash
python3 scripts/generate_release_artifacts.py --tag r0.1.0 --dry-run
```

## Release trigger

Push a release tag:

```bash
git tag r0.1.0
git push origin r0.1.0
```

The workflow runs automatically for tags matching `r*.*.*`.

## Future extension

The script already includes optional flags for future needs:

- `--schematic-pdf` to export schematic PDF.
- `--board-pdf` to export PCB layout PDF.

These are optional now and can be enabled later in the workflow.
