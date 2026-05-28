# Agent Instructions — Unity Space Sharing Sample

Focused Unity demo of MRUK's **Space Sharing** API, which lets colocated multiplayer apps synchronize real-world Scene entities and geometry between clients. Walks through the full Meta Horizon Store + Photon Realtime onboarding needed to exercise the API end-to-end.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, Horizon Store / DUC / Release Channel walkthrough, Photon configuration, and run instructions
- `ProjectSettings/ProjectVersion.txt` — Unity editor version
- `Packages/manifest.json` — Unity package versions (Meta XR Core, MRUK, Photon)
- `Assets/Oculus/OculusProjectConfig.asset` — Anchor & Space Sharing / Scene / Passthrough Support flags
- `Assets/Scripts/MRSceneManager.cs` — every key Space Sharing API call lives here
- `LICENSE` — license terms (and per-folder licenses under `Assets/ThirdParty/`)

## Quest / Horizon-specific notes

- This sample uses **Photon Realtime**, not PUN and not Fusion. Do not silently swap the networking layer.
- Space Sharing **will not work** without a Meta Horizon Store-registered app whose App ID and bundle identifier match what is installed on the device. The Data Use Checkup (User ID, User Profile) and Release Channel setup are mandatory, not optional.
- After changing anything in `OculusProjectConfig.asset`, run `Meta > Tools > Update AndroidManifest.xml` before building — otherwise the device-side feature flags and permissions go stale.
- The **first** APK uploaded to the dashboard locks the signing keystore for all subsequent uploads; do not regenerate the keystore casually.
- All Space Sharing API calls are intentionally centralized in `Assets/Scripts/MRSceneManager.cs` — read it first before adding new sharing flows.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unity answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unity-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
