# Sihari Labs — Claude Code Plugins

A collection of Claude Code plugins by Sihari Labs.

## Available Plugins

| Plugin | Description | Version |
|--------|-------------|---------|
| [chatverce-design](./chatverce-design/) | Jony Ive-inspired design system and brand identity for Chatverce | 1.0.0 |

## Installation

```bash
# Add this marketplace (from inside Claude Code)
/plugin marketplace add org-siharilabs/plugins

# Install a plugin (from inside a Chatverce project)
/plugin install chatverce-design@siharilabs-plugins
```

## Team Setup

Add to your project's `.claude/settings.json` so team members get prompted automatically:

```json
{
  "extraKnownMarketplaces": {
    "siharilabs-plugins": {
      "source": {
        "source": "github",
        "repo": "org-siharilabs/plugins"
      }
    }
  },
  "enabledPlugins": {
    "chatverce-design@siharilabs-plugins": true
  }
}
```

## Local Development

```bash
# Test the marketplace locally
/plugin marketplace add ./path/to/plugins

# Or load a single plugin directly
claude --plugin-dir ./chatverce-design
```

## Validation

```bash
claude plugin validate .
```
