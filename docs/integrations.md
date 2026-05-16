# Integrations

Wirestudio integrates with several external tools and ecosystems to turn your design into reality.

## Fleet Handoff

The **Push to fleet** flow allows you to seamlessly push rendered ESPHome YAML to a running [`weirded/fleet-for-esphome`](https://github.com/weirded/fleet-for-esphome) Home Assistant add-on over Bearer-token HTTP.

- **Option to compile:** You can optionally enqueue an OTA build with live log streaming (Server-Sent Events).
- **Strict mode integration:** If strict mode is enabled, the studio refuses the push if any warn/error compatibility issues remain.

### Configuration

To enable the fleet handoff, point the API at a running fleet-for-esphome add-on:

```sh
export FLEET_URL=http://homeassistant.local:8765
export FLEET_TOKEN=$(grep -oP '(?<=token: )\S+' .../addon/secrets.yaml)
python -m wirestudio.api
```

`GET /fleet/status` reports `available: true` when both env vars are set and the addon answers a probe.

## Enclosures

Wirestudio assists in creating physical enclosures for your devices.

- **OpenSCAD:** You can generate a parametric `.scad` shell from the board's mount-hole and USB-port metadata.
- **Thingiverse Search (Experimental):** Search community-uploaded models for a specific board. This requires a third-party API key.

### Configuration

To enable enclosure search (`/enclosure/search`):

```sh
export THINGIVERSE_API_KEY=your_key_here
```

## KiCad Schematic

The studio can emit a SKiDL Python script that the user runs locally to produce a `.kicad_sch` file.

This feature is currently evaluated as **Works (lighter checks)**: Unit tests assert the script is well-formed Python with expected nets, but it is not automatically verified by opening it in KiCad.

## Model Context Protocol (MCP)

The studio's design-editing tools are exposed over the Model Context Protocol at `/mcp`. Point Claude Code or Claude Desktop at the daemon and the model edits designs on your Claude subscription — no Anthropic key needed.

See [`MCP.md`](MCP.md) for full details.