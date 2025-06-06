---
id: Velock
aliases: []
tags:
  - #project
  - #cycling
---

# Velock

## Roadmap

- Created an in-memory storage of TfL data for faster retrieval (in a hashmap)

## VPS Deployment

- [ ] Docker compose Backend (frontend to cloudflare)
- [ ] Docker compose Postgres db
- [ ] Upload to VPS server

## TODO

- [x] add tests
- [x] update remaining fetch requests to use axios
- [x] make save endpoint or modify existing if needed
- [ ] create objects for BikePoints and BikePointsLists with relevant methods
- [ ] update .env FREQ_UPDATE to be in minutes (and update calcs)
- [x] fix list ordering

## DONE

- [x] update refresh method for bikepoints (keep in memory update only changed timestamp)
- [x] add edit button
- [x] add modal up / down arrows (ie only in edit mode)
- [x] update refresh to auto add failed / missing bikepoints to db
- [x] add save button
