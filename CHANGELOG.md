# Changelog

Changes to the MOFO MCP server's tools and their inputs, newest first. Version
numbers match the `version` the server reports when a client connects.

## 0.1.0

The live version when this changelog started on 2026-11-06.

- `search_products`: keyword search with filters for category, subcategory,
  manufacturer, list price, stock, contract grade, color, shape and size.
- `get_product`: full product details.
- `get_product_image`: product photos returned through MCP.
- `list_categories` and `list_manufacturers`: valid filter values.
- `get_net_price`: the signed-in buyer's trade price for up to 5 products, with OAuth sign-in.
