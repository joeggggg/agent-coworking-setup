# Antigravity Workspace Rules

These persistent rules apply to all coding, scripting, and document authoring actions performed by the Antigravity agent in this workspace.

---

## 🛠️ Code Modification Standards

1.  **Preserve Comments & Structure**: Keep all unrelated comments, docstrings, and headers intact when making edits.
2.  **No Placeholders**: Never write placeholder code (e.g., `// TODO: implement later` or `...`). All code must be fully implemented and functional.
3.  **Contiguous Edits**: Use targeted, contiguous edits rather than overwriting full files to minimize git diff footprint.

## 📋 Execution & Workflow Rules

1.  **Multi-Step Tasks**: For complex or multi-step tasks, always execute a plan first:
    *   State the target goal in a single sentence.
    *   List out the steps with estimated efforts (S/M/L).
    *   Stop and wait for user confirmation before executing.
2.  **Verification**: After implementing any changes:
    *   Run tests or linters if available in the folder.
    *   For UI changes, build/run the dev server and test manually (use a browser subagent if required).
    *   Document what was tested and results in a walkthrough.

## 📝 Document Formatting & Styling

1.  **Markdown First**: All documentation, notes, and specs must use GitHub-flavored markdown (GFM) unless another format is explicitly requested.
2.  **File Schemes**: Use clickable `file://` URLs when referencing files and folders. Use forward slashes for Windows path compatibility.
3.  **Aesthetics**: When generating UI prototypes or dashboards, use HSL-derived color palettes, dark modes, modern typography. Avoid generic or flat HTML styling.
