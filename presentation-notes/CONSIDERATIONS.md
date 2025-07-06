## Considerations

### Usage

Consider how we want to **use** the object.

- Do we want something more like a value object to reference?
- Do we want to be able to modify its behavior once created?

Use Data class to create Data Objects when behavior and attribute values should not change, once defined.

### Communication

What do we want to **communicate about the intent** for the object - e.g. to future contributors?

Use of `Data` communicates to future collaborators to think of the object as more like a value object - `nil`, `true`, `false`, etc.

Use of `Class`, `Struct`, `Openstruct`, for example, communicate more flexibility (in different ways).

