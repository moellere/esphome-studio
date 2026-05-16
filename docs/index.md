# wirestudio documentation

Wirestudio is an agent-driven IoT device design tool. Describe a goal (or pick parts); get ESPHome YAML, an ASCII wiring diagram, and a BOM that compile under upstream ESPHome.

## Architecture

```
   design.json  ── single source of truth (JSON-Schema-validated)
        │
        ▼
  ┌─ wirestudio.model         pydantic models mirroring the schema
  ├─ wirestudio.library       loads boards/ + components/ YAML
  ├─ wirestudio.generate      design + library → ESPHome YAML + ASCII
  ├─ wirestudio.csp           pin solver + port-compatibility checker
  ├─ wirestudio.recommend     deterministic capability ranking
  ├─ wirestudio.agent         Claude tool-using agent + session store
  ├─ wirestudio.designs       file-backed designs/<id>.json store
  ├─ wirestudio.fleet         fleet-for-esphome HTTP client
  ├─ wirestudio.enclosure     parametric OpenSCAD + Thingiverse search
  ├─ wirestudio.kicad         SKiDL schematic emitter + .kicad_sym importer
  ├─ wirestudio.mcp           MCP server over the agent tool surface
  └─ wirestudio.api           FastAPI HTTP layer (mounts everything above)
                          serve.py adds the production wrapper:
                          API at /api/*, web bundle at /
```

Generators are pure functions of `design.json` + the static library — no artifact-to-document round-trips. Library files in `wirestudio/library/components/` carry the electrical metadata ESPHome doesn't (pin roles, voltage ranges, current draw, decoupling caps, pull-up requirements) plus a Jinja2 template that renders the ESPHome YAML for that component, an `enclosure:` block the OpenSCAD generator reads, and a `kicad:` block the schematic exporter reads.

## Documentation Index

- [User Guide](user_guide.md) - Learn how to use the Web UI, inspector, header actions, and the CLI.
- [Deployment](deployment.md) - Learn how to self-host Wirestudio using Docker or Kubernetes.
- [Integrations](integrations.md) - Documentation on integrating with Fleet, OpenSCAD/Thingiverse, KiCad, and the MCP agent.
- [Library Coverage](library-coverage.md) - View the coverage matrix of the library components.
- [Model Context Protocol](MCP.md) - Read detailed instructions on how the MCP integration works.