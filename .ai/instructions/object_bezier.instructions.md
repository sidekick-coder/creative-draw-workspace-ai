# Bezier Object

A bezier is a `LayerObject` of a `layer` that represents a smooth curved path drawn by the user using the bezier tool.

## JSON Structure

```json
{
  "id": "string",
  "type": "bezier",
  "start": { "x": 100.0, "y": 200.0 },
  "segments": [
    {
      "c1": { "x": 150.0, "y": 180.0 },
      "c2": { "x": 230.0, "y": 260.0 },
      "end": { "x": 300.0, "y": 250.0 }
    }
  ],
  "color": { "r": 0, "g": 0, "b": 0 },
  "strokeWidth": 10,
  "opacity": 1
}
```

## Fields

| Field         | Type               | Description                                                         |
| ------------- | ------------------ | ------------------------------------------------------------------- |
| `id`          | `string`           | Unique identifier for the bezier object.                            |
| `type`        | `"bezier"`         | Always `"bezier"`. Identifies this object as a bezier path.        |
| `start`       | `Point`            | The starting point of the path (position of the first anchor).     |
| `segments`    | `BezierSegment[]`  | Array of cubic bezier segments that form the path.                  |
| `color`       | `ColorRGB`         | Stroke color (RGB, each channel 0–255).                             |
| `strokeWidth` | `number`           | Width of the stroke line in pixels. Defaults to `2`.               |
| `opacity`     | `number`           | Opacity from `0` (transparent) to `1` (fully opaque). Defaults to `1`. |

### `Point`

```ts
{ x: number, y: number }
```

Canvas coordinates (in layer space).

### `ColorRGB`

```ts
{ r: number, g: number, b: number }
```

Each channel (`r`, `g`, `b`) is an integer in the range **0–255**.

### `BezierSegment`

Each segment describes one cubic bezier curve from the previous anchor to the next.

| Field | Type    | Description                                                            |
| ----- | ------- | ---------------------------------------------------------------------- |
| `c1`  | `Point` | First control point (outgoing handle of the start anchor).            |
| `c2`  | `Point` | Second control point (incoming handle mirror of the end anchor).      |
| `end` | `Point` | End position of this segment (position of the next anchor).           |

## Notes

- The path starts at `start` and continues through each `segments[i].end` point.
- Each segment is rendered using `ctx.bezierCurveTo(c1.x, c1.y, c2.x, c2.y, end.x, end.y)`.
- `c2` of a segment is computed as the mirror of the end anchor's outgoing handle: `{ x: 2*pos.x - cp.x, y: 2*pos.y - cp.y }`.
- When `c1` equals the segment's start position (no drag), the anchor is treated as a corner point.
- The tool uses a double-click or `Enter` key to commit the path; `Escape` cancels the current drawing.
- To produce a straight line segment, set both `c1` and `c2` equal to the start and end positions respectively.
