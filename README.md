# Partner Days Extended 2026

Workshop packages for building on **Dynamicweb 10** with an AI coding agent
(Claude Code, GitHub Copilot or OpenAI Codex).

There are five packages. They all get you to the same place — a running
Dynamicweb 10 solution with the Swift design and a local database — but they
differ in how much is prepared for you up front, and the setup is genuinely
different for each.

**Pick one folder below and follow the guide inside it.** You don't need more
than one package, and you don't need anything from the other folders.

## Packages

| Folder | Package | What you get |
|---|---|---|
| **[solution-clean](solution-clean/)** | 19 MB | .NET solution with host + files project, code, database backup and the base Swift files. No typography or colours. |
| **[solution-demo-data](solution-demo-data/)** | 453 MB | Same as above, plus demo data: product images, typography and colours. |
| **[app-clean](app-clean/)** | 88 MB | Published Dynamicweb 10 (10.29.1) application, database backup and the base Swift files. No typography or colours. |
| **[app-demo-data](app-demo-data/)** | 522 MB | Same as above, plus demo data: product images, typography and colours. |
| **[skills-only](skills-only/)** | 34 KB | Just the agent skills. They build the solution for you and download every asset from the Dynamicweb doc site themselves. |

Each folder holds the package zip and a setup guide written for that package
alone. Every package except skills-only also ships with the latest Dynamicweb
MCP server installed, and with an `install-database` skill that finds your SQL
Server, imports the database and wires up the connection for you.

## Which one should I pick?

- **You want to write code** → *solution*. You get an `.slnx` to open in Visual
  Studio or Rider and a host project referencing `Dynamicweb.Suite`. Needs the
  .NET 10 SDK.
- **You just want a running site to click around in** → *app*. Already
  published, so there's nothing to build — but it has to be hosted in IIS.
- **You want to see the agent do the work** → *skills-only*. A 34 KB download;
  the agent builds and fetches the rest. The best demo, and the slowest route
  to a running site.
- **Clean or demo data?** Demo data means product images, typography and
  colours. Take *clean* if you intend to put your own design on top, and
  *demo-data* if you want the Swift demo shop to look finished out of the box.
  The database is the same in both.

## What every package needs

Whichever you pick, have these ready before you start:

- **A SQL Server** — a local instance, a Docker container or a remote server.
  LocalDB does **not** work. If you have Docker but no SQL Server, the setup
  skill will offer to create a container for you.
- **An agent** — Claude Code, GitHub Copilot or OpenAI Codex.
- **.NET 10 SDK** for the solution and skills-only packages, or **IIS with the
  .NET 10 Hosting Bundle** for the app packages.

Your guide covers the rest.

## Notes

- The package zips are stored with **Git LFS**. If you clone this repository,
  install [Git LFS](https://git-lfs.com) first — without it you get small
  pointer files instead of the actual zips. If you only want one package,
  download its zip from the folder here on GitHub rather than cloning; the
  repository is over a gigabyte in total.
- `GlobalSettings.Database.config` holds your local SQL credentials in plain
  text. It's a workshop file — don't commit it.
