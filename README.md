# fusion-3d-print-design

A Claude plugin for designing FDM 3D-printed parts in Autodesk Fusion. Its skill gives Claude Voron-style design rules, material notes, proven tolerances (clearance holes, heat-set inserts, press and sliding fits, snap fits), a parametric modeling workflow for Fusion, lessons learned for the Fusion MCP connector, and a check list before handover.

The skill itself: [`plugins/fusion-3d-print-design/skills/fusion-3d-print-design/SKILL.md`](plugins/fusion-3d-print-design/skills/fusion-3d-print-design/SKILL.md)

## Install as plugin (recommended, receives updates)

This repository is a plugin marketplace.

**Claude desktop app (Cowork)**

1. Open **Customize > Plugins**, select **Add marketplace** and enter `herrla/fusion-3d-print-design`.
2. Install the plugin **fusion-3d-print-design**.
3. To get changes, select **Update** on the marketplace.

**Claude Code**

```
/plugin marketplace add herrla/fusion-3d-print-design
/plugin install fusion-3d-print-design@herrla
```

To update automatically, open `/plugin` > **Marketplaces**, select `herrla` and choose **Enable auto-update**.

## Install as standalone skill (no updates)

For Claude chat, where plugins are not used: download `SKILL.md`, put it into a folder named `fusion-3d-print-design`, zip that folder, then upload the ZIP under **Customize > Skills**. Changes in this repository do not reach this copy; upload again after updates.

## Fusion connection

To let Claude model directly in Fusion, enable the Fusion MCP server in Fusion (**Preferences > General > API**) and install the **Autodesk Fusion** connector in the Claude desktop app. The skill walks you through this if the connection is missing. See [Connecting to the Autodesk Fusion MCP Server](https://help.autodesk.com/cloudhelp/ENU/ADSKMCP-FusionDesktopMcp/files/ADSKMCP_FusionDesktopMcp_connecting_to_the_fusion_mcp_server_html.html).

## Adjusting

The values in the skill are defaults. Values you give Claude for a project override them. Permanent changes go into this repository; plugin users receive them with the next update.
