# Workspace Folder Structure

This workspace is a **filesystem-backed storage** for the Creative Draw application. The app reads and writes all data directly from this folder tree using the [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API).

## Top-Level Layout

```
workspace-root/
├── .ai/                    # AI context and instructions (not read by the app)
│   └── instructions/
├── files/                  # Uploaded / AI-generated binary files
└── projects/               # Drawing projects
```

---

## `projects/`

Each project is stored in its own UUID-named subdirectory.

```
projects/
└── {project-id}/                     # UUID (crypto.randomUUID())
    ├── index.json                    # Project metadata
    ├── pulltimestamp.txt             # Last autoreload pull timestamp (ISO 8601)
    ├── layer-groups.json             # Array of all LayerGroup objects for this project
    └── layers/
        └── {layer-id}/               # Unique layer ID
            ├── index.json            # Layer metadata (without stroke data)
            └── data/
                └── {stroke-id}.json  # One file per stroke/draw object
```

### `projects/{id}/index.json` — Project

```json
{
  "id": "uuid",
  "name": "My Drawing",
  "width": 3508,
  "height": 2480,
  "createdAt": "2024-01-15T10:30:00.000Z",
  "updatedAt": "2024-01-15T14:22:00.000Z"
}
```

> `thumbnailSrc` is a runtime-only field. The thumbnail is stored as a regular file: `files/projects-thumbnail-{id}/content.png`.

### `projects/{id}/layer-groups.json` — Layer Groups

A JSON **array** of `LayerGroup` objects for the project.

```json
[
  {
    "id": "string",
    "project_id": "string",
    "name": "Background",
    "layer_ids": ["layer-id-1", "layer-id-2"],
    "visible": true,
    "order": 0,
    "created_at": "ISO 8601",
    "updated_at": "ISO 8601"
  }
]
```

### `projects/{id}/layers/{layer-id}/index.json` — Layer

```json
{
  "id": "string",
  "project_id": "string",
  "name": "Layer 1",
  "visible": true,
  "order": 0,
  "opacity": 1,
  "background_color": { "r": 255, "g": 255, "b": 255 },
  "created_at": "ISO 8601",
  "updated_at": "ISO 8601"
}
```

> `data` (stroke objects) is **not** stored in this file — it is loaded at runtime from the `data/` subfolder.

### `projects/{id}/layers/{layer-id}/data/{stroke-id}.json` — Stroke Object

One file per stroke/draw object on the layer. See `stroke_object_schema.md` for the full schema.

---

## `files/`

Binary files (images, uploads, AI-generated assets) are stored here.

```
files/
└── {basename}/                  # basename = filename without extension
    ├── index.json               # File metadata
    └── content.{ext}            # Raw binary content (e.g. content.png)
```

### `files/{basename}/index.json` — File Metadata

```json
{
  "id": "string",
  "filename": "my-image.png",
  "mimetype": "image/png",
  "createdAt": "ISO 8601",
  "updatedAt": "ISO 8601"
}
```

> `src` is a runtime-only blob URL. It is **not** stored in the JSON.

---

## Key Notes

- **Layer data is split**: layer metadata (`index.json`) and stroke objects (`data/*.json`) are stored separately to allow efficient partial reads and writes.
- **Layer groups are aggregated**: all groups for a project live in a single `layer-groups.json` array, not individual files.
- **Files use basename as folder name**: e.g. `files/my-image/` for a file named `my-image.png`.
- **`pulltimestamp.txt`**: written by the autoreload mechanism to track when the workspace was last synced.
