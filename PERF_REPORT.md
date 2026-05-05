# Performance Optimization Report: TxnTracker

## Baseline Observations

- TODO: Record the initial page load time.
- TODO: Record the scroll and typing jank observed before any code changes.
- TODO: Add `screenshots/baseline-profiler.png` from React DevTools Profiler.

## Root Cause Analysis

- TODO: Number of `TransactionRow` components re-rendered on one keystroke.
- TODO: Render time shown in the profiler for the baseline interaction.
- TODO: Why every row re-rendered when only the filter string changed.
- TODO: DOM node count before virtualization.

## Optimisation Plan

- Virtualization with `react-window` reduces the number of rendered DOM nodes.
- `React.memo` prevents visible rows from re-rendering when their props do not change.
- `useCallback` keeps the row click handler stable so memoized rows can skip work.
- `useMemo` caches filtered results so the search computation only runs when inputs change.

## Implementation Notes

### Virtualization

After inspection I updated `TransactionList.jsx` to use `react-window`'s
`FixedSizeList` (v2) API. The list now renders only the visible rows by:

- Passing a fixed `height` (644) and `itemSize` (84) to the `FixedSizeList`.
- Using `itemData` (memoized via `useMemo`) to provide `{ transactions, onSelect }` to each row.
- Rendering a `Row` renderer that receives `{ index, style, data }` and maps
  the index to the transaction object, forwarding `style` to the `TransactionRow`.

This replaces the previous non-standard props and ensures react-window controls
the DOM nodes created (only ~visible items are mounted at any time).

### React.memo

`TransactionRow.jsx` is exported wrapped in `React.memo`. This ensures a row
component will skip re-rendering when its props (`transaction`, `style`,
`onSelect`) are referentially equal to the previous render.

### useCallback

The parent `Transactions.jsx` memoizes the row selection handler with `useCallback`:
`const handleSelect = useCallback((id) => setSelectedId(id), []);`.
This provides a stable `onSelect` function reference so `React.memo` on rows
is not defeated by a changing handler reference.

### useMemo

`useTransactions.js` already memoizes the `filteredTransactions` computed value
with `useMemo`, using `[transactions, filter]` as dependencies. This ensures the
expensive `.filter()` runs only when the transactions array or the filter
string change.

## Results Table

| Metric                               | Before | After | Improvement |
| ------------------------------------ | ------ | ----- | ----------- |
| Initial render time                  | TODO   | TODO  | TODO        |
| Keystroke re-render time             | TODO   | TODO  | TODO        |
| Components re-rendered per keystroke | TODO   | TODO  | TODO        |
| DOM nodes in list                    | TODO   | TODO  | TODO        |

### How to measure

- Initial render time: open the app with an empty cache, start a Profiler recording immediately after load completes, and note the "Total time" in the profiler summary for the initial render pass.
- Keystroke re-render time: in the Profiler, click Record, type a single character into the search input, stop recording, and note the "Total time" for that interaction.
- Components re-rendered per keystroke: read the number of renders/components highlighted in the flame chart for that recorded interaction.
- DOM nodes in list: open Chrome DevTools → Elements, find the list container, and count the number of row DOM nodes present (before virtualization should be ~2000, after virtualization should be ~15).

Fill the table with exact numbers from these measurements and commit `screenshots/baseline-profiler.png` and `screenshots/after-profiler.png` alongside the report.

## Reflection

TODO: Summarize which optimization produced the largest win, and note when you would avoid each technique.

## Evidence Checklist

- TODO: Add `screenshots/after-profiler.png` after the optimized profiling pass.
- TODO: Verify the report numbers against the actual DevTools Profiler recordings.
