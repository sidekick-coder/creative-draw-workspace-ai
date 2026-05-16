# Project Object

A project is the top-level container for all drawing work. It holds metadata and references to its layers and layer groups, which are stored separately.

## JSON Structure

```json
{
  "id": "string",
  "name": "My Drawing",
  "createdAt": "2024-01-15T10:30:00.000Z",
  "updatedAt": "2024-01-15T14:22:00.000Z"
}
```

## Fields

| Field       | Type     | Description                                              |
| ----------- | -------- | -------------------------------------------------------- |
| `id`        | `string` | Unique identifier for the project.                       |
| `name`      | `string` | Display name of the project.                             |
| `createdAt` | `string` | ISO 8601 timestamp of when the project was created.      |
| `updatedAt` | `string` | ISO 8601 timestamp of the last update.                   |

## Notes

- `thumbnailSrc` is a runtime-only field populated from the drive/storage. It is **not** persisted in the JSON.
- The thumbnail filename is derived from the id: `projects-thumbnail-{id}.png`.
- Layers and layer groups are stored separately and reference the project via `project_id`.
- In the filesystem storage backend, each project lives at `projects/{id}/index.json`.
