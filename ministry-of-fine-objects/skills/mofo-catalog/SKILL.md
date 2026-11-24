---
name: mofo-catalog
description: Search and inspect the Ministry of Fine Objects trade furniture catalogue via its MCP tools. Use when a user wants furniture, lighting, or decor products, manufacturer or category filters, product specs and list prices, product photos, or signed-in trade (net) prices.
---

# Ministry of Fine Objects catalogue

Use this skill whenever someone is shopping or specifying furniture, lighting, or decor from Ministry of Fine Objects (MOFO). The plugin connects to the remote MCP server at `https://mcp.ministryoffineobjects.com/mcp`. All six tools are read-only. Quotes and ordering are not available yet.

Do not invent product names, ids, or prices. Call the tools and report what they return.

## When to use

- Finding products in the live MOFO catalogue
- Narrowing a search by category, manufacturer, price, stock, contract grade, color, shape, or dimensions
- Looking up full specs, materials, lead time, and manufacturer policies for a product
- Showing a product photo, especially when an image URL cannot be downloaded directly
- Fetching a signed-in buyer's trade (net) price for specific products already under consideration

## How to work

1. If you need valid filter values, call `list_categories` or `list_manufacturers` first and use the exact names they return.
2. Call `search_products` to find candidates. Search and product tools return list (retail) prices in USD, key dimensions in inches, stock status, an image, and a product page link.
3. Call `get_product` with a product `id` from search when the user needs full public details (specs, materials, all images, lead time, certifications, manufacturer policies).
4. Call `get_product_image` with that same `id` when you need the actual photo content through MCP. `image_number` is 1-based and defaults to the main image; later numbers follow `get_product`'s images list. Rendering the image in a card depends on the client.
5. Only after the user is considering specific products, call `get_net_price` with up to 5 of those ids. Never ask for net prices on a whole category. The client will prompt the buyer to connect their MOFO account if they are not already signed in.

## Tools

### `search_products`

Search the active catalogue by keyword, with optional filters.

- `query`: free-text, for example "green outdoor sofa" or "walnut dining table"
- `category`: one of `Seating`, `Tables`, `Casegoods`, `Lighting`, `Accessories`, `Outdoor`
- `subcategory`: exact subcategory name from `list_categories` (for example "Lounge Chairs")
- `manufacturer`: exact manufacturer name from `list_manufacturers`
- `min_list_price` / `max_list_price`: numbers, list price in USD
- `in_stock_only` / `contract_grade_only`: booleans
- `colors`: array from Beige, Black, Blue, Brown, Cream, Gold, Green, Grey, Multicolor, Natural, Orange, Pink, Purple, Red, Silver, Taupe, White, Yellow. Products without color data are excluded.
- `shapes`: array from Rectangle, Circle, Square, Oval, Irregular
- `min_width` / `max_width`, `min_depth` / `max_depth`, `min_height` / `max_height`: inches
- `page`: integer, minimum 1
- `hits_per_page`: integer 1–50, default 10

### `get_product`

Full public details for one product. Required: `id` (the product id from `search_products`).

### `get_product_image`

Retrieve an actual product photo through MCP. Required: `id`. Optional: `image_number` (1 is the main image).

### `list_manufacturers`

No arguments. Returns manufacturers currently sold on MOFO, with freight and return policies.

### `list_categories`

No arguments. Returns product categories and their subcategories, for use as `search_products` filters.

### `get_net_price`

The signed-in buyer's trade (net) price for up to 5 products. Required: `product_ids` (array of 1–5 ids from `search_products`).

This tool requires the buyer to connect their MOFO account (OAuth scope `net_prices`). The plugin does not store credentials. Buyers without an approved trade account only get list price back. Limited to 60 product lookups per hour. If the call returns unauthorized, tell the user they need to sign in with their MOFO trade account; do not invent a net price.

## Constraints

- Do not call tools that write or change data. This server does not expose any.
- Do not use net prices as a category-wide filter.
- List prices are public; net prices are account-specific and must come from `get_net_price`.
