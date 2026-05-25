# Rect Object

A rect is a `LayerObject` of a `layer` that represents a rectangle (or square) shape drawn by the user using the rect tool.

## JSON Structure

```json
{
  "id": "string",
  "type": "rect",
  "x": 50.0,
  "y": 80.0,
  "width": 200.0,
  "height": 120.0,
  "color": { "r": 0, "g": 0, "b": 0 },
  "strokeWidth": 10,
  "opacity": 1,
  "fill": false
}
```

## Fields

| Field         | Type       | Description                                                               |
| ------------- | ---------- | ------------------------------------------------------------------------- |
| `id`          | `string`   | Unique identifier for the rect object.                                    |
| `type`        | `"rect"`   | Always `"rect"`. Identifies this object as a rectangle.                  |
| `x`           | `number`   | X coordinate of the top-left corner (in canvas coordinates).              |
| `y`           | `number`   | Y coordinate of the top-left corner (in canvas coordinates).              |
| `width`       | `number`   | Width of the rectangle in pixels. Must be ≥ 1.                           |
| `height`      | `number`   | Height of the rectangle in pixels. Must be ≥ 1.                          |
| `color`       | `ColorRGB` | Fill/stroke color (RGB, each channel 0–255).                              |
| `strokeWidth` | `number`   | Stroke line width in pixels. Defaults to `2`. Used when `fill` is false.  |
| `opacity`     | `number`   | Opacity from `0` (transparent) to `1` (fully opaque). Defaults to `1`.  |
| `fill`        | `boolean`  | When `true`, the rectangle is drawn filled. When `false`, only the outline is drawn. Defaults to `false`. |

### `ColorRGB`

```ts
{ r: number, g: number, b: number }
```

Each channel (`r`, `g`, `b`) is an integer in the range **0–255**.

## Notes

- When `fill` is `true`, rendered via `ctx.fillRect(x, y, width, height)`.
- When `fill` is `false`, rendered via `ctx.strokeRect(x, y, width, height)` using `strokeWidth`.
- `x` and `y` always represent the top-left corner; the tool normalises negative drag directions so that dragging in any direction produces a positive-dimension rectangle.
- Holding **Shift** while drawing constrains the shape to a perfect square.
- Objects with `width < 1` or `height < 1` are not committed to the layer.
