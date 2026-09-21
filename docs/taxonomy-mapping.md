# Taxonomy Mapping

This specification keeps category, brand, collection, product line, and product location distinct.

## Category

The category is supplied in `product.json` as a hierarchy path:

```json
{
  "category": "Phones & Tablets > Mobile Phones > Smartphones"
}
```

The importer should compare this path against an authoritative category tree received from WooCommerce or the commerce API.

### Do not auto-create missing categories

If a category path cannot be matched, reject the product and report the requested path.

```text
CATEGORY_NOT_FOUND
Requested: Phones & Tablets > Mobile Phones > Gaming Smartphones
```

This keeps taxonomy creation under intentional catalog governance and prevents near-duplicate categories.

## Brand

`brand.txt`

```text
Example Brand
```

## Product line

`product-line.txt`

```text
Example Series
```

## Collection

`collection.txt`

```text
2026 Collection
```

## Location

`location.txt`

```text
Accra
```

The exact taxonomy names, slugs, and IDs are implementation details. The importer should resolve human-readable values to existing authoritative entities and fail safely when a required entity cannot be resolved.
