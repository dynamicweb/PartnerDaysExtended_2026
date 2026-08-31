# Partner Days Extended 2026

Five ready-made packages for getting a **Dynamicweb 10** site running, each
with an AI coding agent (Claude Code, GitHub Copilot or OpenAI Codex) already
set up to help.

Whichever you choose you end up in the same place: the Swift demo shop running
on your own machine, with its own database and an administration you can sign
in to. What differs is how much is prepared for you in advance, and how much
you can change afterwards.

**Pick one, open its folder, and follow the guide inside.** You don't need more
than one, and each guide covers the whole thing — database, hosting, license
and sign-in.

## Which package should I use?

| I want to… | Take | You'll need |
|---|---|---|
| See a finished Dynamicweb shop running and click around in it | **[app-demo-data](app-demo-data/)** | IIS, or .NET 10 runtime |
| The same, but with a plain design I'll style myself | **[app-clean](app-clean/)** | IIS, or .NET 10 runtime |
| Write code, starting from a finished-looking shop | **[solution-demo-data](solution-demo-data/)** | .NET 10 SDK |
| Write code, starting from a plain design | **[solution-clean](solution-clean/)** | .NET 10 SDK |
| Watch an AI agent build the whole thing from nothing | **[skills-only](skills-only/)** | .NET 10 SDK, internet |

Everyone also needs **SQL Server** — a local instance, a Docker container or a
remote server. LocalDB will not work.

## At a glance

| Package | Somewhere to write code | How it runs | Design | Download |
|---|---|---|---|---|
| [app-clean](app-clean/) | No | IIS, or Kestrel | Plain | 93 MB |
| [app-demo-data](app-demo-data/) | No | IIS, or Kestrel | Finished | 548 MB |
| [solution-clean](solution-clean/) | Yes | Visual Studio, or `dotnet run` | Plain | 20 MB |
| [solution-demo-data](solution-demo-data/) | Yes | Visual Studio, or `dotnet run` | Finished | 475 MB |
| [skills-only](skills-only/) | Yes | Visual Studio, or `dotnet run` | Your choice | 34 KB |

## Understanding the choice

There are only two decisions, and the table above has already made them for
you. This section is here if you want to know why.

### App or solution?

**App packages** are a finished Dynamicweb site, already built. There is
nothing to compile — you host the folder in IIS, or start it directly with the
.NET runtime, and it runs. You change the site through the Dynamicweb
administration and its templates, but there is no code project in the box, so
there is nowhere to write code.

You can still use add-ins: install them from the AppStore, or build one in a
separate project and install it into the site. What you can't do is develop
them here.

An app package is also useful in the opposite direction. It gives you a real,
running Dynamicweb instance with the MCP server installed — so if the thing
you're building lives *outside* Dynamicweb, this is a quick way to get a system
an agent can pull data from.

**Solution packages** are the same site as a development project. You open it
in Visual Studio or Rider, or run it from the command line, and you can change
the code. This is the one to take if you want somewhere to write code.

**Skills-only** is neither. It contains three agent skills and nothing else —
34 KB. The agent creates the solution and downloads every asset itself while
you watch. It ends up equivalent to a solution package, but it is the most
interesting one to demonstrate and the slowest way to reach a running site, and
it needs internet access throughout.

### Plain design or demo data?

"Demo data" means the product images, the typography and the colour scheme that
make the Swift demo shop look finished.

- **demo-data** — the shop looks complete straight away. Best if you want to
  show it to someone, or start from something that already looks right.
- **clean** — the same shop with the demo styling stripped out. Best if you
  intend to apply your own design.

The database is identical either way. Only the files differ, so a clean package
still has the full product catalogue behind it — just without the images and
styling on top.

## What's in the packages

Every package builds the same thing: **Dynamicweb 10.29.1** with the Swift
design, and a database restored from a backup during setup.

Each one also ships with an `install-database` skill for your agent, which
finds your SQL Server, imports the database and connects the site to it — so
the fiddliest part of the setup is handled for you. Every package except
skills-only additionally has the latest Dynamicweb MCP server installed.

What differs is the shape of what you unpack:

- **Solution packages** — a `.slnx` solution with a host project and a files
  project, plus the database backup.
- **App packages** — a published application: `bin`, `wwwroot`, `web.config`,
  plus the database backup.
- **Skills-only** — three agent skills. Everything else is downloaded from the
  Dynamicweb doc site as the agent works.

## Notes

- The package zips are stored with **Git LFS**. If you clone this repository,
  install [Git LFS](https://git-lfs.com) first — without it you get small
  pointer files instead of the actual zips. If you only want one package,
  download its zip from the folder here on GitHub rather than cloning; the
  repository is over a gigabyte in total.
- `GlobalSettings.Database.config` holds your local SQL credentials in plain
  text. It's a workshop file — don't commit it.
