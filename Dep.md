# Dependencies

Crystal UI is a single self-contained `.lua` file with **no external
modules and no `require()`s** — everything is built from vanilla Roblox
services. That said, several features rely on executor-specific globals
that aren't part of standard Luau. Here's what's needed for the *full*
experience, and what happens if something's missing.

## Roblox Services (always available, no setup needed)
- `TweenService`
- `UserInputService`
- `Players`
- `CoreGui`
- `HttpService`
- `TextService`
- `RunService`

These ship with every Roblox client — nothing to install.

## Executor Environment
Crystal UI is designed to run through a script executor (loaded via
`loadstring(game:HttpGet(...))`). It needs a reasonably modern executor
with support for:

| Global function(s)                                   | Used for                                  | Required? |
|--------------------------------------------------------|--------------------------------------------|-----------|
| `gethui()`                                              | Parenting UI outside CoreGui detection      | Optional — falls back to `CoreGui` |
| `setclipboard(text)`                                     | "Get Key" link copy, `CopyDocumentation()`  | Optional — falls back to `print()` |
| `writefile`, `readfile`, `isfile`, `isfolder`, `makefolder` | `SaveConfig` / `LoadConfig`, saved key system | Optional — features no-op with a Notify if missing |

**Every one of these is guarded with `typeof(x) == "function"` checks.**
The script will not error if your executor lacks any of them — it just
degrades gracefully (e.g. config saving tells you it's unsupported
instead of saving).

### Known-good executors
Any modern executor with a standard file-system API and `gethui` support
works fine — e.g. Synapse X, Script-Ware, KRNL, Fluxus, Wave. Older or
minimal executors that lack the file functions will still
