<img src="assets/banner.svg" alt="Carbura" width="100%">

# Carbura Inc. 2026

The public reference for every error the Carbura app can show.


Carbura is navigation for road teams. A leader assigns destinations to drivers
and can change a route while the truck is already moving. It is built for
truckers, who drive vehicles that do not fit everywhere.

## Why there are codes

Every error carries a stable identifier like `CBR-3004`.

A driver stopped on the shoulder can read that out, and whoever is helping them
looks it up here and knows what happened in seconds. Compare that with "it says
something went wrong", which starts a twenty minute conversation.

Codes never change once published, so this page stays correct for every version
of the app that is out in the field.

**[Look up a code in the full reference](REFERENCE.md)**

The first digit tells you the area. 1 is account and sign-in, 2 is teams, 3 is
trips and routes, 4 is location and navigation, 5 is places and search, 6 is
connectivity, 7 is vehicle setup, 8 is security, and 9 is anything unexpected.

## The ones worth recognising

**CBR-3004, no route exists that fits this vehicle.** Not a malfunction. It
means no road along the way allows the declared height, weight or cargo. Check
the vehicle's dimensions are right, or pick a different destination. With trucks
this comes up legitimately and often.

**CBR-7001, this vehicle has no height or weight declared.** The most
consequential missing field there is. Without it the route is calculated as if
for an ordinary vehicle, and it may pass under a bridge the truck does not
clear.

**CBR-3006, your leader changed the route.** Also not an error. It is announced
loudly on purpose, because finding out late is worse than the change itself.

**CBR-4005, this phone is reporting a simulated location.** The operating system
says the position comes from a mocking app. The team leader is notified.

## For developers

[`catalog.json`](catalog.json) is the machine readable source. Each entry has a
code, a slug, a severity, whether retrying could help, and three pieces of text:
what went wrong, why it happens, and what to do about it.

```jsonc
{
  "code": "CBR-3004",
  "slug": "no_route_for_vehicle",
  "severity": "critical",
  "retryable": false,
  "summary": "No route exists that fits this vehicle.",
  "cause": "No road on the way allows the declared height, weight or cargo.",
  "action": "Check the vehicle's height and weight, or pick a different destination."
}
```

The `retryable` flag decides whether the app offers a retry button. Offering one
for something that can never succeed just wastes a driver's attention.

## About this repository

It is generated from Carbura's private source repository. Both the app and the
server mirror this catalog, and their test suites fail if any of them drift
apart, so what is written here is what the software actually does.

The reference is in English. The app itself shows these messages translated, in
English and Spanish.
