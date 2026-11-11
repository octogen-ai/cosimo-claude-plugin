# Cosimo for Claude

![Cosimo](assets/cosimo-icon.png)

**Intelligent fashion discovery**

Find fashion for your style, occasion, and budget with Cosimo. Discover clothing,
shoes, and accessories across brands, and explore beauty, home, and lifestyle
products. Describe what you want or share a retailer's product link to look up
details. Compare prices, materials, colors, sizes, and availability where
provided, and explore a shortlist with product images and links to Cosimo and
the retailer. Product information can change, so confirm prices and availability
with the retailer. Purchases take place on retailer websites; Cosimo does not
place orders or process payments.

## Connect

This plugin connects to Cosimo's hosted MCP server at
`https://mcp.cosimo.shop/mcp` over HTTPS with OAuth. No local server, package
installation, API key, or credential in a configuration file is required.

For Claude chat:

1. Download the plugin ZIP from the
   [GitHub release](https://github.com/octogen-ai/cosimo-claude-plugin/releases/tag/v0.1.1).
2. Open **Customize > Plugins > Add plugin > Upload plugin**, upload the ZIP,
   and enable Cosimo. This adds the `shop-with-cosimo` skill and connector reference.
3. Connect Cosimo through the plugin's connector or **Customize > Connectors**.
   If adding a custom connector, use the endpoint above. Complete the Cosimo
   consent flow by signing in or choosing guest access if offered. Guest access
   has lower limits.
4. In a new chat, select `/shop-with-cosimo` from the slash-command menu, or ask
   Claude to use Cosimo. Approve read-only tool calls when Claude asks.

A custom connector alone exposes the server's tools without installing the
shopping skill. The public GitHub package is available for testing; it is not
an approved Claude directory listing.

For local testing in Claude Code, extract the release ZIP and use Claude Code's
`--plugin-dir` option to select the extracted `cosimo` folder. See
[load a local plugin](https://code.claude.com/docs/en/plugins/create#load-a-plugin-from-a-directory-or-zip)
for the current Claude Code instructions. This package has no local server,
installation script, or executable hook; its tools run through the hosted OAuth
connector.

Complete the Cosimo OAuth connection through `/mcp`. Each person connects their
own account; never paste passwords or tokens into chat. The host's discovered
tool schemas and connection controls are authoritative.

## Try it

- Find black wool trousers for a formal occasion between $200 and $500. Show product images.
- Find a cotton dress for a summer wedding under $200. Show a few options.
- Help me compare products by material, price, and available sizes.

Search ranking does not prove every constraint matches. Cosimo's shopping skill
checks returned facts and distinguishes verified matches from partial matches.
Prices retain their returned currency. Unknown availability stays unknown, and
product-family availability does not establish stock for a particular size.

## Tools and product cards

The server provides product search and lookup; brand and store discovery,
similarity, and details; brand coverage, products, and top categories; and MCP
Apps views through `show_search_results`, `render_products`, and `show_product`.
Availability depends on the tools exposed by the connected server.

In a compatible Claude surface, Cosimo presents product cards and images through
MCP Apps. In a surface without app support, use the returned readable links and
facts. The skill avoids duplicating the app's gallery with image embeds.
Shopper links use the returned retailer redirect or Cosimo product page.
Purchases take place on merchant websites. Confirm current prices and stock
with the merchant before purchasing.

Cosimo cannot place orders, charge cards, track orders, issue refunds, edit
merchant prices or inventory, or save items to a cart or wishlist.

## Privacy Policy

The plugin sends shopping queries, supplied product URLs, and selected product
references to the declared Cosimo connector to provide its results. OAuth is
handled by Cosimo and the host. The package contains no credentials or local
executable hooks and does not request access to local files or conversation
history. The hosted service's data practices are described in the
[Privacy Policy](https://cosimo.shop/legal/privacy-policy) and
[Terms of Service](https://cosimo.shop/legal/terms-of-service).

For connection failures, reconnect through the host's MCP or connector controls.
For missing results, try a broader description; a product may not be indexed.
Respect sign-in, quota, and retry messages. Contact
[Cosimo support](https://cosimo.shop/contact) for help.

## License and release status

Copyright 2026 Octogen Systems Inc. This distributable plugin package is licensed
under [Apache-2.0](LICENSE); see [NOTICE](NOTICE). The hosted service remains
subject to Cosimo's terms. This repository is the generated public distribution;
Octogen maintains the authoring source in its monorepo. It contains the plugin
manifest, shopping skill, icon, and remote connector configuration, with no
hosted server source or credentials. Directory submission and approval remain
separate from this public release.
