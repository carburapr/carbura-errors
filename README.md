# Carbura error codes

Public reference for every error the Carbura app can show.

Carbura is navigation for road teams: a leader assigns destinations to drivers
and can change a route while the truck is already moving. It is built for
truckers, who drive vehicles that do not fit everywhere.

## Why codes exist

Every error carries a stable identifier like `CBR-3004`.

A driver on the shoulder of a road can read that out — *"it says CBR-3004"* —
and whoever helps them looks it up here and knows exactly what happened, in
seconds. Compare that to *"it says something went wrong"*, which starts a
twenty-minute conversation.

The codes never change once published. This page stays correct for every
version of the app in the field.

## Look up a code

**[Full reference →](REFERENCE.md)**

Or jump to the range:

| Range | Area |
|---|---|
| `CBR-1xxx` | Account and sign-in |
| `CBR-2xxx` | Teams and membership |
| `CBR-3xxx` | Trips and routes |
| `CBR-4xxx` | Location and navigation |
| `CBR-5xxx` | Places and search |
| `CBR-6xxx` | Connectivity and server |
| `CBR-7xxx` | Vehicle and setup |
| `CBR-8xxx` | Security and abuse |
| `CBR-9xxx` | Unexpected |

## The ones worth knowing

A few come up often enough to be worth recognizing on sight.

**`CBR-3004` — No route exists that fits this vehicle.**
Not a malfunction. It means no road on the way allows the declared height,
weight or cargo. Check the vehicle's dimensions are right, or pick a different
destination. With trucks this is a legitimate and frequent answer.

**`CBR-7001` — This vehicle has no height or weight declared.**
The single most consequential missing field. Without it the route is calculated
as if for an ordinary vehicle, and may pass under a bridge the truck does not
clear.

**`CBR-3006` — Your leader changed the route.**
Also not an error. It is announced loudly on purpose: finding out late is worse
than the change itself.

**`CBR-4005` — This phone is reporting a simulated location.**
The operating system says the position comes from a mocking app. The team
leader is notified.

## For developers

[`catalog.json`](catalog.json) is the machine-readable source. Each entry has:

```jsonc
{
  "code": "CBR-3004",
  "slug": "no_route_for_vehicle",
  "severity": "critical",      // info | warning | error | critical
  "retryable": false,          // whether trying again could help
  "summary": "...",            // what went wrong
  "cause": "...",              // why it happens
  "action": "...",             // what to do about it
  "note": "..."                // optional context
}
```

`retryable` is what decides whether the app offers a retry button. Offering one
for something that can never succeed just wastes a driver's attention.

## About this repository

Generated from Carbura's private source repository. Both the app and the server
mirror this catalog, and their test suites fail if any of them drift apart — so
what is written here is what the software actually does.

Reference text is in English. The app itself shows these messages translated,
in English and Spanish.
