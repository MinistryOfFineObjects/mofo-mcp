# MOFO MCP server

Connect your AI assistant to **[Ministry of Fine Objects (MOFO)](https://ministryoffineobjects.com)**,
a trade marketplace for contract and residential furniture, lighting, rugs and decor.
Your assistant can search the catalogue, read full product specs and look at product photos.
If you connect a MOFO trade account, it can also show your trade (net) prices.

| | |
|---|---|
| **Server URL** | `https://mcp.ministryoffineobjects.com/mcp` |
| **Transport** | Streamable HTTP |
| **Sign-in** | Not needed to browse. Only needed for trade prices (see [Trade prices](#trade-prices)). |

This repo holds the server's documentation and [changelog](CHANGELOG.md). The server's
source code isn't published.

## Connect

**Claude (web and desktop):** add a custom connector in Settings → Connectors and paste the server URL.

**Claude Code:**

```bash
claude mcp add --transport http mofo https://mcp.ministryoffineobjects.com/mcp
```

**Cursor:** add this to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "mofo": { "url": "https://mcp.ministryoffineobjects.com/mcp" }
  }
}
```

**VS Code:** add this to `.vscode/mcp.json`:

```json
{
  "servers": {
    "mofo": { "type": "http", "url": "https://mcp.ministryoffineobjects.com/mcp" }
  }
}
```

**Other clients:** use the server URL in any MCP client that supports Streamable HTTP.
To get trade prices, the client also needs to support OAuth sign-in.

## Tools

All tools are read-only.

| Tool | What it does |
|---|---|
| `search_products` | Searches the active catalogue by keyword and filters. Returns list prices in USD, key dimensions in inches, stock status, an image and a product page link. |
| `get_product` | Full details for one product: specs, materials, dimensions, list price, all images, lead time, certifications, and the manufacturer's freight and return policies. |
| `get_product_image` | Returns a product photo through MCP, so the assistant doesn't have to download it from a URL. Whether the image is displayed depends on your client. |
| `list_categories` | Categories and subcategories, to use as search filters. |
| `list_manufacturers` | Manufacturers currently sold on MOFO, with their freight and return policies. |
| `get_net_price` | Your trade (net) price for up to 5 products. Needs sign-in. |

### `search_products` inputs

All inputs are optional.

| Input | Meaning |
|---|---|
| `query` | Free text, such as `green outdoor sofa` or `walnut dining table` |
| `category` | `Seating`, `Tables`, `Casegoods`, `Lighting`, `Accessories` or `Outdoor` |
| `subcategory` | An exact subcategory name from `list_categories`, such as `Lounge Chairs` |
| `manufacturer` | An exact manufacturer name from `list_manufacturers` |
| `min_list_price`, `max_list_price` | List price range in USD |
| `min_width`, `max_width`, `min_depth`, `max_depth`, `min_height`, `max_height` | Size range in inches |
| `colors` | Any of: Beige, Black, Blue, Brown, Cream, Gold, Green, Grey, Multicolor, Natural, Orange, Pink, Purple, Red, Silver, Taupe, White, Yellow. Products without color data are left out. |
| `shapes` | Any of: Rectangle, Circle, Square, Oval, Irregular |
| `in_stock_only` | Only products in stock |
| `contract_grade_only` | Only contract-grade (commercial use) products |
| `page`, `hits_per_page` | Paging. `hits_per_page` is 1–50, and the default is 10. |

### Other inputs

| Tool | Inputs |
|---|---|
| `get_product` | `id`: a product id from `search_products` |
| `get_product_image` | `id`: a product id. `image_number`: optional, 1-based, and 1 (the main image) by default. |
| `get_net_price` | `product_ids`: 1–5 product ids from `search_products` |
| `list_categories`, `list_manufacturers` | None |

## Trade prices

Prices from the search and product tools are **list prices in USD**. To see your
**trade (net) price**, ask your assistant for net prices on specific products. It then
calls `get_net_price`.

1. **The first time, your client asks you to sign in.** It uses standard OAuth with PKCE
   and dynamic client registration. The only permission requested is `net_prices`.
2. **You sign in to MOFO and approve your assistant.** You can disconnect it at any time.
3. **Your assistant gets your trade prices.** It can look up 5 products per request and
   60 products per hour. Net prices are only available for specific products, never for
   a whole category.

If your MOFO account doesn't have approved trade access, you'll see list prices instead.
You can apply for a trade account at [ministryoffineobjects.com](https://ministryoffineobjects.com).

Quotes and orders aren't available through the MCP server yet.

## Product feed

There's also a JSON feed of active products at
`https://mcp.ministryoffineobjects.com/feed.json`, with 100 products per page.

## Privacy

See MOFO's [privacy policy](https://ministryoffineobjects.com/privacy).

## Support

Found a problem? [Open an issue](https://github.com/MinistryOfFineObjects/mofo-mcp/issues/new/choose).
Issues are public, so don't include account details or your trade prices.
