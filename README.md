# Sihari Labs — Claude Code Plugins

A collection of Claude Code plugins by Sihari Labs.

## Available Plugins

| Plugin | Description | Version |
|--------|-------------|---------|
| [chatverce-design](./chatverce-design/) | Jony Ive-inspired design system and brand identity for Chatverce | 1.0.0 |

## Installation

```bash
# Add this marketplace
claude plugin marketplace add --url https://github.com/org-siharilabs/plugins

# Install a plugin (from inside a Chatverce project)
claude plugin install chatverce-design@org-siharilabs-plugins --scope project
```

## Local Development

```bash
# Run from inside the plugins/ repo root
claude --plugin-dir ./chatverce-design
```
