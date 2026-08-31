# Solution — with demo data

**Package:** `PartnerDaysExtended_project_skill_bacpac_assets.zip` (453 MB)

A Dynamicweb 10 solution you can open in an IDE, build and run. Includes the
code, the database backup, the base Swift files **and the demo data** — product
images, typography and colours. The Swift demo shop looks finished out of the
box.

If you'd rather start from a blank canvas and apply your own design, use the
[clean](../solution-clean/) package instead. The database is identical in both;
only the assets differ.

> [!IMPORTANT]
> **Extract to a local path on the C: drive** — for example
> `C:\Projects\PartnerDaysExtended`.
>
> Do **not** extract inside OneDrive, Dropbox or any other synced folder. Sync
> locks files while the build and Dynamicweb are using them, and the failures
> that follow look like something else entirely — you'll waste an hour chasing
> the wrong problem. Keep the path short while you're at it.

## What's in the package

| | |
|---|---|
| `PartnerDaysExtended.slnx` | The solution file |
| `PartnerDaysExtended.Host/` | The web application — references `Dynamicweb.Suite` |
| `PartnerDaysExtended.Files/` | The Dynamicweb `Files` folder, demo assets and the database backup |
| `.claude/`, `.agents/` | The `install-database` skill |

Demo data lives in `PartnerDaysExtended.Files/Files/` — product images under
`Images/Products`, and typography and colours under `System/Styles`.

The host is already pointed at the files folder through a `FilesPath` setting
in `PartnerDaysExtended.Host/appsettings.json`. You don't need to configure it.
The latest Dynamicweb MCP server is already installed.

## 1. Install the prerequisites

- **[.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)** — check
  with `dotnet --list-sdks`; you need a 10.x entry.
- **A SQL Server** — a local instance, a Docker container or a remote server.
  LocalDB does **not** work; Dynamicweb needs a real instance. If you have
  Docker but no SQL Server, that's fine — step 4 will offer to create a
  container for you.
- **An agent** — Claude Code, GitHub Copilot or OpenAI Codex.

## 2. Unpack

Extract to `C:\Projects\PartnerDaysExtended`, or anywhere else local and
short. **Not** OneDrive, Dropbox or any synced folder — see the warning at the
top of this page.

The 453 MB download unpacks to about 0.5 GB across 2,584 files, so give it a
moment.

## 3. Build

```bash
dotnet build PartnerDaysExtended.slnx
```

`Dynamicweb.Suite` comes from nuget.org — no feed configuration needed. Expect
a minute or so on the first build while packages download.

Doing this before anything else means that if your toolchain is wrong, you find
out now rather than after a database import.

## 4. Set up the database

Open the **unpacked folder** in your agent — the one containing
`PartnerDaysExtended.slnx`. This matters: the skill lives in `.claude/skills/`
inside the package and is only found when that folder is the working directory.

Then ask for the skill. In Claude Code:

```
/install-database
```

In Copilot or Codex, ask in plain words: *"run the install-database skill"*. It
won't trigger on its own — you have to ask for it.

The skill will:

1. Find `PartnerDaysExtended.Files/Database/swift2.bacpac`.
2. Detect your SQL Server — local instance, running Docker container, or offer
   to create a new container. It'll ask which to use.
3. Ask for credentials and a database name (default: `swift`).
4. Import the backup with `sqlpackage`. If `sqlpackage` isn't installed it
   offers to install it, and to remove it again afterwards.
5. Write the connection details into
   `PartnerDaysExtended.Files/Files/GlobalSettings.Database.config`.

## 5. Run

```bash
dotnet run --project PartnerDaysExtended.Host
```

The site comes up on <http://localhost:5041>, or <https://localhost:7046> if
you launch the `https` profile. The Dynamicweb administration is at `/Admin`.

You should see the Swift demo shop with product images, typography and colours
in place. If images are missing, you're on the clean package by mistake.

## Troubleshooting

**`dotnet build` fails immediately.** Check `dotnet --list-sdks` shows a 10.x
SDK. This is the cause far more often than anything else.

**The agent doesn't know the `install-database` skill.** Your working directory
is wrong. It has to be the folder containing `.claude/` — not a parent folder,
not `PartnerDaysExtended.Host`.

**The import says the database already exists.** Run the skill again and choose
to drop and replace, or give it a different database name.

**`sqlpackage` errors about certificates.** The skill already passes
`/TargetTrustServerCertificate:True`. If you're running the import by hand, add
it.

**The site starts but can't connect to the database.** Open
`PartnerDaysExtended.Files/Files/GlobalSettings.Database.config` and check the
values the skill wrote are what you expect.
