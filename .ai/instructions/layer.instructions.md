# Layer Object

A layer belongs to a project and can optionally be organized into layer groups.

## JSON Structure

```json
{
  "id": "string",
  "project_id": "string",
  "name": "Layer 1",
  "visible": true,
  "order": 0,
  "opacity": 1,
  "type": null,
  "parent_id": null,
  "background_color": { "r": 255, "g": 255, "b": 255 },
  "created_at": "2024-01-15T10:30:00.000Z",
  "updated_at": "2024-01-15T14:22:00.000Z"
}
```

## Fields

| Field              | Type           | Required | Default      | Description                                                         |
| ------------------ | -------------- | -------- | ------------ | ------------------------------------------------------------------- |
| `id`               | `string`       | Yes      | —            | Unique identifier for the layer.                                    |
| `project_id`       | `string`       | Yes      | —            | ID of the project this layer belongs to.                            |
| `name`             | `string`       | Yes      | `"New Layer"`| Display name of the layer.                                          |
| `visible`          | `boolean`      | No       | `true`       | Whether the layer is visible on the canvas.                         |
| `order`            | `number`       | No       | `0`          | Render order. Higher values render on top.                          |
| `opacity`          | `number`       | No       | `1`          | Layer-level opacity, from `0` (transparent) to `1` (fully opaque). |
| `type`             | `string\|null` | No       | `null`       | Optional layer type tag for custom behavior.                        |
| `parent_id`        | `string\|null` | No       | `null`       | ID of the `LayerGroup` this layer belongs to, or `null` if none.   |
| `background_color` | `ColorRGB`     | No       | `undefined`  | Optional solid background color for the layer.                      |
| `created_at`       | `string`       | No       | —            | ISO 8601 timestamp of creation.                                     |
| `updated_at`       | `string`       | No       | —            | ISO 8601 timestamp of last update.                                  |

### `ColorRGB`

```ts
{ r: number, g: number, b: number }
```

Each channel (`r`, `g`, `b`) is an integer in the range **0–255**.

## Notes

- Layers are rendered in ascending `order`, so a layer with `order: 1` renders above a layer with `order: 0`.
- `opacity` is applied to the entire layer composite, not to individual objects within it.
- `parent_id` links this layer to a `LayerGroup` for organizational purposes; the group's `layer_ids` array should also include this layer's `id`.
