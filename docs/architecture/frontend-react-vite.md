# Frontend Architecture: React, Vite, & Tailwind CSS SPA

The frontend is a modern Single Page Application (SPA) designed for responsive interaction and instantaneous user feedback.

## Key Features

1. **Zero Full-Page Reloads:**
   - Client-side navigation via `react-router-dom` v6.
   - Dynamic deep-linking for problem solving, submissions, and contest scoreboards.

2. **Tailwind CSS & Complete SCSS Deprecation:**
   - 100% utility-first Tailwind CSS.
   - High-contrast dark theme by default (`bg-zinc-950`, `bg-zinc-900`, `border-zinc-800`).
   - Standardized verdict color system:
     - Accepted (`AC`): Emerald
     - Wrong Answer (`WA`): Rose
     - Time Limit Exceeded (`TLE`): Amber
     - Memory Limit Exceeded (`MLE`): Purple
     - Compilation Error (`CE`): Cyan
     - Queued / Grading (`QU` / `G`): Blue pulse

3. **Integrated Monaco IDE Editor:**
   - Syntax highlighting for C++, Python, Rust, C, and Java.
   - Dynamic language switching.
   - Keyboard submission hotkey (`Ctrl+Enter`).

4. **Mathematical Formula Typesetting:**
   - KaTeX rendering with LaTeX syntax support (`$...$` and `$$...$$`).
   - Powered by `react-markdown`, `remark-math`, and `rehype-katex`.

5. **Real-Time Reactive WebSockets:**
   - Custom `useLiveWebSocket` hook connecting to `/ws/live`.
   - Real-time submission verdict progression without page refresh.
