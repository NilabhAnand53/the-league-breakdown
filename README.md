# The League Breakdown | Excel Dashboard

I built this learning project to summarize cricket team wins and player statistics in Excel. I used Power Query for data preparation, Power Pivot for data modeling, and DAX for calculations.

The report has separate team and player dashboard pages, with charts, summary cards and slicers for reviewing the available records.

## Project objectives

- Compare recorded wins across teams.
- Summarize match runs and wickets.
- Review the distribution of matches across venues.
- Compare player runs, wickets, strike rates and economy rates.
- Use slicers to examine selected teams and venues.

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

## Limitations and next improvements

- Revise team filtering to include appearances in both team positions before relying on team comparisons.
- Retrieve winner names through Match_Winner. The documented RELATED expression follows Team1_ID.
- Validate win type using innings order and result information. Comparing innings scores alone does not establish a win by runs or wickets.
- Separate each team's runs from combined match runs.
- Replace simple averages of strike rates with an aggregate calculation based on total runs and balls faced.
- Calculate aggregate economy from runs conceded and legal balls bowled, with appropriate treatment of non-bowling records.
- Convert cricket overs notation to balls before calculating bowling rates. For example, 3.2 overs represents 20 balls.
- Use sorted bar charts for player comparisons and replace the economy pie chart with a chart suited to rates.

These improvements are identified work, not completed corrections.

## Project files

The source workbook contains the five input tables. The build walkthrough documents the preparation steps, relationships, calculations and dashboard screenshots.

The completed interactive dashboard workbook should be included separately so readers can inspect the model and test the filters in Excel.

## What I practiced

This project gave me practice in preparing multiple related tables, building an Excel Data Model, writing DAX calculations and connecting PivotTables to dashboard visuals.

Reviewing the calculations also highlighted why relationship design, metric definitions and data coverage need to be checked before drawing conclusions from a dashboard.

## Repository layout

```text
the-league-breakdown/
├── README.md
├── data/
│   ├── raw/
│   │   └── league_source.xlsx
│   └── processed/
│       └── league_cleaned.xlsx
├── dax/
│   └── calculations.dax
├── docs/
│   └── league_build_walkthrough.docx
├── reports/
│   └── league_dashboard.xlsx
└── screenshots/
    ├── team_dashboard.png
    └── player_dashboard.png
```
