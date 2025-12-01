# YAML Validation Setup for Datafold Monitors

Get real-time validation, autocomplete, and inline documentation for your Datafold monitors-as-code configurations!

[Read more about monitors-as-code in our docs](https://docs.datafold.com/data-monitoring/monitors-as-code)

## Table of Contents

- [Overview](#overview)
- [VSCode Setup (Recommended)](#vscode-setup-recommended)
- [IntelliJ/PyCharm Setup](#intellijpycharm-setup)
- [Other Editors](#other-editors)
- [Inline Schema Reference](#inline-schema-reference)
- [Troubleshooting](#troubleshooting)
- [Examples](#examples)

---

## Overview

Datafold provides a JSON Schema that enables your editor to:

- **Validate** your monitor configurations in real-time
- **Autocomplete** field names and values
- **Show documentation** on hover
- **Suggest** valid configuration options
- **Highlight errors** with helpful messages

**Schema URL:**
```
https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json
```

---

## VSCode/Cursor Setup (Recommended)

### Step 1: Install YAML Extension

1. Open VSCode/Cursor
2. Open "Extensions" (Press `Ctrl+Shift+X` (Windows/Linux) or `Cmd+Shift+X` (Mac))
3. Search for **"YAML"** by Red Hat
4. Click **Install**

Or install directly: [YAML Extension by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

### Step 2: Configure Schema Mapping

Choose **Option A** (workspace-specific) or **Option B** (user-wide):

#### Option A: Workspace Settings (Recommended)

Create or edit `.vscode/settings.json` in your project root:

```json
{
  "yaml.schemas": {
    "https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json": [
      "monitors.yaml",
      "monitors.yml",
      "**/monitors/**/*.yaml",
      "**/monitors/**/*.yml",
      "**/*-monitors.yaml",
      "**/*-monitors.yml"
    ]
  },
  "yaml.customTags": [
    "!include scalar",
    "!include_dir_list scalar"
  ]
}
```

This configuration applies validation to any file matching the patterns above.

#### Option B: User Settings (All Projects)

1. Press `Ctrl+,` (Windows/Linux) or `Cmd+,` (Mac) to open Settings
2. Search for `yaml.schemas`
3. Click "Edit in settings.json"
4. Add the same configuration as Option A

### Step 3: Verify Setup

1. Open or create a file named `monitors.yaml`
2. Start typing and you should see:
   - Autocomplete suggestions
   - Documentation on hover
   - Red underlines for errors

**Test with this snippet:**

```yaml
monitors:
  test_monitor:
    type: test
    connection_id: # Hover to see docs!
```

---

## IntelliJ/PyCharm Setup

### Step 1: Configure JSON Schema Mapping

1. Go to **Settings/Preferences** (`Ctrl+Alt+S` or `Cmd+,`)
2. Navigate to **Languages & Frameworks → Schemas and DTDs → JSON Schema Mappings**
3. Click **+** to add a new mapping
4. Configure the mapping:

   | Field | Value |
   |-------|-------|
   | **Name** | Datafold Monitors |
   | **Schema file or URL** | `https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json` |
   | **Schema version** | JSON Schema version 7 |

5. Under **File path pattern**, click **+** and add:
   - `monitors.yaml`
   - `monitors.yml`
   - Or custom patterns like `*-monitors.yaml`

### Step 2: Apply and Test

1. Click **OK** to save
2. Open a monitor configuration file
3. Validation should work automatically!

---

## Other Editors

Most modern editors support JSON Schema validation for YAML files:

### Neovim/Vim

Install [yaml-language-server](https://github.com/redhat-developer/yaml-language-server):

```bash
npm install -g yaml-language-server
```

Configure in your LSP setup (e.g., with `nvim-lspconfig`):

```lua
require'lspconfig'.yamlls.setup {
  settings = {
    yaml = {
      schemas = {
        ["https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json"] = "monitors*.{yaml,yml}"
      }
    }
  }
}
```

### Sublime Text

1. Install [LSP](https://packagecontrol.io/packages/LSP) package
2. Install [LSP-yaml](https://packagecontrol.io/packages/LSP-yaml)
3. Configure in `LSP-yaml` settings:

```json
{
  "settings": {
    "yaml.schemas": {
      "https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json": ["monitors*.{yaml,yml}"]
    }
  }
}
```

### Emacs

Use [lsp-mode](https://github.com/emacs-lsp/lsp-mode) with yaml-language-server:

```elisp
(use-package lsp-mode
  :hook (yaml-mode . lsp)
  :config
  (setq lsp-yaml-schemas
        '(:https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json ["monitors*.yaml" "monitors*.yml"])))
```

---

## Inline Schema Reference

You can specify the schema directly in your YAML files using a comment:

```yaml
# yaml-language-server: $schema=https://raw.githubusercontent.com/datafold/monitors-schema/main/schema.json

monitors:
  my_monitor:
    type: test
    connection_id: 123
    test:
      type: accepted_values
      tables:
        - path: schema.table
          columns: [column1]
      variables:
        accepted_values:
          value: [1, 2, 3]
          quote: false
    schedule:
      interval:
        every: hour
    notifications: []
```

**Benefits:**
- Works without editor configuration
- Self-documenting (schema URL is visible)
- Portable (works on any machine)
- Useful for shared snippets

**When to use:**
- One-off configuration files
- Shared examples and templates
- CI/CD validation scripts
- Files outside standard naming patterns

---

## Troubleshooting

### Validation Not Working

**Problem:** Editor shows no validation, autocomplete, or hover docs.

**Solutions:**

1. **Verify YAML extension is installed**
   - VSCode: Check Extensions panel
   - IntelliJ: Check Plugins

2. **Check file name patterns**
   - Ensure your file matches configured patterns (e.g., `monitors.yaml`)
   - Try adding inline schema reference (see above)

3. **Reload editor window**
   - VSCode: `Ctrl/Cmd+Shift+P` → "Reload Window"
   - IntelliJ: File → Invalidate Caches / Restart

4. **Verify schema URL is accessible**
   - Open the URL in a browser to ensure it loads
   - Check network/firewall settings


## Questions or Issues?

Contact [support team](mailto:support@datafold.com).


**Happy monitoring!** 🎉
