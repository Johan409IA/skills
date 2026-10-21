# Verification with Playwright CLI

Goal: see the implemented interface in a real browser, at three viewport sizes and in its key states, and hand the user screenshots. This is a check for visible breakage and obvious deviations, not a pixel comparison with the reference.

Playwright CLI is the `playwright-cli` command from the `@playwright/cli` package. Command names and flags can change between versions, so when something fails, run `playwright-cli --help` and use what that version reports instead of guessing.

## 1. Check that it is installed

```bash
playwright-cli --version
# or, for a local install:
npx --no-install playwright-cli --version
```

- Installed: continue.
- Missing: stop and ask the user to install it (`npm install -g @playwright/cli@latest` is the documented way), or offer to verify with another tool that can take screenshots. Wait for their choice. Do not install global packages silently.

## 2. Start the app

Read `package.json` (and equivalent files) to find the dev script: `npm run dev`, `ng serve`, `npm start`, `pnpm dev`, and so on. Start it in the background and wait until the URL responds. Note the port from the dev server output instead of assuming one.

If a dev server already responds on the project's port, reuse it. Do not start a second one, and do not stop it in step 7, because you did not start it.

If the dev server fails to start (missing dependencies, build error, port taken), report the exact error to the user. Do not fix unrelated build problems. Verify only what can be verified, and say what could not be.

Start the **backend** only when the interface needs real data to look right (lists, tables, dashboards, authenticated views). If the frontend renders fine without it, or the empty and loading states are what you are checking, leave it off. When you do start it, find its dev script the same way, and check whether it needs a database or environment variables the user has to provide. If it does and they are missing, say so and verify what you can without it.

Keep the process IDs of every server you start. You need them in step 7.

## 3. Choose the browser

Try Chrome first:

```bash
playwright-cli open --browser=chrome http://localhost:<port>
```

If Chrome is not available (the command fails saying the browser is missing), use Edge:

```bash
playwright-cli open --browser=msedge http://localhost:<port>
```

Tell the user in the final report which browser was used.

## 4. Check the three viewports

Suggested sizes (adjust to the project's own breakpoints if known):

| Viewport | Size       |
| -------- | ---------- |
| Mobile   | 390 x 844  |
| Tablet   | 768 x 1024 |
| Desktop  | 1440 x 900 |

```bash
playwright-cli resize 390 844
playwright-cli screenshot --filename=.playwright-cli/mobile.png
playwright-cli resize 768 1024
playwright-cli screenshot --filename=.playwright-cli/tablet.png
playwright-cli resize 1440 900
playwright-cli screenshot --filename=.playwright-cli/desktop.png
```

Look for horizontal overflow, clipped or overlapping elements, text below readable size and elements that disappeared at a breakpoint.

## 5. Check the states

Use `playwright-cli snapshot` to get element references, then interact with them. Capture the states that apply to the redesigned piece:

- **Hover**: `playwright-cli hover <ref>`, then screenshot.
- **Focus**: `playwright-cli press Tab` to move focus, then screenshot and confirm the focus indicator is clearly visible.
- **Empty**: a view with no data, or the backend off, if the interface has an empty state.
- **Error**: trigger validation (for example submit an empty form with `click`) or a failed request.
- **Loading**: capture right after navigation or an action, or slow the request if the CLI supports it.

Not every state applies to every component. Check the ones the spec lists and say which were not applicable.

Name screenshots descriptively, for example `navbar-hover-desktop.png`, `form-error-mobile.png`.

## 6. Where screenshots go

By default in `.playwright-cli/`, inside the project. Pass the path in `--filename` explicitly so you know where files land. If the user named another folder earlier in the conversation, save there instead.

If `.playwright-cli/` is not in `.gitignore`, mention it in the report. Do not edit `.gitignore` without asking.

## 7. Wrap up

Close the browser:

```bash
playwright-cli close
```

Then stop every dev server you started, using the process IDs you kept. On Windows use `taskkill /PID <pid> /T /F` (the `/T` also ends child processes spawned by `npm`). On macOS and Linux use `kill <pid>`. Do not leave servers running and do not kill processes you did not start.
