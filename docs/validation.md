# Validation

Validation should happen before any product is created or modified.

## Suggested validation phases

### 1. Package validation

Check that the product is under the correct product-type folder and contains all required files.

### 2. JSON validation

Parse JSON and validate against the schemas in `/schemas`.

### 3. Cross-file validation

Examples:

- `simple-product/` must contain `productType: simple`.
- `variable-product/` must contain `productType: variable`.
- A variable product must include `variations.json`.
- A simple product should not include active variation definitions.
- Variation attributes must exist in `attributes.json`.

### 4. Taxonomy validation

Resolve category, brand, collection, product line, and location against the catalog's authoritative taxonomies.

### 5. Uniqueness validation

Check parent and variation SKUs against both the package and existing catalog.

### 6. Image validation

Verify supported extension, non-zero file size, and deterministic ordering.

### 7. API preflight

Confirm that required remote resources still exist immediately before creation.

## Error shape

An importer can return machine-readable errors like:

```json
{
  "code": "CATEGORY_NOT_FOUND",
  "product": "Example Smartphone",
  "field": "category",
  "value": "Phones > Experimental Devices",
  "message": "The requested category does not exist in the current catalog."
}
```

Batch imports should report all validation failures that can be determined safely rather than stopping at the first malformed product.
