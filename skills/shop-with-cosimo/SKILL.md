---
name: shop-with-cosimo
description: Find and compare products with Cosimo when the user describes a shopping need, provides a merchant product link, or asks to see product images. Use for fashion, beauty, home, and lifestyle product discovery and curated shortlists.
---

# Shop with Cosimo

Use Cosimo's connected product tools to turn the user's preferences into a useful shortlist. Keep recommendations concise and support product claims with returned facts.

## Discover and verify

1. Capture the relevant category, intended wearer, color, style, occasion, materials, size, budget, and currency from the request. Ask only about details that materially affect the recommendation and cannot be inferred. Start with useful search results when enough information is available.
2. Call `product_search` with a natural-language query containing the user's constraints. Request a manageable number of results; the server accepts 1–20 per call. If the current schema exposes `cursor`, continue with the returned `nextCursor` and the same query when more options are needed.
3. Check the returned descriptions, details, prices, currency, and stock before recommending. Search ranking is not proof that every constraint matches. For an exact requirement such as 100% wool, recommend a verified match only when the returned material composition supports it. Identify unverified requirements or partial matches plainly. Do not infer composition from a title alone.
4. Use `product_lookup` for a supplied merchant product-page URL, or to verify a shortlisted product's details and variants. Pass the merchant `productUrl` from a returned product when resolving an existing result. Do not use category or homepage URLs. If a product is not indexed, explain the result and search by description; do not fabricate a lookup.

## Explore brands and stores

When the user asks about brands or retailers rather than specific products, use `brand_search` or `store_search` with a description, and `brand_similar` or `store_similar` for alternatives to ones they name. Use `brand_details`, `brand_top_categories` and `store_details` for what a brand or store carries. Brands are not stores. `brand_products` returns the same product cards as `product_search`; to apply style, color, or budget constraints, use `product_search` with the brand in the query instead. Coverage counts describe the observed assortment, not live availability.

## Show the shortlist

When product presentation is useful, especially when the user asks for images, call `render_products` after selecting and verifying the products. Pass 1–12 merchant URLs in recommendation order, an optional short title, and a concise factual `reason` per product. A `variant_sku` must come from lookup. The renderer resolves product facts itself; never supply invented prices, availability, images, or HTML.

Use this same flow for a single product and for follow-up detail replies that include images: look up the returned merchant `productUrl`, then call `render_products` with that one URL. Cosimo owns product image presentation. After calling `render_products`, do not duplicate its gallery with Markdown image embeds, HTML, or custom product cards.

Search results include CDN-cached product images. A text-only tool result or missing display confirmation is not evidence that images are unavailable and does not justify adding image embeds. Keep accompanying prose concise: explain the recommendation and relevant tradeoffs, using verified facts and returned product links without repeating the full card contents. In hosts without MCP Apps support, provide readable product links and facts without embedding images. Never invent or rewrite image URLs.

When you link a product, use its returned `outboundUrl`, which opens the retailer's page through Cosimo's redirect. If `outboundUrl` is null, link the returned `cosimoUrl` (the product's Cosimo page) instead. The merchant `productUrl` identifies the product for `product_lookup` and `render_products`; do not show it as the shopper's link. Do not construct or rewrite any of these URLs. Keep each price paired with its currency. Unknown availability remains unknown; product-family availability or price does not establish a particular size or SKU's availability or price.

## Respect the service boundaries

Cosimo provides discovery, product details, and links. These tools cannot place orders, process payments, track orders, issue refunds, edit merchant inventory, or save items to a user's cart or wishlist. Explain unsupported requests without claiming an action occurred. Follow the server's sign-in, quota, and retry guidance, and stop automatic retries when instructed. Do not request passwords or access tokens in chat or include them in tool arguments.
