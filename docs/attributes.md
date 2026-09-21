# Attributes

Attributes describe structured product characteristics.

```json
{
  "attributes": [
    {
      "name": "Storage",
      "visible": true,
      "variation": true,
      "values": ["128GB", "256GB"]
    }
  ]
}
```

## Field meanings

- `name`: Human-readable attribute name.
- `visible`: Whether the attribute is intended to appear on the product page.
- `variation`: Whether WooCommerce should use the attribute to construct variations.
- `values`: Allowed values for the product.

## Normalization

Importers may trim whitespace and compare values case-insensitively during validation, but should preserve the canonical display value selected by the catalog system.

Avoid silently turning similar values such as `Space Gray`, `Space Grey`, and `Grey` into separate taxonomy terms unless the catalog explicitly distinguishes them.
