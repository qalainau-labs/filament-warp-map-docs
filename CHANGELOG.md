# Changelog

All notable changes to `qalainau/filament-warp-map` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
