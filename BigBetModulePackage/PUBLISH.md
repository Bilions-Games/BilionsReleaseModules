# BigBet Module — finish publishing (DLL + git release)

Package scaffold is ready (`package.json`). Games consume these modules as a **git package with a
version tag** (not a local path), e.g.:
`https://github.com/Bilions-Games/BilionsReleaseModules.git?path=BetModulePackage#1.0.27`

Remaining steps (steps 1–2 need a **Unity compile** — a `.dll` can't be produced without the compiler;
step 4 push is outward-facing — do it yourself / confirm before it runs):

1. **Compile the DLL** — open `E:/Bilions/BilionsFoundationCode` in Unity. It compiles the new
   `Bilions.Foundation.BigBet` asmdef into:
   `BilionsFoundationCode/Library/ScriptAssemblies/Bilions.Foundation.BigBet.dll`
   (Console green first — no CS#### in the BigBet source). BigBet is **standalone** (asmdef
   references = []), so no `Common.dll` bundling — ship just the one DLL.

2. **Copy the DLL into this package:**
   `Library/ScriptAssemblies/Bilions.Foundation.BigBet.dll`  →  `BigBetModulePackage/Runtime/Bilions.Foundation.BigBet.dll`
   (Exactly how Bet/FreeSpin packages were built — DLL straight from ScriptAssemblies.)

3. **Commit** in `E:/Bilions/BilionsReleaseModules`:
   `git add BigBetModulePackage && git commit -m "feat: add BigBet module 1.0.0"`

4. **Tag + push** (tag convention is `<shortname>-x.y.z`, per `payline-1.1.5`, `reels-1.0.7`):
   `git tag bigbet-1.0.0 && git push origin HEAD --tags`

5. **Pin in each consuming game** — add to the game's `Packages/manifest.json` (match the sibling
   `com.bilions.*` git-URL style already there). For MBK:
   `"com.bilions.bigbetmodule": "https://github.com/Bilions-Games/BilionsReleaseModules.git?path=BigBetModulePackage#bigbet-1.0.0"`

6. **Bump `version` in `package.json` + a new `bigbet-x.y.z` tag** on every subsequent DLL change (semver).

Source of truth: `E:/Bilions/BilionsFoundationCode/Assets/BilionsFoundation/Modules/BigBet/Runtime/`.
Spec: `E:/Bilions/GameGeneratorAgent/module_specs/BigBet.md`.
