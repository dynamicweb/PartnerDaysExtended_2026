# Partner Days Extended 2026

Five ready-made packages for getting a **Dynamicweb 10** site up and running,
each with an AI coding agent (Claude Code, GitHub Copilot or OpenAI Codex)
already set up to help.

They all end at the same place: a working Dynamicweb site with the Swift design
and a local database. What differs is how much is prepared for you, and how
much you can change afterwards.

**Pick one, open its folder, and follow the guide inside.** You don't need more
than one.

## Which package should I use?

| I want to… | Take | You'll need |
|---|---|---|
| See a finished Dynamicweb shop running and click around in it | **[app-demo-data](app-demo-data/)** | IIS, or .NET 10 runtime |
| The same, but with a plain design I'll style myself | **[app-clean](app-clean/)** | IIS, or .NET 10 runtime |
| Write code, starting from a finished-looking shop | **[solution-demo-data](solution-demo-data/)** | .NET 10 SDK |
| Write code, starting from a plain design | **[solution-clean](solution-clean/)** | .NET 10 SDK |
| Watch an AI agent build the whole thing from nothing | **[skills-only](skills-only/)** | .NET 10 SDK, internet |

Everyone also needs a SQL Server — a local instance, a Docker container or a
remote server. LocalDB will not work.

## Understanding the choice

There are only two decisions, and the table above has already made them for
you. This section is here if you want to know why.

### App or solution?

**App packages** are a finished Dynamicweb site, already built. You host the
folder in IIS and it runs — there is nothing to compile. You can change the
site through the Dynamicweb administration and its templates, but there is no
code project in the box, so there is nowhere to write code. You can still use
add-ins — installed from the AppStore, or built in a separate project — you
just can't develop them here.

That is not only a limitation: an app package gives you a real, running
Dynamicweb instance with the MCP server installed. If the thing you're building
lives *outside* Dynamicweb, this is a quick way to get a system an agent can
pull data from.

**Solution packages** are the same site as a development project. You open it
in Visual Studio or Rider, or run it from the command line, and you can change
the code. This is the one to take if you want somewhere to write code.

**Skills-only** is neither. It contains three agent skills and nothing else —
34 KB. The agent creates the solution and downloads every asset itself while
you watch. It is the most interesting one to demonstrate and the slowest way to
reach a running site, and it needs internet access throughout.

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

## What's in each package

| Folder | Download | Contents |
|---|---|---|
| [solution-clean](solution-clean/) | 19 MB | `.slnx` solution, host + files project, database backup, base Swift files |
| [solution-demo-data](solution-demo-data/) | 453 MB | As above, plus product images, typography and colours |
| [app-clean](app-clean/) | 88 MB | Published Dynamicweb 10.29.1 application, database backup, base Swift files |
| [app-demo-data](app-demo-data/) | 522 MB | As above, plus product images, typography and colours |
| [skills-only](skills-only/) | 34 KB | Three agent skills; everything else is downloaded on demand |

Each folder holds the package itself and a setup guide written for that package
alone. Every package except skills-only also ships with the latest Dynamicweb
MCP server installed, and with an `install-database` skill that finds your SQL
Server, imports the database and connects the site to it.

## Notes

- The package zips are stored with **Git LFS**. If you clone this repository,
  install [Git LFS](https://git-lfs.com) first — without it you get small
  pointer files instead of the actual zips. If you only want one package,
  download its zip from the folder here on GitHub rather than cloning; the
  repository is over a gigabyte in total.
- `GlobalSettings.Database.config` holds your local SQL credentials in plain
  text. It's a workshop file — don't commit it.
