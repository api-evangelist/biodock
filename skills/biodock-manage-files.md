---
name: Manage the Biodock Filesystem
description: >-
  Upload files into the Biodock Filesystem and browse files and folders with
  cursor pagination.
api: openapi/biodock-public-api-openapi.yml
operations: [checkApiKey, uploadFile, listFilesystemItems]
---

# Manage the Biodock Filesystem

Programmatically populate and browse the Biodock Filesystem via the public REST
API at `https://app.biodock.ai/api/external`. Authenticate with the `X-API-KEY`
header on every request.

## Steps

1. **Verify the key** — `checkApiKey` (`GET /check`).
2. **Upload a file** — `uploadFile` (`POST /filesystem-items/upload-file`,
   `multipart/form-data`): `fileName` (required), `destinationFolder` (optional,
   created if it does not exist, defaults to root), `upload` (the binary, up to
   2 GB). Returns `{ id, __t: "File" }`.
3. **List items** — `listFilesystemItems` (`GET /filesystem-items`) with
   optional `folderId` (defaults to root), `limit`, and `startingAfter` cursor.
   Each item has `id`, `name`, `createdAt`, and `__t` (`File` or `Folder`).

## Rules

- Auth: `X-API-KEY` header on every call.
- Only local-device upload is supported via the API; S3/Google Drive/Dropbox
  import is website-only.
- Pagination is cursor-based: pass the last item's `id` as `startingAfter` to
  page forward; read `results` and `count`.
- Use `__t` to distinguish files from folders.
