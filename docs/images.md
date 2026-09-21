# Product Images

Images live inside the product's `images/` directory.

Recommended ordering:

```text
images/
  01.jpg
  02.jpg
  03.jpg
```

The importer should sort by filename to preserve display order.

## Recommendations

- Use descriptive, consistent formats such as JPEG, PNG, or WebP.
- Avoid zero-byte files.
- Validate that each image can be decoded before upload.
- Keep image processing separate from product-data validation where possible.
- Do not infer product attributes solely from image filenames.

An implementation may rename images during processing, but the original package should remain deterministic and auditable.
