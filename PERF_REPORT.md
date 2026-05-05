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

TODO: Explain how `TransactionList.jsx` now renders rows through `react-window` v2 `List` with `rowComponent` and `rowProps`.

### React.memo

TODO: Explain how `TransactionRow.jsx` is wrapped in `React.memo` so unchanged rows can skip re-rendering.

### useCallback

TODO: Explain how `Transactions.jsx` now memoizes the row selection handler with `useCallback`.

### useMemo

TODO: Explain how `useTransactions.js` caches `filteredTransactions` with `useMemo`.

## Results Table

| Metric                               | Before | After | Improvement |
| ------------------------------------ | ------ | ----- | ----------- |
| Initial render time                  | TODO   | TODO  | TODO        |
| Keystroke re-render time             | TODO   | TODO  | TODO        |
| Components re-rendered per keystroke | TODO   | TODO  | TODO        |
| DOM nodes in list                    | TODO   | TODO  | TODO        |

## Reflection

TODO: Summarize which optimization produced the largest win, and note when you would avoid each technique.

## Evidence Checklist

- TODO: Add `screenshots/after-profiler.png` after the optimized profiling pass.
- TODO: Verify the report numbers against the actual DevTools Profiler recordings.
