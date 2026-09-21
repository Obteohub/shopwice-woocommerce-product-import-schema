# Variable Products

A variable product represents one canonical product with multiple purchasable combinations such as storage, colour, size, or capacity.

## Required package

```text
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

`product.json` must contain `"productType": "variable"`.

## Variation rules

- Every key used by a variation must correspond to an attribute with `variation: true`.
- Every variation value must exist in that attribute's `values` array.
- Every variation SKU should be unique when SKUs are supplied.
- Duplicate attribute combinations should fail validation.
- A variation does not need to repeat parent-level information unless it differs.

Example:

```json
{
  "variations": [
    {
      "attributes": {
        "Storage": "128GB",
        "Colour": "Black"
      },
      "sku": "EXAMPLE-PHONE-128-BLK",
      "regularPrice": null,
      "stockStatus": "instock"
    }
  ]
}
```
