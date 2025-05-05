# Obsidian PM Experiment Vault (T42)

This directory contains the Obsidian test vault and related configurations for Task T42 (Evaluating Obsidian + MCP for Project Management).

It is managed as a Git submodule within the main `career-agent` repository.

## Setup

1.  **Initialize Submodule:** Ensure the submodule is checked out (`git submodule update --init --recursive`).
2.  **Install Obsidian:** Ensure Obsidian (v1.8.10+) is installed.
3.  **Open Vault:** Open the `test_vault/` directory within this folder (`tools/obsidian-pm/test_vault/`) as a vault in Obsidian.
4.  **Install Plugins:** Via Obsidian's Community Plugins browser, install and enable:
    *   `Local REST API`
    *   `Tasks`
    *   `Kanban`
    *   `Dataview`
    *   `Projects`
    *   `MCP Tools` (by jacksteamdev)
5.  **Configure Local REST API:** Ensure the `Local REST API` plugin is configured and has an API key generated.
6.  **Download MCP Server Binary (Manual Step Required):** The `MCP Tools` plugin currently fails to download its server component (HTTP 404). Download it manually:
    *   Go to [MCP Tools Releases](https://github.com/jacksteamdev/obsidian-mcp-tools/releases).
    *   Download the appropriate binary for your platform (e.g., `mcp-server-linux` for Linux v0.2.23).
    *   Place the downloaded binary inside `test_vault/.obsidian/plugins/obsidian-mcp-tools/bin/`.
    *   Make the binary executable (e.g., `chmod +x test_vault/.obsidian/plugins/obsidian-mcp-tools/bin/mcp-server-linux`).

## Usage with Cursor

The MCP server is configured to run automatically via Cursor's MCP settings (`~/.cursor/mcp.json`). This configuration:
- Points to the downloaded binary.
- Sets the required `OBSIDIAN_API_KEY` environment variable (using the key from the `Local REST API` plugin).
- Makes the tools available to Cursor agents (prefixed with `mcp_obsidian-tools_...`).

**No manual server execution or `.env` file creation is needed for standard use within the Cursor/Agent workflow.**

## Running the MCP Server Manually (Optional - for Debugging)

If you need to run the server manually outside of Cursor (e.g., for debugging):

1.  **Create `.env` File:** Create a file named `.env` in this directory (`tools/obsidian-pm/.env`) with the following content, replacing the placeholder with the actual API key from the `Local REST API` plugin settings:
    ```
    OBSIDIAN_API_KEY=YOUR_API_KEY_HERE
    ```
    *(Refer to `.env.example`)*. **Do not commit the `.env` file.**
2.  **Execute from Submodule Root:** Run the following command from the *root of the submodule* (`tools/obsidian-pm/`):
    ```bash
    # Load .env file and execute server binary
    export $(grep -v '^#' .env | xargs) && ./test_vault/.obsidian/plugins/obsidian-mcp-tools/bin/mcp-server-linux
    ```
    This command sources the API key and executes the server, which communicates via stdio.
