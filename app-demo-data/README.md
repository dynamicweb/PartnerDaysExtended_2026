# App — with demo data

**Package:** `PartnerDaysExtended_app_skill_bacpac_assets.zip` (522 MB)

A **published** Dynamicweb 10.29.1 application. There is no solution and
nothing to build — you host the folder as it comes. Includes the database
backup, the base Swift files **and the demo data** — product images, typography
and colours. The Swift demo shop looks finished out of the box.

This is the fastest route to a running, good-looking site. If you'd rather
start from a blank canvas, use the [clean](../app-clean/) package; the database
is identical in both and only the assets differ. If you want to write code,
take a [solution package](../solution-demo-data/).

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
| `bin/` | The compiled application |
| `wwwroot/` | The Dynamicweb `Files` folder, demo assets and web content |
| `web.config` | IIS configuration — runs `bin\Dynamicweb.Host.Suite.dll` |
| `Database/` | The database backup |
| `.claude/`, `.agents/` | The `install-database` skill |

Demo data lives in `wwwroot/Files/` — product images under `Images/Products`,
and typography and colours under `System/Styles`. The latest Dynamicweb MCP
server is already installed.

## 1. Install the prerequisites

- **IIS**, with the **.NET 10 Hosting Bundle** from
  <https://dotnet.microsoft.com/download/dotnet/10.0>. IIS cannot run the
  application without the bundle — this is not optional, and it's the single
  most common reason the site won't start. Install it, then:

  ```bash
  iisreset
  ```

- **A SQL Server** — a local instance, a Docker container or a remote server.
  LocalDB does **not** work; Dynamicweb needs a real instance. If you have
  Docker but no SQL Server, that's fine — step 3 will offer to create a
  container for you.
- **An agent** — Claude Code, GitHub Copilot or OpenAI Codex.

## 2. Unpack

Extract to `C:\Projects\PartnerDaysExtended` or `C:\inetpub\PartnerDaysExtended`.
**Not** OneDrive, Dropbox or any synced folder — see the warning at the top of
this page.

The 522 MB download unpacks to about 0.9 GB across 3,099 files, so give it a
moment.

Note where the folder root is — the level containing `web.config`. IIS needs
exactly that path in step 4, and pointing it at `wwwroot` or `bin` instead is a
common mistake.

## 3. Set up the database

**Do this before creating the IIS site.** The application fails to start
without a database connection, so setting up IIS first just gives you an error
page.

Open the **unpacked folder** in your agent — the one containing `web.config`.
This matters: the skill lives in `.claude/skills/` inside the package and is
only found when that folder is the working directory.

Then ask for the skill. In Claude Code:

```
/install-database
```

In Copilot or Codex, ask in plain words: *"run the install-database skill"*. It
won't trigger on its own — you have to ask for it.

The skill will:

1. Find `Database/swift2.bacpac`.
2. Detect your SQL Server — local instance, running Docker container, or offer
   to create a new container. It'll ask which to use.
3. Ask for credentials and a database name (default: `swift`).
4. Import the backup with `sqlpackage`. If `sqlpackage` isn't installed it
   offers to install it, and to remove it again afterwards.
5. Write the connection details into
   `wwwroot/Files/GlobalSettings.Database.config`.

## 4. Create the IIS site

1. Open IIS Manager and add a new website.
2. **Physical path:** the unpacked folder root — the one containing
   `web.config`. Not `wwwroot`, not `bin`.
3. **Port:** pick a free one, e.g. 8080.
4. **Application pool:** set the .NET CLR version to **No Managed Code**.
   ASP.NET Core runs out of process from the .NET Framework CLR, and leaving
   this on the default will fail to start.
5. **Permissions:** give the app pool identity read and write access to the
   folder. Dynamicweb writes into `wwwroot/Files` at runtime, so read-only
   isn't enough.

Browse to the site. You should see the Swift demo shop with product images,
typography and colours in place. The Dynamicweb administration is at `/Admin`.

## Troubleshooting

**HTTP 500.31 or 500.30 on first load.** Nearly always one of two things: the
Hosting Bundle isn't installed (step 1 — and remember `iisreset` afterwards),
or the database was never set up (step 3). Check the Windows Event Viewer under
*Windows Logs → Application* for the actual startup error.

**HTTP 500.19.** IIS can't read `web.config` — usually the app pool identity
lacks read access to the folder, or the Hosting Bundle isn't installed so
`AspNetCoreModuleV2` isn't registered.

**The site loads but the administration won't save anything.** The app pool
identity can't write to `wwwroot/Files`. Fix the folder permissions in step 4.5.

**Product images are missing.** You unpacked the clean package by mistake —
check the zip filename ends in `_assets.zip`.

**The agent doesn't know the `install-database` skill.** Your working directory
is wrong. It has to be the folder containing `.claude/` — not a parent folder,
not `wwwroot`.

**The import says the database already exists.** Run the skill again and choose
to drop and replace, or give it a different database name.
