# Line Intersection

A small Rust program that reads line segments from a text file, treats the **first** segment as a fixed “main” line, and checks whether each subsequent segment line intersects that main line. When an intersection exists and lies **on** the segment (not only on the infinite line), the program prints the intersection coordinates.

## Features

- Parses segment endpoints from a plain-text input file
- Builds implicit line equations \(Ax + By - C = 0\) from two points
- Detects intersections of two lines (parallel lines are skipped via a determinant threshold)
- Verifies that the intersection point lies on the finite segment, not just on its supporting line

## Requirements

- [Rust](https://www.rust-lang.org/tools/install) (edition 2021; no external crates)

## Input format

The program reads `./text.txt` from the **project root** (the directory you run `cargo run` from).

Each non-empty line defines one segment with two endpoints:

```
x1,y1 x2,y2
```

- Coordinates are comma-separated; endpoints are separated by a space.
- Floating-point values are supported.

**Example** (`text.txt`):

```
1.5,2.7 2.896,3
2.68,3 4,8.6666661
```

- **Line 1** — main line (reference for all checks)
- **Lines 2+** — segments tested against the main line

## Build and run

From the repository root:

```bash
cargo build
cargo run
```

Release build (optional):

```bash
cargo run --release
```

Ensure `text.txt` exists in the current working directory (typically the project root when using Cargo).

## Output

For each segment after the first, the program prints:

1. Whether the infinite lines intersect (`intersect_answer: x y true/false`)
2. Either the intersection point on that segment, or a message that no intersection on the segment was found

Example messages:

- Intersection on segment: `N line (x1,y1) и (x2,y2). Intersection point is (x,y)!`
- No intersection on segment: `N line have coordinates (...) and (...). Doesn't have an intersection point.`

## Algorithm overview

1. **Line coefficients** — From endpoints \((x_1, y_1)\) and \((x_2, y_2)\), compute \((A, B, C)\) for \(Ax + By - C = 0\).
2. **Line intersection** — Solve the \(2 \times 2\) linear system; if \(|A_1 B_2 - A_2 B_1| < 10^{-4}\), lines are treated as parallel.
3. **On-segment test** — Check collinearity of the candidate point with the segment, then whether its coordinates lie within the segment’s bounding interval.
