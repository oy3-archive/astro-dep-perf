This repo is a reproduction of the effect icon packs have on Astro's build
performance. Committed at best performance, uncomment to see perf cost.

Looking at verbose output, Vite via Astro seems to blindly load every single
file/dependency in the module instead of being selective

|           | Build (total `time` score) | Average Page Load \* |
| --------- | -------------------------: | -------------------: |
| Clean     |                    `1.57s` |             `5.2 ms` |
| Importing |                   `12.81s` |         `16,4686 ms` |

<small>\* User hand refreshed consecutively</small>

# Build: `time node_modules/.bin/astro build`

**Without imports**

```
18:50:34 [content] Syncing content
18:50:34 [content] Synced content
18:50:34 [types] Generated 49ms
18:50:34 [build] output: "static"
18:50:34 [build] directory: /home/jason/code/repro/astro-slow-dev/dist/
18:50:34 [build] Collecting build info...
18:50:34 [build] ✓ Completed in 56ms.
18:50:34 [build] Building static entrypoints...
18:50:34 [vite] ✓ built in 775ms
18:50:34 [build] ✓ Completed in 827ms.

 generating static routes
18:50:34 ▶ src/pages/index.astro
18:50:34   └─ /index.html (+7ms)
18:50:34 ✓ Completed in 10ms.

18:50:34 [build] 1 page(s) built in 910ms
18:50:34 [build] Complete!
node_modules/.bin/astro build  1.57s user 0.67s system 127% cpu 1.751 total
```

**With imports**

```
18:53:58 [content] Syncing content
18:53:58 [content] Synced content
18:53:58 [types] Generated 27ms
18:53:58 [build] output: "static"
18:53:58 [build] directory: /home/jason/code/repro/astro-slow-dev/dist/
18:53:58 [build] Collecting build info...
18:53:58 [build] ✓ Completed in 34ms.
18:53:58 [build] Building static entrypoints...
18:54:07 [vite] ✓ built in 9.07s
18:54:07 [build] ✓ Completed in 9.10s.

 generating static routes
18:54:07 ▶ src/pages/index.astro
18:54:07   └─ /index.html (+6ms)
18:54:07 ✓ Completed in 10ms.

18:54:07 [build] 1 page(s) built in 9.15s
18:54:07 [build] Complete!
node_modules/.bin/astro build  12.81s user 1.05s system 143% cpu 9.669 total
```

# Dev: `time node_modules/.bin/astro dev`

**Without imports**

```
18:55:38 [types] Generated 1ms
18:55:38 [vite] Re-optimizing dependencies because vite config has changed

 astro  v5.0.9 ready in 80 ms

┃ Local    http://localhost:4321/
┃ Network  use --host to expose

18:55:38 [content] Syncing content
18:55:38 [content] Synced content
18:55:38 watching for file changes...
18:55:48 [200] / 14ms
18:55:51 [200] / 2ms
18:55:52 [200] / 2ms
18:55:53 [200] / 6ms
18:55:54 [200] / 2ms
^C
node_modules/.bin/astro dev  1.38s user 0.20s system 7% cpu 19.975 total
```

**With imports**

```
18:56:37 [types] Generated 1ms

 astro  v5.0.9 ready in 64 ms

┃ Local    http://localhost:4321/
┃ Network  use --host to expose

18:56:37 [content] Syncing content
18:56:37 [content] Synced content
18:56:37 watching for file changes...
18:57:30 [200] / 15264ms
18:57:50 [200] / 17155ms
18:58:09 [200] / 16689ms
18:58:28 [200] / 16475ms
18:58:47 [200] / 16760ms
^C
node_modules/.bin/astro dev  105.40s user 6.48s system 82% cpu 2:16.20 total
```
