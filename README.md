# ocx-mcp

[MCP](https://modelcontextprotocol.io) server that exposes OCX `.3docx` ship
models to LLM clients: load a model, read its info, frame table and
compartments, and write optimised scantlings back.

Built on the [ocx-platform](https://github.com/OCXStandard/ocx-platform)
packages `ocx-model-core` (parsing, IR, write-back) and `ocx-model-validator`
(frame table, compartment data).

## Install

Requires Python ≥ 3.12 and [uv](https://docs.astral.sh/uv/).

```bash
uv sync
```

## Run

The server speaks MCP over stdio:

```bash
uv run ocx-mcp
```

MCP client configuration (e.g. `mcp.json`):

```json
{
  "mcpServers": {
    "ocx": {
      "command": "uv",
      "args": ["run", "--directory", "C:\\PythonDev\\ocx-mcp", "ocx-mcp"]
    }
  }
}
```

## Tools

| Tool | Purpose |
|---|---|
| `load_model` | Parse a `.3docx` file and build the IR (kept in session state); returns the vessel id and entity counts |
| `get_model_info` | Vessel id, vessel name and entity counts (panels, plates, stiffeners, compartments, ref planes, materials, sections) |
| `get_frame_table` | Frame 0 offset, spacing entries and frame positions (mm), plus extraction warnings |
| `get_compartments` | Per compartment: id, name, tank type, COG (mm), volume (m³), extent (mm), filling height, air-pipe height and relief-valve pressure; plus extraction warnings |
| `apply_scantlings` | Apply an `nh-optimisation/1` report to a `.3docx`: plate thicknesses plus stiffener `BarSection`s, written to a new file; returns updated/skipped counts, unmatched rows and the output path |

Every tool returns a dict with `"ok": true` and its data, or `"ok": false`
and an `"error"` message.

## Upgrading the platform

The platform packages are pinned to one ocx-platform commit in
`[tool.uv.sources]` of `pyproject.toml`. To upgrade, change both `rev`
values to the new commit and run `uv lock`.

## Development

```bash
uv sync --dev
uv run pytest
uvx pre-commit run --all-files
```

## Provenance

Moved out of `ocx-model-validator` (`ocx_model_validator.mcp`) at
ocx-platform commit `f8fd680069099247a6a31640edef86d3683b381b` (release v0.7.0).
