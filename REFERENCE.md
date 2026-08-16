# Error reference

Every error Carbura can show carries a stable code like `CBR-3004`.

A driver reads that out over the phone and you look it up here. Codes never
change once published, so this page stays correct for every version of the
app that is out in the field.

> Generated from `errors/catalog.json`. Do not edit by hand: run
> `node scripts/generate-error-docs.mjs` instead.

## What the ranges mean

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

## The ones that matter most

These mean somebody may be relying on something that is not working.

- **[CBR-1005](#cbr-1005)**. This account is blocked.
- **[CBR-3004](#cbr-3004)**. No route exists that fits this vehicle.
- **[CBR-3006](#cbr-3006)**. Your leader changed the route.
- **[CBR-4001](#cbr-4001)**. Carbura cannot access your location.
- **[CBR-4003](#cbr-4003)**. Location is turned off on this phone.
- **[CBR-4005](#cbr-4005)**. This phone is reporting a simulated location.
- **[CBR-6006](#cbr-6006)**. The app is not configured.
- **[CBR-7001](#cbr-7001)**. This vehicle has no height or weight declared.
- **[CBR-8003](#cbr-8003)**. An impossible jump in position was detected.
- **[CBR-8004](#cbr-8004)**. The app was built with a server key.

## Account and sign-in

### CBR-1001

**Email or password is incorrect.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `invalid_credentials` |

**Why it happens.** The credentials did not match any account.

**What to do.** Check the email and password and try again.

> Deliberately does not say which of the two was wrong: telling someone an email exists confirms which accounts are real.

### CBR-1002

**The session has expired.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | No, trying again will not change anything |
| Internal name | `session_expired` |

**Why it happens.** The access token is no longer valid and could not be refreshed.

**What to do.** Sign in again.

### CBR-1003

**An account already exists with that email.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `email_already_registered` |

**Why it happens.** Sign-up was attempted with an email that is already in use.

**What to do.** Sign in instead, or reset the password.

### CBR-1004

**The password is too short.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `password_too_weak` |

**Why it happens.** Passwords must be at least 10 characters.

**What to do.** Use a phrase you can remember. Length matters more than symbols.

### CBR-1005

**This account is blocked.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `account_blocked` |

**Why it happens.** An administrator blocked the account for abuse.

**What to do.** Contact whoever administers your fleet.

### CBR-1006

**New accounts are closed.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `signup_disabled` |

**Why it happens.** The server has sign-up disabled; accounts are created by invitation.

**What to do.** Ask your team leader to create your account.

### CBR-1007

**You still lead a team.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `still_leads_team` |

**Why it happens.** The account cannot be deleted while it owns a team. The team belongs to the people in it as much as to whoever created it, and its trip history is their record too.

**What to do.** Hand the team over to another leader, or close it, and then delete the account.

### CBR-1008

**This account has no profile yet.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `profile_not_set_up` |

**Why it happens.** The account exists but its Carbura profile was never created, so there is nothing on file to check the request against.

**What to do.** Open the app again. It creates the profile by itself the first time it loads. If this keeps happening, sign out and sign back in.

> Kept apart from CBR-1005 on purpose. Both refuse the request, and the server is right to refuse either way, but telling somebody their account is blocked when it simply has no profile yet sends them looking for a punishment that does not exist.

## Teams and membership

### CBR-2001

**That team code is not valid or has expired.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `invalid_join_code` |

**Why it happens.** No active team matches the code.

**What to do.** Check the code with your leader. Codes are 8 characters and never contain the letter O or the digit 0.

> Says the same thing for a non-existent code and an expired one, so probing cannot tell them apart.

### CBR-2002

**Too many failed attempts.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `too_many_join_attempts` |

**Why it happens.** More than 10 wrong codes in 15 minutes.

**What to do.** Wait a few minutes before trying again.

### CBR-2003

**That team is full.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `team_full` |

**Why it happens.** The team has reached its member limit.

**What to do.** Ask the leader to raise the limit or remove inactive members.

### CBR-2004

**You are not in that team.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `not_a_member` |

**Why it happens.** The action requires membership, and you left or were removed.

**What to do.** Join again with the team code.

### CBR-2005

**Only the team leader can do that.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `leader_only` |

**Why it happens.** The action is restricted to leaders.

**What to do.** Ask your leader.

> Enforced by the database, not the UI. Even a modified app gets rejected.

### CBR-2006

**That team has ended.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | No, trying again will not change anything |
| Internal name | `team_expired` |

**Why it happens.** Teams are temporary and this one passed its end date.

**What to do.** Ask your leader to create a new one.

### CBR-2007

**You have reached the limit of active teams.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `too_many_teams` |

**Why it happens.** A person can own at most 20 active teams.

**What to do.** Archive a team you no longer use.

## Trips and routes

### CBR-3001

**You already have a trip in progress.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `trip_already_active` |

**Why it happens.** Only one trip can be live at a time.

**What to do.** Finish or cancel the current trip first.

### CBR-3002

**That trip is already finished.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `trip_closed` |

**Why it happens.** Completed and cancelled trips cannot be reopened.

**What to do.** Start a new trip.

> Reopening an old trip would allow backdating false positions onto it.

### CBR-3003

**Only your leader can change this destination.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `destination_change_denied` |

**Why it happens.** The trip was dispatched by a team leader.

**What to do.** Ask your leader, or leave the team if you should not be under dispatch.

### CBR-3004

**No route exists that fits this vehicle.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `no_route_for_vehicle` |

**Why it happens.** No road on the way allows the declared height, weight or cargo.

**What to do.** Check the vehicle's height and weight are correct, or pick a different destination.

> A legitimate and frequent answer for trucks, not a malfunction.

### CBR-3005

**That destination is outside the mapped area.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `route_outside_map` |

**Why it happens.** The route falls outside the map region loaded on the server.

**What to do.** Contact whoever administers the server: the map region needs to be extended.

### CBR-3006

**Your leader changed the route.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `route_changed_by_leader` |

**Why it happens.** The team leader assigned a new destination while driving.

**What to do.** Follow the new directions when it is safe to do so.

> Not a failure. It is announced loudly on purpose: finding out late is worse than the change itself.

### CBR-3007

**This trip has no route yet.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `no_active_route` |

**Why it happens.** The route has not been calculated.

**What to do.** Calculate the route before starting to navigate.

## Location and navigation

### CBR-4001

**Carbura cannot access your location.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | Yes |
| Internal name | `location_permission_denied` |

**Why it happens.** Location permission was denied.

**What to do.** Grant location access in system settings. Without it there is no navigation.

### CBR-4002

**Navigation will stop when the screen turns off.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `background_location_denied` |

**Why it happens.** Background location permission was not granted.

**What to do.** Allow location "always" so directions continue with the phone in your pocket.

### CBR-4003

**Location is turned off on this phone.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | Yes |
| Internal name | `location_services_off` |

**Why it happens.** System location services are disabled.

**What to do.** Turn on location in system settings.

### CBR-4004

**You have left the assigned route.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | No, trying again will not change anything |
| Internal name | `off_route` |

**Why it happens.** Your position is outside the route's tolerance corridor.

**What to do.** Return to the route, or recalculate if the road is blocked.

> Your leader is notified. Nothing is blocked: blocking a driver would be dangerous.

### CBR-4005

**This phone is reporting a simulated location.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `mock_location_detected` |

**Why it happens.** The operating system reports the position comes from a mocking app.

**What to do.** Turn off the mock location app. Your leader has been notified.

### CBR-4006

**Too many position updates.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `position_rate_limited` |

**Why it happens.** More than 180 positions were sent in one minute.

**What to do.** Usually a bug in the app. Restart it.

### CBR-4007

**This phone's clock is wrong.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `clock_out_of_sync` |

**Why it happens.** The device clock differs from the server by more than five minutes.

**What to do.** Set the date and time automatically in system settings.

> Positions are still recorded, but flagged for review.

## Places and search

### CBR-5001

**Place search is unavailable.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `search_unavailable` |

**Why it happens.** The search server did not answer.

**What to do.** Try again, or pick from your saved places.

### CBR-5002

**Showing saved results.**

| | |
|---|---|
| Severity | Info, nothing is broken |
| Retrying helps | No, trying again will not change anything |
| Internal name | `showing_cached_results` |

**Why it happens.** There is no connection, so previously saved results are being used.

**What to do.** Nothing. Addresses rarely move, but results may be out of date.

### CBR-5003

**Nothing found for that search.**

| | |
|---|---|
| Severity | Info, nothing is broken |
| Retrying helps | Yes |
| Internal name | `place_not_found` |

**Why it happens.** The search returned no results.

**What to do.** Try fewer words, or the street and city separately.

### CBR-5004

**You already saved a place with that name.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `duplicate_saved_place` |

**Why it happens.** Home and Base can only exist once each.

**What to do.** Rename the existing one, or save this as a custom place.

## Connectivity and server

### CBR-6001

**No connection.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `no_connection` |

**Why it happens.** The device could not reach the server.

**What to do.** Check your signal. Navigation continues with the route already downloaded.

### CBR-6002

**The server took too long.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `server_timeout` |

**Why it happens.** The request exceeded its time limit.

**What to do.** Try again in a moment.

### CBR-6003

**The server had a problem.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `server_error` |

**Why it happens.** The server returned an unexpected error.

**What to do.** Try again. If it keeps happening, report this code.

### CBR-6004

**The routing server is not responding.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `routing_server_down` |

**Why it happens.** Valhalla is unreachable or still building its map.

**What to do.** Try again shortly. After a fresh deploy, building the map takes a while.

### CBR-6005

**This version of the app is too old.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `app_out_of_date` |

**Why it happens.** The server sent data this version does not understand.

**What to do.** Update Carbura.

### CBR-6006

**The app is not configured.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `not_configured` |

**Why it happens.** Required server addresses were not provided at build time.

**What to do.** Rebuild with the required --dart-define values. See the README.

### CBR-6007

**This server does not have that address.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `no_such_endpoint` |

**Why it happens.** The app asked for something the server does not serve. Either the app is older than the server, or it is pointed at the wrong address.

**What to do.** Update the app. If it is already up to date, check the server address it was built with.

> Answered at the front door rather than by the service behind it, so a request for an address that does not exist never costs any work. Anything that is not a real endpoint gets this, which also means a scan of common paths learns nothing from the difference between them.

## Vehicle and setup

### CBR-7001

**This vehicle has no height or weight declared.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `vehicle_dimensions_missing` |

**Why it happens.** Routing will treat it as an ordinary vehicle.

**What to do.** Add the height and weight before navigating. Without them the route may pass under a bridge your truck does not clear.

> The single most consequential missing field in the whole app.

### CBR-7002

**Those dimensions look wrong.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `vehicle_dimensions_implausible` |

**Why it happens.** A value is far outside what a real vehicle measures.

**What to do.** Check the units. Height goes in centimeters, weight in kilograms.

### CBR-7003

**You have no vehicle set up.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | No, trying again will not change anything |
| Internal name | `no_vehicle` |

**Why it happens.** No default vehicle is registered for this account.

**What to do.** Add your vehicle so routes account for its size.

## Security and abuse

### CBR-8001

**You do not have permission to do that.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `permission_denied` |

**Why it happens.** The database rejected the operation.

**What to do.** If you believe this is wrong, report this code.

### CBR-8002

**Too many requests.**

| | |
|---|---|
| Severity | Warning, degraded but usable |
| Retrying helps | Yes |
| Internal name | `rate_limited` |

**Why it happens.** The rate limit for this action was exceeded.

**What to do.** Wait a moment and try again.

### CBR-8003

**An impossible jump in position was detected.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `impossible_movement` |

**Why it happens.** The distance between two positions implies a speed no vehicle reaches.

**What to do.** Usually a GPS glitch. If it repeats, your leader will be notified.

### CBR-8004

**The app was built with a server key.**

| | |
|---|---|
| Severity | Critical, something being relied on is not working |
| Retrying helps | No, trying again will not change anything |
| Internal name | `insecure_configuration` |

**Why it happens.** A server-side key was found in the app configuration.

**What to do.** Rebuild with the public key. A server key inside an app exposes the entire database.

> The app refuses to start. This is intentional.

## Unexpected

### CBR-9001

**Something went wrong.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | Yes |
| Internal name | `unexpected` |

**Why it happens.** An error occurred that the app does not recognize.

**What to do.** Try again. If it keeps happening, report this code with what you were doing.

### CBR-9002

**The app received data it does not understand.**

| | |
|---|---|
| Severity | Error, the action failed |
| Retrying helps | No, trying again will not change anything |
| Internal name | `malformed_server_data` |

**Why it happens.** The server response did not match the expected shape.

**What to do.** Usually means the app and the server are different versions. Update the app.

---

49 codes, catalog version 1.
