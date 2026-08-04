# bisect - Ecko Std Lib Package

Binary search over a sorted list: insertion points, membership, and
order-preserving insert. The shape of Python's `bisect`, which is the reference
most people already have in mind.

Pure computation - no capabilities.

## Install

```bash
ecko get github.com/ecko-lang/bisect
```

```ecko
import bisect
```

## Usage

```ecko
scores = [10, 20, 20, 20, 30]

bisect.left(scores, 20)          # 1 - the start of the run of 20s
bisect.right(scores, 20)         # 4 - one past the end of it
bisect.count(scores, 20)         # 3
bisect.has(scores, 30)           # true, in O(log n)

bisect.insert_right(scores, 25)  # a new list, still sorted
```

Searching records by a field:

```ecko
by_age = fn(p) p.age
bisect.left_by(people, 35, by_age)
bisect.insert_by(people, { name: "Barbara", age: 38 }, by_age)
```

## API

| function | what it does |
|---|---|
| `left(items, x)` | leftmost insertion point; the index of the first `x` if present |
| `right(items, x)` | rightmost insertion point; one past the last `x` if present |
| `left_by(items, x, key)` | `left`, comparing `key(item)` |
| `right_by(items, x, key)` | `right`, comparing `key(item)` |
| `insert_left(items, x)` | a new list with `x` inserted at `left` |
| `insert_right(items, x)` | a new list with `x` inserted at `right` |
| `insert_by(items, item, key)` | a new list with `item` placed by its key |
| `has(items, x)` | membership, `O(log n)` |
| `index(items, x)` | index of the first `x`, or `null` |
| `count(items, x)` | how many copies of `x` |
| `at(items, x, index)` | splice `x` in at a known index |

## Notes

**The list must already be sorted.** Nothing here checks that, because checking
would cost the `O(n)` the binary search exists to avoid. An unsorted list gives
a meaningless answer, quickly.

**`left` versus `right` only differ on duplicates.** With no equal element they
return the same index. When there is a run of equals, `left` lands before it and
`right` after, which is why `right - left` counts the copies.

**Inputs are never mutated.** `insert_*` returns a new list; Ecko collections are
copy-on-write, so the copy happens once and only when needed.

**`has` beats the `contains` builtin on a sorted list** - `O(log n)` against
`O(n)`. On an unsorted list `contains` is the correct choice and this is not.

## Testing

```bash
ecko test
```

Offline and deterministic.

## License

MIT
