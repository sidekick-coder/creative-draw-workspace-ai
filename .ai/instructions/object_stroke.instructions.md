# Stroke Object

A stroke is a `LayerObject` of a `layer` that represents a single brush stroke drawn by the user.

## JSON Structure

```json
{
  "id": "string",
  "type": "stroke",
  "color": {
    "r": 0,
    "g": 0,
    "b": 0
  },
  "paths": [
    {
      "x": 120.5,
      "y": 340.2,
      "size": 8.4,
      "opacity": 0.85,
      "pressure": 0.6,
      "erase": false
    }
  ]
}
```

## Fields

| Field   | Type          | Description                                                    |
| ------- | ------------- | -------------------------------------------------------------- |
| `id`    | `string`      | Unique identifier for the stroke object.                       |
| `type`  | `"stroke"`    | Always `"stroke"`. Identifies this object as a stroke.        |
| `color` | `ColorRGB`    | The color used for the entire stroke (RGB, each channel 0–255). |
| `paths` | `BrushPath[]` | Array of sampled brush dot points that make up the stroke.     |

### `ColorRGB`

```ts
{ r: number, g: number, b: number }
```

Each channel (`r`, `g`, `b`) is an integer in the range **0–255**.

### `BrushPath`

Each entry in `paths` represents a single rendered dot along the stroke.

| Field      | Type      | Required | Description                                                             |
| ---------- | --------- | -------- | ----------------------------------------------------------------------- |
| `x`        | `number`  | Yes      | X position on the canvas (in canvas/layer coordinates).                |
| `y`        | `number`  | Yes      | Y position on the canvas (in canvas/layer coordinates).                |
| `size`     | `number`  | Yes      | Diameter of the brush dot at this point (in pixels).                   |
| `opacity`  | `number`  | Yes      | Opacity of the dot, from `0` (transparent) to `1` (fully opaque).     |
| `pressure` | `number`  | Yes      | Input pressure at this point, from `0` (none) to `1` (full pressure). |
| `erase`    | `boolean` | No       | When `true`, this dot erases instead of paints. Defaults to `false`.  |

## Notes

- A stroke is created in `createBrush.ts` when the user finishes drawing (pointer/mouse/touch `end` event).
- The `paths` array is built incrementally during drawing by the active brush's `draw()` function (`BrushDefinition.draw`).
- Brushes (e.g. `cd.ts`, `pencil.ts`) may vary `size` and `opacity` per dot based on pressure to simulate natural media.
- The stroke's `color` applies to all paths; individual dots do not have their own color.
- To draw a continuous stroke, the `paths` array should have closely spaced points (e.g. every 10–20ms during drawing) to ensure smooth rendering.  
