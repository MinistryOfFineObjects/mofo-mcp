# Testing the Ministry of Fine Objects plugin locally in Cursor

Do this on your own computer before you submit the plugin to the Cursor Marketplace.

## Steps

1. In your home folder, create `~/.cursor/plugins/local/ministry-of-fine-objects`.
2. Copy the real files from the `ministry-of-fine-objects` subfolder of this repo (`mofo-mcp`) into it:
   - the `.cursor-plugin` folder with `plugin.json`
   - `mcp.json`
   - the `skills` folder
   - the `assets` folder
   - `README.md`

   Copy the actual files. Don't use symlinks or shortcuts, because Cursor skips a symlink that points somewhere else on disk.
3. Restart Cursor, or run **Developer: Reload Window**.
4. Open **Customize** and confirm that the `mofo` MCP server and the `mofo-catalog` skill appear.

## Suggested checks

- Confirm the `mofo` server connects and shows six tools.
- Ask the agent to search for a product. It should use `search_products` and return real catalog results.
- Ask something the `mofo-catalog` skill covers, and confirm the skill is used.
- Try a net-price query and confirm it prompts you to sign in.

## Cautions

- If a marketplace copy with the same name is installed, remove it first, because Cursor loads both.
- On a Cursor Enterprise plan, an admin must enable **Allow Local Plugin Imports**.

## After testing passes

Submit the repo link at https://cursor.com/marketplace/publish, using https://github.com/MinistryOfFineObjects/mofo-mcp.

Source: https://cursor.com/docs/plugins (section "Test plugins locally").
