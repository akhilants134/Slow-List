# TxnTracker Performance Challenge

Welcome to the TxnTracker React Performance Engineering challenge. Your task is to identify and resolve critical performance bottlenecks in this high-volume transaction dashboard.

## Initial Setup

1. Install dependencies:

   ```bash
   npm install
   ```

2. Run the development server:

   ```bash
   npm run dev
   ```

3. Open the application in your browser.

## The Challenge

The dashboard currently renders 2,000 transaction records. While it looks polished, it is architecturally broken from a performance standpoint. You will notice significant lag when:

- Typing in the search filter.
- Selecting transactions from the list.

## Your Task

1. **Profile**: Use the React DevTools Profiler to record a baseline of the slow interactions.
2. **Optimize**: Systematically apply four optimization techniques (Memoization, Function stability, Computed state caching, and Virtualization).
3. **Report**: Document your findings and improvements in `PERF_REPORT.md`.

Good luck!

## Capturing Profiler Screenshots

When you record profiler sessions, save the screenshots into a `screenshots/` folder at the repo root with these filenames:

- `screenshots/baseline-profiler.png` — the flame chart captured before any code changes.
- `screenshots/after-profiler.png` — the flame chart captured after your optimisations.

Requirements:

- Capture the flame chart showing component names and total render time at the top.
- Commit the baseline screenshot before committing any code changes that affect rendering.
- Use the same interaction when recording both profiles (type one character in the search box).

Example workflow:

```bash
# 1. Start dev server
npm run dev

# 2. Open the app, open React DevTools → Profiler
# 3. Click Record, type one character in the search box, stop recording
# 4. Take a screenshot and save as screenshots/baseline-profiler.png
# 5. Commit the screenshot before changing code
git add screenshots/baseline-profiler.png
git commit -m "perf: add baseline profiler screenshot"
```

Then proceed with the optimisations and capture `screenshots/after-profiler.png` the same way.
