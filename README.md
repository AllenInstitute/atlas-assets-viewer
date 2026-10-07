# atlas-assets-viewer

A single-file, static web viewer for S3 buckets laid out per the
`atlas-assets` spec. It needs
no build step, server, or AWS credentials. The page reads the bucket in the
browser with unsigned S3 requests.

**Live:** https://alleninstitute.github.io/atlas-assets-viewer/

## What it does

- Walks `<type>/<name>/<version>/…` one level at a time (`?delimiter=/`), so
  it never enumerates OME-Zarr chunks.
- Validates structure against the spec: required and optional files per
  asset type, name suffixes, and version format.
- Shows each asset's manifest and `data_description.json`, plus a browser for
  its contents with previews of text files.
- Links atlases, templates, and annotation sets into Neuroglancer and
  NeuroGlass, and asset locations into Quilt.
- Gives every view a shareable URL.
- **Export** saves the fetched tree as JSON. To reload a snapshot without
  touching S3, drop the file anywhere on the page.

Supported asset types: `atlases`, `templates`, `annotation-sets`,
`terminologies`, `coordinate-spaces`, `coordinate-transformations`.

## Usage

Enter an `s3://bucket/prefix` and press Load. Leave the region blank unless
the bucket is in an opt-in region.

To link straight to a store, set `store` in the URL hash:

```
https://alleninstitute.github.io/atlas-assets-viewer/#/?store=<bucket>/<prefix>
```

The bucket must allow public `ListBucket`/`GetObject` and return CORS
headers for `GET`.

### Hosting inside the bucket

If `index.html` is copied into the bucket and opened via its S3 URL, it reads
the bucket, region, and prefix from its own address and describes that
prefix. Requests are then same-origin, so the bucket needs no CORS setup.

## Running locally

```
python3 -m http.server 8000   # then open http://localhost:8000
```

Opening `index.html` directly (`file://`) also works.

## Limits

Validation is structural only. It does not check JSON schemas, OME-Zarr
metadata, or terminology graphs; the `atlas-assets` CLI does those checks.

## License

[MIT](LICENSE)
