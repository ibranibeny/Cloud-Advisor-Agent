# Document Processing Workflow

## Trigger
File attachment detected, or user says: "analyze this document", "review this file", "what does this say".

## Supported Formats

| Format | MCP Tool | Action |
|--------|----------|--------|
| `.xlsx` / `.xlsm` | `mcp_excel-mcp_file` + `mcp_excel-mcp_range` | Open and read cell data |
| `.docx` / `.pdf` | `mcp_microsoft_mar_convert_to_markdown` | Convert to markdown for analysis |
| `.pptx` | `mcp_microsoft_mar_convert_to_markdown` | Convert slides to markdown |
| Image attachments | Vision/multimodal | Analyze architecture diagrams |

## Unsupported Formats

If file type is not in the supported list:
> Unsupported file format: `{extension}`. Supported: Word (.docx), PDF (.pdf), PowerPoint (.pptx), Excel (.xlsx/.xlsm), and images (.png/.jpg).

## Procedure

1. **Identify file type** from the attachment
2. **Extract content**:
   - Excel -> Use Excel MCP to read ranges/tables
   - Word/PDF/PPT -> Use `mcp_microsoft_mar_convert_to_markdown` to get text
   - Images -> Analyze visually for architecture patterns
3. **Classify intent** -- Is this an existing architecture doc? A requirements list? A cost sheet? An infrastructure diagram?
4. **Route to domain** -- Apply the relevant workflow (architecture, migration, cost, etc.) using extracted content as context
5. **Respond** with recommendations tailored to the document content

## Constraints
- **Single-turn**: Complete extraction and advisory in one response
- Do not ask multi-step follow-ups for extraction
