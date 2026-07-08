# Theme Park Open Data

Free, first-party live data and datasets for the Walt Disney World, Disneyland, and Universal Epic Universe theme parks. Published by [Rope Drop News](https://ropedropnews.com) and released under a [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) license.

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg)](https://creativecommons.org/licenses/by/4.0/)

Live wait times, ride reliability, crowd levels, Lightning Lane prices, and Walt Disney World construction-permit filings. All endpoints are anonymous, need no API key, and return plain JSON or CSV over HTTPS. Use them in your own apps, dashboards, research, or notebooks. The only ask is attribution back to Rope Drop News (see [License](#license)).

## Quick start

```bash
# Rides that are down right now across every park
curl https://ropedropnews.com/parks/down-now

# Live wait times for Magic Kingdom
curl https://ropedropnews.com/parks/magic-kingdom/live

# 90-day ride reliability, as CSV
curl https://ropedropnews.com/reliability.csv
```

## Base URL

```
https://ropedropnews.com
```

## Parks

The `{slug}` in a URL is one of the values below.

| Slug | Park |
|---|---|
| `magic-kingdom` | Magic Kingdom (Walt Disney World) |
| `epcot` | EPCOT (Walt Disney World) |
| `hollywood-studios` | Disney's Hollywood Studios (Walt Disney World) |
| `animal-kingdom` | Disney's Animal Kingdom (Walt Disney World) |
| `disneyland` | Disneyland Park (Disneyland Resort) |
| `california-adventure` | Disney California Adventure (Disneyland Resort) |
| `epic-universe` | Universal Epic Universe |

## Live JSON endpoints

All are `GET`, return JSON, and refresh about once a minute. Please cache responses for at least 60 seconds rather than polling in a tight loop.

### Live wait times across all open parks

```
GET /wire/waits
```

```json
[
  {"ride":"TRON Lightcycle / Run","wait":55,"park":"Magic Kingdom","slug":"magic-kingdom"},
  {"ride":"Test Track","wait":75,"park":"EPCOT","slug":"epcot"},
  {"ride":"Radiator Springs Racers","wait":80,"park":"California Adventure","slug":"california-adventure"}
]
```

### One park's live wait times and ride status

```
GET /parks/{slug}/live
```

```json
{
  "slug":"magic-kingdom",
  "name":"Magic Kingdom",
  "isOpen":true,
  "opensNext":"2026-07-09T09:00:00-04:00",
  "closesToday":"2026-07-08T23:00:00-04:00",
  "rides":[
    {"name":"TRON Lightcycle / Run","wait":55,"status":"OPERATING","lightningLane":"$21.00","lat":28.419625,"lon":-81.577985,"lightningLaneCents":2100},
    {"name":"Peter Pan's Flight","wait":50,"status":"OPERATING","lightningLane":null,"lat":28.4202640272,"lon":-81.5818916811,"lightningLaneCents":null}
  ]
}
```

### Rides down right now, across every park

```
GET /parks/down-now
```

```json
[
  {"ride":"Seven Dwarfs Mine Train","park":"Magic Kingdom","slug":"magic-kingdom"},
  {"ride":"Remy's Ratatouille Adventure","park":"EPCOT","slug":"epcot"}
]
```

### Water parks status

```
GET /waterparks/live
```

```json
{
  "typhoonLagoon":{"isOpen":true,"closesToday":"2026-07-08T19:00:00-04:00","opensNext":"2026-07-09T10:00:00-04:00","running":10,"total":11},
  "blizzardBeach":{"isOpen":true,"closesToday":"2026-07-08T19:00:00-04:00","opensNext":"2026-07-09T10:00:00-04:00","running":12,"total":12}
}
```

### Live single-ride Lightning Lane prices

```
GET /lightning-lane-prices/live
```

```json
{
  "maxCents":2900,
  "singles":[
    {"ride":"Star Wars: Rise of the Resistance","park":"Disneyland","slug":"disneyland","url":null,"cents":2900,"price":"$29"},
    {"ride":"TRON Lightcycle / Run","park":"Magic Kingdom","slug":"magic-kingdom","url":"/parks/magic-kingdom/rides/tron-lightcycle-run","cents":2100,"price":"$21"}
  ]
}
```

## CSV datasets

All are `GET`, return `text/csv` with a header row, and refresh about hourly. Each is released under CC BY 4.0.

### Ride reliability

```
GET /reliability.csv
```

```
ride,park,availability_percent,grade,avg_outage_minutes,outages,operating_days
Golden Zephyr,California Adventure,64,Frequently down,172,40,28
Mine-Cart Madness™,Epic Universe,67,Frequently down,80,33,13
```

### Best times to ride, per park

```
GET /parks/{slug}/best-times.csv
```

```
attraction,average_wait_min,peak_wait_min,quietest_hour,busiest_hour,samples
TRON Lightcycle / Run,60,150,8 AM,10 PM,1372
Seven Dwarfs Mine Train,47,85,10 PM,7 PM,1204
```

### Crowd calendar, per park

```
GET /parks/{slug}/crowd-calendar.csv
```

```
date,crowd_level,average_wait_min,samples
2026-06-12,Moderate,21,948
2026-06-13,Light,18,1099
```

### Lightning Lane prices

```
GET /lightning-lane-prices.csv
```

```
ride,park,typical_price,cheapest_price,priciest_price,latest_price,cheapest_hour
```

### Walt Disney World construction permit tracker

```
GET /tracker.csv
```

```
project,location,status,expected_opening,filings,latest_filing
Beyond Big Thunder Expansion,Magic Kingdom,Early planning,TBA,2,2026-06-26
Tropical Americas,Animal Kingdom,Under construction,2027,0,
```

## Notes

- No authentication and no API key. All endpoints are open.
- Please cache. Live JSON refreshes about once a minute; CSVs about hourly. Tight polling loops add no freshness.
- Data is first-party and original to Rope Drop News.
- Endpoints may evolve. This README tracks the current shape.
- More datasets (ticket prices, weather history, showtimes, full park schedules) are planned for future release.

## License

All data here is released under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You are free to use, adapt, and republish it, including commercially, as long as you credit the source.

Suggested attribution:

```
Data from Rope Drop News (https://ropedropnews.com), CC BY 4.0
```

## More

- Data hub for journalists and researchers: https://ropedropnews.com/for-journalists
- Machine-readable index for AI tools: https://ropedropnews.com/llms.txt
- Site: https://ropedropnews.com

Built and maintained by Michael Burns.
