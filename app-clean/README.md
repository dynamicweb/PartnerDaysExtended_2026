# App — clean

**Package:** `PartnerDaysExtended_app_skill_bacpac.zip` (88 MB)

A **published** Dynamicweb 10.29.1 application. There is no solution and
nothing to build — you host the folder as it comes. Includes the database
backup and the base Swift files, but **no demo typography or colours**.

The database is the demo catalogue either way, so expect product images to be
missing. If you'd rather have a finished-looking shop, use the
[with demo data](../app-demo-data/) package instead.

**This package only gets you a running site.** There is no code project in it,
so there is nothing here to write code in — you work through the
administration and the Swift templates. If you want a project to develop in,
take a [solution package](../solution-clean/) instead.

You can still **use** add-ins: install them from the AppStore, or build one in
a separate project and install it into `wwwroot/Files/System/AddIns/Installed/`
— that's where the bundled MCP add-in sits. What you can't do is develop them
inside this package.

There's a second use for this package, too. If the thing you're building lives
**outside** Dynamicweb — your own application, in whatever stack — this package
gives you a real Dynamicweb instance to pull data from without standing up a
development solution. The bundled MCP server exposes tools over products
(including index queries and assets), content, documentation and migration, so
an agent can query this site while your actual project lives somewhere else
entirely.

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
| `wwwroot/` | The Dynamicweb `Files` folder and web content |
| `web.config` | IIS configuration — runs `bin\Dynamicweb.Host.Suite.dll` |
| `Database/` | The database backup |
| `.claude/`, `.agents/` | The `install-database` skill |

The latest Dynamicweb MCP server is already installed.

## 1. Install the prerequisites

- **A way to host it.** Either:
  - **IIS**, with the **.NET 10 Hosting Bundle** from
    <https://dotnet.microsoft.com/download/dotnet/10.0>. IIS cannot run the
    application without the bundle — this is not optional, and it's the single
    most common reason the site won't start. Install it, then run `iisreset`.
  - or just the **.NET 10 runtime**, if you'd rather run it directly with
    Kestrel (option B in step 4). Simpler, but nothing is preconfigured.

- **A SQL Server** — a local instance, a Docker container or a remote server.
  LocalDB does **not** work; Dynamicweb needs a real instance. If you have
  Docker but no SQL Server, that's fine — step 3 will offer to create a
  container for you.
- **An agent** — Claude Code, GitHub Copilot or OpenAI Codex.

## 2. Unpack

Extract to `C:\Projects\PartnerDaysExtended` or `C:\inetpub\PartnerDaysExtended`.
**Not** OneDrive, Dropbox or any synced folder — see the warning at the top of
this page.

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

## 4. Start the site

Two ways. IIS is what the package is set up for; Kestrel is quicker if you'd
rather not touch IIS at all.

### Option A — IIS

1. Open IIS Manager and add a new website.
2. **Physical path:** the unpacked folder root — the one containing
   `web.config`. Not `wwwroot`, not `bin`.

The defaults are fine for everything else — you shouldn't need to touch the
application pool or the folder permissions.

If you're hosting more than one package at the same time, give each site its
own port so the bindings don't overlap. If the site doesn't come up, see
Troubleshooting.

### Option B — Kestrel

Nothing is preconfigured for this, but the application runs on its own:

```bash
dotnet .in\Dynamicweb.Host.Suite.dll
```

It prints the URL it's listening on. This needs the .NET 10 runtime rather
than the Hosting Bundle, and skips IIS entirely.

Browse to the site. The Dynamicweb administration is at `/Admin`.

## 5. Set up the license

The first time you open the site, Dynamicweb asks you to set up a license.
Select **Trial**.

## 6. Sign in

Go to `/Admin` and sign in with:

| | |
|---|---|
| Username | `Administrator` |
| Password | `Administrator1` |

These are the workshop defaults and are the same in every package. Change them
before putting the site anywhere reachable from outside your machine.

## Troubleshooting

**HTTP 500.31 or 500.30 on first load.** Nearly always one of two things: the
Hosting Bundle isn't installed (step 1 — and remember `iisreset` afterwards),
or the database was never set up (step 3). Check the Windows Event Viewer under
*Windows Logs → Application* for the actual startup error.

If it still won't start, set the application pool's .NET CLR version to **No
Managed Code**. It isn't normally required, but it rules out the pool trying to
load the .NET Framework CLR alongside ASP.NET Core.

**HTTP 500.19.** IIS can't read `web.config` — usually the app pool identity
lacks read access to the folder, or the Hosting Bundle isn't installed so
`AspNetCoreModuleV2` isn't registered.

**The site loads but the administration won't save anything.** The app pool
identity can't write to `wwwroot/Files`. Grant it write access to the unpacked
folder — the identity is `IIS AppPool\<your app pool name>`.

**The agent doesn't know the `install-database` skill.** Your working directory
is wrong. It has to be the folder containing `.claude/` — not a parent folder,
not `wwwroot`.

**The import says the database already exists.** Run the skill again and choose
to drop and replace, or give it a different database name.

**Product images are missing.** Expected — this is the clean package. The
database holds the demo catalogue but the images aren't shipped. Add your own,
or switch to the [with demo data](../app-demo-data/) package.
