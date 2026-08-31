# Skills only

**Package:** `PartnerDaysExtended_skills.zip` (34 KB)

No code, no database, no assets — just three agent skills. The agent builds the
solution and downloads everything else from the Dynamicweb doc site while you
watch.

That makes this the most interesting package to run in front of an audience,
and the slowest route to a working site. It needs internet access from start to
finish. If you want something that just works offline, take one of the
[solution](../solution-demo-data/) or [app](../app-demo-data/) packages
instead.

This is also the only package without the Dynamicweb MCP server preinstalled —
there's nothing to install it into until the agent has scaffolded the solution.

> [!IMPORTANT]
> **Create the folder on the C: drive** — for example
> `C:\Projects\PartnerDaysExtended`.
>
> Do **not** put it inside OneDrive, Dropbox or any other synced folder. Sync
> locks files while the build and Dynamicweb are using them, and the failures
> that follow look like something else entirely — you'll waste an hour chasing
> the wrong problem. Keep the path short while you're at it.

## What's in the package

| Skill | What it does |
|---|---|
| `scaffold-dw10-solution` | The one you ask for. Orchestrates the other two. |
| `dynamicweb10-scaffold` | Creates the `.slnx` solution and the host project referencing `Dynamicweb.Suite`. |
| `scaffold-swift-project` | Creates the files project, downloads the latest Swift release, imports the database and wires up the connection. |

Each skill ships twice — under `.claude/skills/` for Claude Code and
`.agents/skills/` for Copilot and Codex.

## 1. Install the prerequisites

- **[.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)** — check
  with `dotnet --list-sdks`; you need a 10.x entry.
- **A SQL Server** — a local instance, a Docker container or a remote server.
  LocalDB does **not** work; Dynamicweb needs a real instance. If you have
  Docker but no SQL Server, that's fine — the skill will offer to create a
  container for you.
- **An agent** — Claude Code, GitHub Copilot or OpenAI Codex.
- **Internet access**, throughout. Unlike the other packages the assets aren't
  bundled — they're fetched from <https://doc.dynamicweb.com/downloads/swift>
  as the agent works.

## 2. Create an empty folder and unpack into it

This is the folder the finished solution will live in, so pick the name you
actually want — under `C:\Projects\`, for example. **Not** OneDrive, Dropbox or
any synced folder — see the warning at the top of this page.

After unpacking you should see `.claude/`, `.agents/` and `readme.md` — and
nothing else. If you see a `PartnerDaysExtended.Host` folder, you unpacked the
wrong zip.

## 3. Open that folder in your agent

The skills are only found when the unpacked folder is the working directory.
Opening a parent folder will not find them.

## 4. Ask it to scaffold

**Give the solution a name in your request.** If you don't, the agent will get
partway through and stop to ask you — harmless, but it interrupts the flow.

```
Scaffold a Dynamicweb 10 solution called PartnerDaysExtended.
```

From here the agent works through the whole sequence: creating the solution and
host project, adding the `Dynamicweb.Suite` reference, creating the files
project, downloading the latest Swift release, importing the database and
writing the connection details.

It will stop and ask you for SQL Server credentials partway through — that's
expected, not a failure.

### Skipping demo data

Say so explicitly in the request:

```
Scaffold a Dynamicweb 10 solution called PartnerDaysExtended, no demo data.
```

Be aware this drops more than product images — it also skips the demo
typography **and** colours, so you'll be styling from scratch.

### Using assets you already have

If you've already downloaded any of the asset packs, point the agent at them
and it'll use your copies instead of downloading again. You can mix and match —
for example a local database backup while it downloads the rest:

```
Scaffold a Dynamicweb 10 solution called PartnerDaysExtended, using the bacpac
at C:\Downloads\swift2.bacpac.
```

Worth doing if you're on conference wifi.

## 5. Run

```bash
dotnet run --project PartnerDaysExtended.Host
```

The site comes up on the port the host project was scaffolded with — the agent
will tell you, or check
`PartnerDaysExtended.Host/Properties/launchSettings.json`. The Dynamicweb
administration is at `/Admin`.

## 6. Set up the license

The first time you open the site, Dynamicweb asks you to set up a license.
Select **Trial**.

## Troubleshooting

**The agent doesn't know the scaffold skills.** Your working directory is
wrong. It has to be the folder containing `.claude/`.

**It asks for a solution name partway through.** You didn't give it one in the
request. Just answer — nothing is broken.

**A download fails or times out.** This package depends on
<https://doc.dynamicweb.com/downloads/swift> being reachable. Check you can
open that page in a browser. If the network is unreliable, download the asset
packs manually and point the agent at them as shown in step 4.

**`dotnet build` fails during scaffolding.** Check `dotnet --list-sdks` shows a
10.x SDK.

**You want to see what it's about to do.** Ask before you start — *"walk me
through what the scaffold-dw10-solution skill will do, don't run it yet"*.
Useful if you're demoing this.
