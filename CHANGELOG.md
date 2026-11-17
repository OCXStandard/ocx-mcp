# Changelog — ocx-mcp

## 0.1.0 — 2026-11-17

- Initial release: the `ocx-mcp` server moved out of ocx-platform
  (`ocx_model_validator.mcp`, platform v0.7.0) with its five tools
  `load_model`, `get_model_info`, `get_frame_table`, `get_compartments`
  and `apply_scantlings`; behaviour and result shapes unchanged.
- Depends on `ocx-model-core` and `ocx-model-validator` via uv git sources
  pinned to ocx-platform `f8fd680069099247a6a31640edef86d3683b381b`.
