# null


## Null-conditional operators

- `?.`
- `??`

string first = person?.FirstName ?? "Default";

|                   | first        |
| ----------------- | ------------ |
| person == null    | => null      |
| person != null    | => FirstName |
| FirstName == null | "Default"    |