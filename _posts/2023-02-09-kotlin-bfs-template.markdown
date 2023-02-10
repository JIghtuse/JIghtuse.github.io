---
layout: post
title: "Kotlin BFS template"
date: 2023-02-09 22:28:00 +0600
tags: leetcode Kotlin
---
I've used Kotlin for a month to solve [Advent of code 2022](https://adventofcode.com/),
and the next few months to solve some leetcode problems.
My experience was great so far. Kotlin allows to write concise
and safe code. I'll try to demonstrate that in this post, and to share
my BFS template.

# What are we doing?

Let's say we have a problem on 2D grid concerning some cells and
their neighbours, like [Flood Fill](https://leetcode.com/problems/flood-fill/).
To apply BFS or DFS in our solution we must take one cell at a time
and look at its neighbours. Usually it is easy to iterate over neighbours,
but we must check that neighbour exist to not fall through the grid border.
In most languages we probably use standard ifs and kinda verbose code like:
```cpp
int col = cell_x + delta_x;
int row = cell_y + delta_y;

if (0 <= col && col < grid_width &&
    0 <= row && row < grid_height)
{
    // Neighbour at (row, col) exist, we'll process it
}
```

Python allows us to be more precise and clear:
```python
col = cell_x + delta_x
row = cell_y + delta_y

if 0 <= col < grid_width and 0 <= row < grid_height:
    # Process neighbour at (row, col)
```

But still, my favourite snippet for the same logic is on Kotlin:
```kotlin
val col = cell_x + delta_x
val row = cell_y + delta_y

if (row in grid.indices && col in grid[0].indices) {
    // Process neighbour at (row, col)
}
```
We don't even need to think about exact start and end of a range!
`indices` has type `IntRange`, and `IntRange.contains` [check](https://github.com/JetBrains/kotlin/blob/34e57a45f2d4283be572137b4b497414b8833ee7/core/builtins/src/kotlin/Range.kt#L26) is almost like
the handwritten check in other languages. The only difference is that
end of range is at the last index of container, so we use `<=` for both ends.

I really liked this part in particular and how the entire solution
for Flood fill comes out: data classes, indices, sequences all combine
into a thing of beauty.
So I decided to share my BFS template for solving such problems with Kotlin.

# BFS template
```kotlin
val deltasGrid = listOf(
    0 to -1,
    0 to 1,
    -1 to 0,
    1 to 0)

data class Point(val row: Int, val col: Int)

fun bfs(grid: Array<IntArray>, start: Point) {
    fun getNeighbours(p: Point) = sequence {
        for ((deltaRow, deltaCol) in deltasGrid) {
            val row = p.row + deltaRow
            val col = p.col + deltaCol

            // probably, more checks here before yielding a neighbour
            if (row in grid.indices && col in grid[0].indices) {
                yield(Point(row, col))
            }
        }
    }

    val queue = ArrayDeque<Point>()
    queue.add(start)

    while (queue.isNotEmpty()) {
        val point = queue.removeFirst()

        // process point

        for (neighbour in getNeighbours(point)) {
            queue.add(neighbour)
        }
    }
}
```
