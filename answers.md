# Tutorial 4 – Idempotence notes

Run straight after row 8 of Check yourself, without restarting the server.

| # | Request | Status | Devices after |
|---|---------|--------|---------------|
| 9 | POST /devices (probe) | 201 | 5 |
| 10 | POST /devices (probe) again | 201 | 6 |
| 11 | GET /devices/probe | 200 | 6 |
| 12 | PUT /devices/attic | 200 | 6 |
| 13 | PUT /devices/attic again | 200 | 6 |
| 14 | DELETE /devices/fridge | 200 | 5 |
| 15 | DELETE /devices/fridge again | 404 | 5 |

PUT and DELETE left the system in the same state whether I sent the request once or twice, so they are idempotent. POST is not idempotent: sending it twice created two devices called probe, and GET /devices/probe could only return the first one. DELETE's status changed from 200 to 404 on the repeat, but the state did not change, and idempotence is about the state, not the response.
