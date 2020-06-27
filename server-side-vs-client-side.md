## Tracking on the client side versus server side

What are we tracking?
- user interaction (events)
- user information (location, ip, api calls etc)
- can we correlate the api calls with events?
- errors *1
- metrics *1


NOTES 
- 1: errors and metrics tracking (latency) should be separated from analytics tracking

Disadvantages of server-side tracking
- requires additional resources/processing power. Imagine serving logs from few million users

Disadvantages of client-side tracking
- adblock
- can be manipulated if not throttling is present (multiple clicks etc)
