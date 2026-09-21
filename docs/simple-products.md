# Simple Products

A simple product has one sellable configuration and therefore does not require `variations.json`.

## Required package

```text
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
```

`product.json` must contain `"productType": "simple"`.

## Recommended fields

```json
{
  "productType": "simple",
  "name": "Example Wireless Speaker",
  "sku": "EXAMPLE-SPK-001",
  "category": "Electronics > Audio > Bluetooth Speakers",
  "status": "draft",
  "regularPrice": null,
  "salePrice": null,
  "stockStatus": "instock",
  "manageStock": false,
  "stockQuantity": null,
  "weight": null
}
```

Commerce-specific fields may be omitted when pricing, inventory, or seller offers are controlled by another system.

## Attributes

Simple products may still have attributes. Set `variation` to `false` for every attribute.
