# The League Breakdown | Excel Dashboard

The League Breakdown brings team wins, match totals and player statistics into two Excel dashboard pages. Charts, summary cards and slicers support comparisons across the available team and venue records.

The project covers data preparation in Power Query, relationship modeling in Power Pivot, and calculations in DAX.

## Dataset

The source workbook contains five tables:

| Table | Records | Description |
| --- | ---: | --- |
| dim_team | 8 | Team names, captains, coaches and home grounds |
| dim_player | 33 | Player names, team identifiers, roles and player details |
| dim_venue | 10 | Venue names, cities and capacities |
| fact_match | 60 | Match dates, participating teams, winners and innings statistics |
| fact_player | 201 | Player statistics recorded against individual matches |

The match dates range from 22 March to 19 May 2025.

The original dataset source and its accuracy as an official season record have not been confirmed. I treat it as a practice dataset. League is the project title; the supplied team, player and venue names are retained.

## Tech stack

| Tool | Use in this project |
| --- | --- |
| Microsoft Excel | Workbook organization and dashboard layout |
| Power Query | Data inspection and preparation |
| Power Pivot | Data Model and table relationships |
| DAX | Calculated columns and measures |
| PivotTables | Team and player summaries |
| PivotCharts | Visual comparisons |
| Slicers | Team and venue filtering |
| GETPIVOTDATA | Linking dashboard cards to PivotTable results |

## Data preparation

The preparation workflow included:

1. Importing the five source sheets into Power Query.
2. Inspecting column quality, value distribution and data types.
3. Promoting the first row to headers in the team query.
4. Trimming text fields in the dimension tables.
5. Applying duplicate and blank-row removal steps to the team table.
6. Loading the prepared tables into Excel's Data Model.

These steps describe the preparation performed. They do not imply that the source contained substantial missing or duplicate data.

## Data model

The documented model connects the tables through the following fields:

| Lookup table | Related table | Relationship fields |
| --- | --- | --- |
| dim_team | fact_match | Team_ID → Team1_ID |
| dim_venue | fact_match | Venue_ID → Venue_ID |
| dim_player | fact_player | Player_ID → Player_ID |
| fact_match | fact_player | Match_ID → Match_ID |

The match table stores one record per match. The player table stores the available player records for each match.

The team relationship currently follows Team1_ID. This is a model limitation because a team can appear in either Team1_ID or Team2_ID.

## Calculations and reporting

The build uses DAX to calculate match counts, combined innings runs, wickets and player totals. It also includes measures intended to calculate team appearances, wins and win percentage.

GETPIVOTDATA connects summary cards to the supporting PivotTables. Slicers are connected to the relevant PivotTables so selections can update the report views.

Team calculations and rate metrics require the validation described in the limitations section.

## Dashboard pages

### Team dashboard

The team page contains:

- Summary cards for total matches, runs and wickets.
- A comparison of recorded team wins.
- A chart of match runs grouped by team.
- A win-type chart.
- Match counts by venue.
- Team and venue slicers.

The run chart uses combined innings totals. Under the documented relationship, it should not be interpreted as runs scored by each team.

### Player dashboard

The player page contains:

- Player run totals.
- Player wicket totals.
- Average recorded strike rates.
- Average recorded economy rates.
- Summary cards for the two average rates.
- A venue slicer.

Player comparisons refer to the available player records rather than complete season statistics.

## Source-data validation

| Check | Verified value |
| --- | ---: |
| Match records | 60 |
| Teams in the team table | 8 |
| Players in the player table | 33 |
| Venues in the venue table | 10 |
| Combined innings runs | 20,944 |
| Combined innings wickets | 799 |

The match, run and wicket totals agree with the unfiltered dashboard cards shown in the project walkthrough.

The player table contains 7,463 runs and 178 wickets. These totals do not reconcile to the match totals, indicating partial player-stat coverage.

## Repository layout

```text
the-league-breakdown/
├── data/
│   ├── README.md
│   └── IPL_2025.xlsx
├── reports/
│   ├── README.md
│   └── project.xlsx
├── LICENSE
└── README.md
```

