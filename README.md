# WooCommerce Product Import Schema

An open, folder-based specification for preparing and validating ecommerce product data before importing it into WooCommerce.

This repository provides a practical structure for **simple products**, **variable products**, **attributes**, **variations**, **brands**, **product collections**, **product lines**, **product locations**, **descriptions**, **images**, and **category validation**.

The specification was developed from catalog infrastructure work at [Shopwice](https://shopwice.com/), an ecommerce marketplace in Ghana.

## Why this project exists

Large WooCommerce catalogs become difficult to maintain when product information arrives in inconsistent spreadsheets, folders, image sets, supplier documents, and free-form descriptions.

This project treats each product as a self-contained package. An importer can validate the package first, map its taxonomy against an existing catalog, and only then send the product to WooCommerce through the appropriate API or middleware layer.

```text
ZIP or root folder
      |
      v
Product-type folders
      |
      v
One folder per product
      |
      v
Schema + content validation
      |
      v
Existing taxonomy matching
      |
      v
Importer / commerce API
      |
      v
WooCommerce
```

## Goals

- Keep product data portable and human-readable.
- Support both simple and variable WooCommerce products.
- Separate descriptive content from structured product data.
- Make variation definitions explicit.
- Keep brand, product line, collection, and location independent.
- Validate categories before import instead of silently creating new categories.
- Make failed imports explainable to catalog teams.
- Provide a format that can be generated manually, by scripts, or by AI-assisted catalog workflows.

## Repository structure

```text
shopwice-woocommerce-product-import-schema/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── SECURITY.md
├── docs/
│   ├── simple-products.md
│   ├── variable-products.md
│   ├── attributes.md
│   ├── taxonomy-mapping.md
│   ├── validation.md
│   └── images.md
├── examples/
│   ├── simple-product/
│   │   └── Example Wireless Speaker/
│   │       ├── product.json
│   │       ├── attributes.json
│   │       ├── brand.txt
│   │       ├── collection.txt
│   │       ├── product-line.txt
│   │       ├── location.txt
│   │       ├── short.txt
│   │       ├── long.txt
│   │       └── images/
│   │           └── README.md
│   └── variable-product/
│       └── Example Smartphone/
│           ├── product.json
│           ├── attributes.json
│           ├── variations.json
│           ├── brand.txt
│           ├── collection.txt
│           ├── product-line.txt
│           ├── location.txt
│           ├── short.txt
│           ├── long.txt
│           └── images/
│               └── README.md
└── schemas/
    ├── product.schema.json
    ├── attributes.schema.json
    └── variations.schema.json
```

## Canonical package layout

A root import package contains product-type folders. Each product then has its own directory.

```text
catalog-import/
  simple-product/
    Product Name/
      product.json
      attributes.json
      brand.txt
      collection.txt
      product-line.txt
      location.txt
      short.txt
      long.txt
      images/

  variable-product/
    Product Name/
      product.json
      attributes.json
      variations.json
      brand.txt
      collection.txt
      product-line.txt
      location.txt
      short.txt
      long.txt
      images/
```

## File responsibilities

| File | Purpose |
|---|---|
| `product.json` | Core product identity, product type, SKU, category path, status, and optional commerce fields |
| `attributes.json` | Product attributes and whether each attribute is used for variations |
| `variations.json` | Variable-product combinations and variation-level data |
| `brand.txt` | Brand only |
| `collection.txt` | Product collection only |
| `product-line.txt` | Product line/family only |
| `location.txt` | Product location only |
| `short.txt` | Short product description |
| `long.txt` | Full product description |
| `images/` | Product images in display order |

Taxonomy files remain separate intentionally. A brand is not a collection, a product line is not a brand, and location is not a product category.

## Minimum simple-product example

`product.json`

```json
{
  "productType": "simple",
  "name": "Example Wireless Speaker",
  "sku": "EXAMPLE-SPK-001",
  "category": "Electronics > Audio > Bluetooth Speakers",
  "status": "draft"
}
```

`attributes.json`

```json
{
  "attributes": [
    {
      "name": "Connectivity",
      "visible": true,
      "variation": false,
      "values": ["Bluetooth"]
    },
    {
      "name": "Colour",
      "visible": true,
      "variation": false,
      "values": ["Black"]
    }
  ]
}
```

## Minimum variable-product example

`product.json`

```json
{
  "productType": "variable",
  "name": "Example Smartphone",
  "sku": "EXAMPLE-PHONE",
  "category": "Phones & Tablets > Mobile Phones > Smartphones",
  "status": "draft"
}
```

`attributes.json`

```json
{
  "attributes": [
    {
      "name": "Storage",
      "visible": true,
      "variation": true,
      "values": ["128GB", "256GB"]
    },
    {
      "name": "Colour",
      "visible": true,
      "variation": true,
      "values": ["Black", "Blue"]
    }
  ]
}
```

`variations.json`

```json
{
  "variations": [
    {
      "attributes": {
        "Storage": "128GB",
        "Colour": "Black"
      },
      "sku": "EXAMPLE-PHONE-128-BLK"
    },
    {
      "attributes": {
        "Storage": "256GB",
        "Colour": "Blue"
      },
      "sku": "EXAMPLE-PHONE-256-BLU"
    }
  ]
}
```

## Category validation rule

The importer should **not automatically create a category that does not exist**.

Before import:

1. Fetch or receive the current category tree from WooCommerce or the commerce API.
2. Normalize the supplied path for matching without changing its intended hierarchy.
3. Attempt an exact hierarchy match.
4. If no valid category exists, fail that product before creation.
5. Return a clear validation message so the category can be created by an authorized catalog administrator.

Example error:

```text
CATEGORY_NOT_FOUND
Product: Example Smart Doorbell
Requested category: Smart Home > Video Doorbells
Action: Create or map the category in the commerce backend, then retry the import.
```

This approach prevents import scripts from creating duplicate or malformed category trees.

See [docs/taxonomy-mapping.md](docs/taxonomy-mapping.md).

## Validation order

A robust importer should validate in this order:

1. Folder structure
2. Required files
3. JSON syntax
4. JSON Schema conformance
5. Product type consistency
6. Required product fields
7. Taxonomy values
8. Attribute definitions
9. Variation combinations
10. Duplicate SKU checks
11. Image presence and file type
12. Remote/API preflight checks
13. Import

See [docs/validation.md](docs/validation.md).

## Security and privacy

Never commit:

- API keys
- access tokens
- refresh tokens
- WooCommerce consumer secrets
- private supplier pricing
- vendor/customer personal information
- internal server addresses
- production database credentials
- private webhooks

Use environment variables or a secrets manager for runtime credentials.

## SEO note

This format intentionally does not require SEO title or meta-description fields. Those may be handled by a dedicated SEO layer or WordPress SEO plugin. Importers may extend the schema if their environment requires explicit SEO metadata.

## Implementation notes

This specification does not prescribe PHP, Node.js, Python, or another runtime. The same package can be consumed by any service that can:

- read folders and text files,
- parse JSON,
- validate against JSON Schema,
- map taxonomies,
- call a commerce API,
- report validation errors clearly.

## Related Shopwice work

The structure was informed by product-catalog engineering work at [Shopwice](https://shopwice.com/). You can adapt the specification to your own WooCommerce installation without using Shopwice services.

## License

Released under the MIT License. See [LICENSE](LICENSE).
