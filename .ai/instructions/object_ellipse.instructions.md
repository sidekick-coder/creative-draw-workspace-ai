# Ellipse Object

An ellipse is a `LayerObject` of a `layer` that represents an ellipse (or circle) shape drawn by the user using the ellipse tool. It can be rendered as a filled shape or as an outline, controlled by the `fill` option.

## JSON Structure

```json
{
  "id": "string",
  "type": "ellipse",
  "x": 50.0,
  "y": 80.0,
  "width": 200.0,
  "height": 120.0,
  "color": { "r": 0, "g": 0, "b": 0 },
  "strokeWidth": 10,
  "opacity": 1,
  "fill": true
}
```

## Fields

| Field         | Type       | Description                                                               |
| ------------- | ---------- | ------------------------------------------------------------------------- |
| `id`          | `string`   | Unique identifier for the ellipse object.                                 |
| `type`        | `"ellipse"`| Always `"ellipse"`. Identifies this object as an ellipse.                |
| `x`           | `number`   | X coordinate of the bounding box top-left corner (in canvas coordinates). |
| `y`           | `number`   | Y coordinate of the bounding box top-left corner (in canvas coordinates). |
| `width`       | `number`   | Width of the bounding box in pixels. Must be ≥ 1.                        |
| `height`      | `number`   | Height of the bounding box in pixels. Must be ≥ 1.                       |
| `color`       | `ColorRGB` | Fill/stroke color (RGB, each channel 0–255).                              |
| `strokeWidth` | `number`   | Stroke line width in pixels. Defaults to `10`. Used when `fill` is false. |
| `opacity`     | `number`   | Opacity from `0` (transparent) to `1` (fully opaque). Defaults to `1`.  |
| `fill`        | `boolean`  | When `true`, the ellipse is drawn filled. When `false`, only the outline is drawn. Defaults to `false`. |

### `ColorRGB`

```ts
{ r: number, g: number, b: number }
```

Each channel (`r`, `g`, `b`) is an integer in the range **0–255**.

## Notes

- The ellipse is centered at `{ x: x + width/2, y: y + height/2 }` and has radii `width/2` and `height/2`.
- Rendered via `ctx.ellipse(cx, cy, rx, ry, 0, 0, Math.PI * 2)`.
- When `fill` is `true`, `ctx.fill()` is used; otherwise `ctx.stroke()` is used with `strokeWidth`.
- Holding **Shift** while drawing constrains the shape to a perfect circle.
- Objects with `width < 1` or `height < 1` are not committed to the layer.
