# User Guide

This guide covers the primary ways to interact with wirestudio: the Web UI and the CLI.

## Web UI

The studio's Web UI is the primary interface for building and validating designs. When running the single-image Docker container or the local development server, the interface is accessible in your browser.

### The 3-Pane Layout

The interface is built around a standard 3-pane layout:

1. **Left Sidebar (Library & Storage):**
   - **Examples:** Bundled golden designs that serve as excellent starting points.
   - **Saved:** Your persisted `design.json` files.
   - **Boards & Components:** Browse the hardware library.
2. **Center Design Pane (The Output):**
   - Renders the ESPHome YAML output and ASCII wiring diagram based on your current design.
3. **Right Inspector:**
   - **Design Overview:** When nothing is selected, displays board details, fleet metadata, requirements, warnings, component list, buses, and design-level compatibility warnings.
   - **Component Instance:** When a component is selected, displays parameter configuration (driven by its schema), connections (Form or Pinout view), and instance-specific compatibility warnings.

### Header Actions

The header contains all the primary operations for your workflow:

- **Core Actions:**
  - **New:** Create a fresh design from a selected board.
  - **Save:** Persists the current design to the server.
  - **Reset:** Revert to the last saved state.
  - **Download JSON:** Export the raw `design.json` file.
- **Builder Actions:**
  - **Connect Device:** Detect a connected ESP via WebSerial for bootstrap configuration.
  - **Add Component:** A capability picker that ranks library matches against your desired use cases.
  - **Solve Pins:** Automatically assigns legal pins to all unbound connections based on capability-aware fallbacks.
- **Advanced / Export (when the "ADVANCED" toggle is active):**
  - **Schematic:** Export a SKiDL Python script that produces a `.kicad_sch`.
  - **Enclosure:** Generate a parametric `.scad` shell or search Thingiverse.
  - **Push to Fleet:** Ship YAML to a running `fleet-for-esphome` instance.
  - **Agent:** Access the natural language, tool-using design agent.

### Validation & Strict Mode

Wirestudio continuously runs a pin solver and port-compatibility checker in the background. It flags:
- Input-only-as-output errors
- Boot-strap risks
- Serial console reuse
- Voltage limits
- ADC2/WiFi conflicts
- Locked-pin mismatches

By enabling **STRICT** mode in the header, you promote warn/error compatibility issues to render errors, acting as a pre-deployment gate.

---

## CLI

You can interact with wirestudio via the command line to generate artifacts without the Web UI.

### Generating Artifacts

To generate ESPHome YAML and an ASCII wiring block to stdout:

```sh
python -m wirestudio.generate wirestudio/examples/garage-motion.json
```

To write the generated artifacts directly to files:

```sh
python -m wirestudio.generate wirestudio/examples/garage-motion.json \
    --out-yaml build/garage-motion.yaml \
    --out-ascii build/garage-motion.txt
```
