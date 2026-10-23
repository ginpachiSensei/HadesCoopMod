# Patches to bundled dependencies

The release zip ships `HadesModCore.dll` from [TheNormalnij/HadesModLoader](https://github.com/TheNormalnij/HadesModLoader)
built with these patches on top of commit `ee0e788` ("bump version").

| Patch | What it fixes |
|---|---|
| `HadesModLoader-0001-load-lua-files-in-binary-mode.patch` | Mod Lua files saved with CRLF line endings were read in text mode with a buffer sized by `tellg()`, leaving NUL bytes at the end; Lua rejected the file and `init.lua` was skipped without a message. |

Build: clone HadesModLoader at `ee0e788`, `git am` the patch, then
`cmake -A x64 . -B build && cmake --build build --config Release --target HadesModCore`.
