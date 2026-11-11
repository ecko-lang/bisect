# Changelog

## 0.39.0

### Fixed

- `at` with a negative index no longer returns a wrong list
  (`at([1, 2, 3], 9, -1)` gave `[9, 3, 1, 2, 3]`). It now fails with
  `bug`/`bad_index`, as does a non-Int index.
- Passing something other than a list, or a `key` that is not a function, now
  fails with `bug`/`bad_argument` naming the function, instead of an error from
  deep inside.

### Breaking

- `at` with an index past the end of the list now fails with `bug`/`bad_index`
  instead of quietly appending. Migration: pass `len(items)` to append.
