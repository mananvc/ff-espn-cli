# ff: ESPN Fantasy Football CLI

A Bash command-line tool that pulls your league data from ESPN Fantasy Football and turns it into reports that help you make better lineup and waiver decisions.

## Project Goal

ESPN Fantasy Football displays basic league stats but gives users no way to export that data and draw their own conclusions. Without it, managers can't easily tell which bench players should be starting or which waiver-wire players consistently outperform their projections. ESPN's internal API does provide this data, but it is undocumented, requires login cookies for private leagues, and returns complex JSON.

`ff` retrieves league data from ESPN's fantasy API and generates automated reports, such as points left on the bench and free agents outperforming their projections. It also exports roster and scoring data to CSV so users can run their own analysis.

## MVP Scope (v1.0)

- **ESPN fetch wrapper:** one `curl` function that pulls league data using the league ID and `espn_s2`/`SWID` cookies stored in a git-ignored `.env` file
- **`ff roster`:** your players with readable positions, teams, and starter/bench status, translated from ESPN's numeric ID codes
- **`ff week N`:** your weekly matchup with points scored per player
- **`ff bench`:** points left on the bench each week
- **`ff waivers`:** free agents scoring above their projections
- **`--csv`:** export roster and scoring data to the `exports/` folder

## Planned Usage

```
ff roster              # show your roster
ff week 8              # show your week 8 matchup
ff bench               # points left on the bench each week
ff waivers             # free agents beating their projections
ff roster --csv        # export your roster to exports/
ff --help              # show all commands
```

## Future Plans (v2.0+)

- Response caching to cut down on repeated API calls
- `ff standings`: win-loss record and points for/against
- `ff trends`: ASCII charts of player points across the season
- Custom scoring "what-if" rules (e.g. standings under PPR)
- Automated tests with bats-core using saved JSON fixtures

## Tech Stack

- **Language:** Bash 5 (installed via Homebrew, since macOS ships with Bash 3.2, which lacks the associative arrays used for ESPN ID lookup tables)
- **Commands and utilities:** `curl` (HTTP requests), `jq` (JSON parsing), `awk` (report calculations), `sort`, `column`, and `printf` (formatting), `getopts` (argument parsing)
- **Development tools:** ShellCheck, Git and GitHub, Homebrew
- **Data storage:** league ID, season, and cookies in a git-ignored `.env` file; reports exported as CSV to `exports/`. No database: data is fetched fresh from ESPN on each run.
- **Data source:** ESPN Fantasy Football's undocumented read API (`lm-api-reads.fantasy.espn.com`), accessed with `curl` GET requests using `view` parameters such as `mRoster` and `mMatchup`. Private leagues are authenticated with the `espn_s2` and `SWID` cookies from a logged-in browser session.

## Repository Structure

```
ff-espn-cli/
├── ff                     # main script (entry point)
├── lib/                   # helper scripts (ESPN fetch, ID lookups, reports)
├── tests/fixtures/        # saved ESPN JSON responses for offline testing
├── exports/               # CSV output (contents git-ignored)
├── .env.example           # template for your league ID and cookies
├── Eisenhower_Matrix.pdf  # feature prioritization
└── System_Diagram.pdf     # program flowchart
```

## Setup

1. Install dependencies: `brew install bash jq shellcheck`
2. Copy the config template: `cp .env.example .env`
3. Fill in `.env` with your league ID, season, and (for private leagues) your `espn_s2` and `SWID` cookies from Chrome DevTools → Application → Cookies → espn.com
4. Run `./ff --help`

Never commit your `.env` file. It contains your ESPN login cookies.

## Status

In planning. The Eisenhower Matrix and System Diagram are complete; development follows the milestone plan below.

1. Set up repo and environment
2. Load config and fetch league data
3. Build ID lookup tables
4. Add `ff roster`
5. Add `ff week N`
6. Add bench report
7. Add waiver report
8. Add CSV export and final cleanup
