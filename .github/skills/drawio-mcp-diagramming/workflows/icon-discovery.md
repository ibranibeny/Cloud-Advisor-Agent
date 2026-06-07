# Icon Discovery and Validation

## Azure Icons (Azure2 Library)

### Lookup
Grep the static catalog — no scripts needed at agent runtime:
```bash
grep -i "gateway" references/azure2-complete-catalog.txt
```

### Style Format
Use Azure2 image style entries:
```
image;aspect=fixed;html=1;points=[];align=center;image=img/lib/azure2/<category>/<Icon_Name>.svg;
```

### Hard Gate
- If an icon path cannot be confirmed in the catalog, do **not** use it in `drawio/create_diagram`
- Find an alternative via grep first

### Caveats
1. **Wrong style type** — `shape=mxgraph.azure2.*` may not render in some hosts. Prefer `image=img/lib/azure2/...` style.
2. **Library/environment mismatch** — Some embedded viewers/extensions do not resolve `img/lib/azure2/...` consistently. If icons do not render in one host, test in `app.diagrams.net`.

### Refresh Catalog (periodic, human-run)
```bash
cd .github/skills/drawio-mcp-diagramming/scripts
python3 search_azure2_icons_github.py --max-results 9999 > ../references/azure2-complete-catalog.txt
```

---

## AWS Icons (AWS4 Stencil Library)

> **Important**: AWS4 icons are **stencil-based**, not individual SVG files. They use `shape=mxgraph.aws4.<name>` rather than `image=img/lib/aws4/...`.

### Lookup
```bash
grep -i "lambda" references/aws4-complete-catalog.txt
```

### Style Format
```
shape=mxgraph.aws4.<shape_name>;fillColor=<service_color>;fontColor=#ffffff;strokeColor=none;
```

### Fill Colour Conventions
| Service Category | `fillColor` |
|---|---|
| Compute (orange) | `#ED7100` |
| Storage (green) | `#3F8624` |
| Database (red) | `#C7131F` |
| Networking (purple) | `#8C4FFF` |
| Security (red) | `#DD344C` |
| Management (pink) | `#E7157B` |
| General/generic (dark) | `#232F3E` |

### Hard Gate
- If a shape name cannot be confirmed in the catalog, do **not** use it in `drawio/create_diagram`
- Find an alternative via grep first

### Caveats
1. **Wrong style approach** — Do **not** use `image=img/lib/aws4/...` — AWS4 icons are stencils, not SVG files.
2. **Library/environment mismatch** — Some embedded viewers may not load the `mxgraph.aws4` stencil library. If shapes do not render in VS Code, test in `app.diagrams.net`.

### Refresh Catalog (periodic, human-run)
```bash
cd .github/skills/drawio-mcp-diagramming/scripts
python3 search_aws4_icons_github.py --max-results 9999 > ../references/aws4-complete-catalog.txt
```

---

## Render Review Fallback
If diagram review shows wrong/missing icon rendering:
1. Grep the relevant catalog for alternative paths/names
2. Substitute with verified alternative
3. Regenerate the diagram

## Fallback Strategy if Icons Still Fail
- Do **not** generate the diagram with an unresolved icon set
- Return the missing icon list and propose verified replacements (grepped from the relevant catalog)
- After replacements validate to `OK`, then generate the diagram
