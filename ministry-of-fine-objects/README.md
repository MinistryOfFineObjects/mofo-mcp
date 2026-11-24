# Ministry of Fine Objects

A Cursor / Grok Bot plugin that connects to the Ministry of Fine Objects (MOFO) trade furniture catalogue through the remote MCP server at [https://mcp.ministryoffineobjects.com/mcp](https://mcp.ministryoffineobjects.com/mcp).

Install it to search products, look up specifications and list prices, browse manufacturers and categories, retrieve product photos, and — after the buyer signs in — request trade (net) prices.

Homepage: [ministryoffineobjects.com](https://ministryoffineobjects.com)

## What this plugin does

MOFO is a trade furniture marketplace. After you install the plugin, Cursor and Grok Bot get a connector to the live catalogue. Agents can then find furniture, lighting, and decor, open a product's public details, and show photos.

Quotes and ordering are not available yet. All six MCP tools are read-only. No API keys or secrets are bundled with this plugin. Catalogue tools work without an account. Trade (net) prices require the buyer to sign in with an approved MOFO trade account; the host discovers OAuth from the server and prompts for sign-in.

## Tools

| Tool | What it does | Sign-in |
| --- | --- | --- |
| `search_products` | Search the catalogue by keyword, with optional filters (category, manufacturer, list price, stock, contract grade, color, shape, dimensions). Returns list prices in USD. | No |
| `get_product` | Full public details for one product id from search: specs, materials, list price, images, lead time, policies. | No |
| `get_product_image` | Returns an actual product photo through MCP, useful when a direct image URL cannot be downloaded. | No |
| `list_categories` | Categories and subcategories to use as search filters. | No |
| `list_manufacturers` | Manufacturers currently sold on MOFO, with freight and return policies. | No |
| `get_net_price` | The signed-in buyer's trade (net) price for up to 5 product ids. Limited to 60 lookups per hour. Buyers without an approved trade account only get list price. Net prices are not available for a whole category: search first, then request net prices on the products under consideration. | Yes — MOFO trade account, OAuth scope `net_prices` |

The server does not expose MCP resources or prompts.

## Sign-in for net prices

Leave `mcp.json` as shipped. Do not put tokens in the URL, in headers, or in this repository.

When a buyer (or the plugin owner) wants trade pricing, the client should follow the server's OAuth metadata and prompt them to connect their MOFO account. The authorization server is `https://mcp.ministryoffineobjects.com`, with the `net_prices` scope. Without that sign-in, `get_net_price` returns HTTP 401 and asks the user to connect their MOFO account.

Catalogue search, product details, images, categories, and manufacturers do not require sign-in.

## Privacy

- This plugin contains no secrets, API keys, or access tokens.
- Unauthenticated catalogue calls send only the search or product-lookup arguments you (or the agent) provide to `https://mcp.ministryoffineobjects.com/mcp`.
- Trade (net) prices are requested only after the buyer signs in. The access token is handled by the Cursor / Grok Bot host, not stored in this plugin.
- MOFO's site privacy page is at [ministryoffineobjects.com/privacy](https://ministryoffineobjects.com/privacy).
- If you discover a security issue with this plugin, follow Cursor's marketplace reporting process, including [security-reports@cursor.com](mailto:security-reports@cursor.com).

## Local testing

This plugin lives in the `ministry-of-fine-objects/` directory of the [mofo-mcp](https://github.com/MinistryOfFineObjects/mofo-mcp) repository. The marketplace manifest at the repository root (`.cursor-plugin/marketplace.json`) points at this folder.

From the repository root, run the official validator (from the [Cursor plugin template](https://github.com/cursor/plugin-template)):

```bash
node ministry-of-fine-objects/scripts/validate-template.mjs
```

To try the plugin in Cursor before submitting, copy or symlink the `ministry-of-fine-objects` folder to `~/.cursor/plugins/local` and reload. After install, the MCP server appears with a `plugin-` prefix (for example `plugin-ministry-of-fine-objects-ministry-of-fine-objects`) in approval prompts.

## Logo

`assets/logo.png` is MOFO's official monogram, taken from the site favicon source on [ministryoffineobjects.com](https://ministryoffineobjects.com) (original 250×250 PNG).

## License

MIT. See [LICENSE](LICENSE).
