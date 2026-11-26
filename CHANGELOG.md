# Changelog

All notable changes to `qalainau/filament-warp-map` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.1] - 2026-11-26

### Fixed

- Waiting for the public Nominatim server's turn (one request a second) now counts its 5 seconds on the real clock. Tests that freeze time with `travelTo()` while another request held the turn waited forever. After 5 seconds it still throws `LockTimeoutException`, as before.

## [1.1.0] - 2026-11-22

### Fixed

- **The default name of an imported plan uses the panel's date**: it used the app's UTC date, so it could show the next day during the UTC night. It now uses the panel's time zone.
- **`Invalid LngLat latitude value` in the browser console**: typing a value outside the range into the latitude or longitude of `LocationField` (for example 95, or -200 on its way to -20) made the map throw an error in the console. Now the map ignores a value outside latitude -90…90 / longitude -180…180 while you type: the pin stays where it was, and moves again as soon as both numbers are back in range.

### Added

- **`LocationField::required()`**: makes both the latitude and the longitude required. The range check (latitude -90 to 90, longitude -180 to 180) was already there and stays on whether or not the field is required. Without `required()` nothing changes.
- **Swap a standard page**: `mapPage()`, `territoriesPage()`, `routesPage()` and `myRoutePage()` on `FilamentWarpMapPlugin` take a class that extends the standard page (the URL and route name stay the same, so links between pages keep working). `mapPage(false)` leaves the map page out; the other three follow `territories()` and `routes()`. Matching getters: `getMapPage()`, `getTerritoriesPage()`, `getRoutesPage()`, `getMyRoutePage()`.

### Fixed

- **Page access rules on a replaced page**: when the app replaced a standard page or resource with a subclass (`xxxPage()`/`xxxResource()`), `canAccessUsing()` and `canWriteUsing()` received the subclass as `$page`, so a rule such as `$page !== Projects::class` did not match and a page the app had closed still opened (HTTP 200). `$page` is now always the plugin's own standard class, whatever the app swaps in. A rule that compared `$page` with the app's own subclass must compare with the standard class instead.

## [1.0.2] - 2026-11-20

### Fixed

- **Easier-to-read small text**: the hint under the location and area fields in dark mode, and the gray line under the selected person on the routes page, now have enough contrast against their background (they were below the WCAG AA ratio of 4.5:1).

## [1.0.1] - 2026-11-20

### Fixed

- **Console error `warpMap is not defined`**: on pages where Livewire re-renders a map inside another Alpine component, the browser console could show `warpMap is not defined` before the component's script had arrived. Empty components are now registered up front, and the real ones replace them as soon as they load. No visible change.

## [1.0.0] - 2026-11-19

### Added

- Map page: your own models on a map (`MapLocation`), with clusters that take the color of their places (grey when mixed), filters, search, drawing to select and selecting around a point.
- Heatmap on the map page, by the number of places or weighted by a `measure()`, following every filter.
- Territories drawn on the map or listed by postal code, with owners, totals and manual assignment; **Shade by** the number of places or a measure, with a scale and the value under each name.
- **Import shapes** on the territories page: territories from a GeoJSON or KML file, into a new plan by default (size, shape and point limits; KML with a DOCTYPE or entities is refused). `BoundaryImport` for your own code.
- Plans: an automatic split by count or by a measure, side-by-side comparison and adoption.
- Routes with the best order (opening hours, time at each place, lunch break, the way home), drag to reorder, confirm, print and CSV.
- **Share between people** on the map page: the selected places are shared between several people's days (even numbers counting visits already planned, nearest home first, each day in the best order), shown as a proposal and adopted or discarded. `Distributor` (`propose()`, `adopt()`, `assign()`) for your own code.
- My route for phones: **Whole route in Google Maps** (the visits still to do as one link, the next 10 when there are more), **Show where I am**, **Directions from here**, and **Arrived**, which records the time and, only then and only if the browser gives it, the position (`recordArrivalPosition(false)` to record the time only). Distances under a tenth of a mile are written in feet.
- Visits due: `MapLocation::visitEvery()` and `lastVisitedAt()`; the map colors places by when the next visit is due (overdue, due soon, not due yet), filters by state and selects every overdue place. `VisitDueColumn` for tables and `$record->mapVisitDue()`. `MapLocation::lastVisitsUsing()` reads the last visits of many places in one query (from your own records, such as the last completed work order).
- `HasMapView`: a **Table / Map** button for any Filament table (list pages, relation managers, table widgets). The map shows what the table's search, filters and tabs leave, from every page; drawing around places selects them for the table's bulk actions.
- Road distances from OSRM or Google, and straight-line estimates without a key.
- Geocoding with Nominatim or Google, remembered per tenant (`rememberFor()`, `remember(false)`, `Geocoding::forget()`, the `warp-map:prune-geocodes` command), and `geocodeOnSave(false)` for the whole panel.
- `LocationField` (hides **Find from the address** with `geocoding(false)`), `LocationEntry`, bulk actions and widgets for your own forms, pages and tables.
- `AreaField`: draw a polygon or a circle in your own form and save it in a JSON column. `Area` (`contains()`, `containsLocation()`, `whereInBounds()`, `toGeoJson()`) and the `AsArea` cast answer "is this point inside?".
- Warp Scheduler integration (`TravelTime`, `OrderDayAction`), used only when Warp Scheduler is installed.
- `canAccessUsing()` and `canWriteUsing()` decide per page who can open it and who can only look (for your app's own roles); they stop the navigation, the URL and Livewire calls alike.
- `navigationGroup()` and `SelectionAction::label()` take a closure too, called in the current language each time they are shown.
- Filament multi-tenancy.
- Ten languages: English, Japanese, German, French, Spanish, Italian, Brazilian Portuguese, Korean, Simplified Chinese and Traditional Chinese.
