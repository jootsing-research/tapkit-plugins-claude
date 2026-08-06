---
name: weather
description: This skill should be used when the user wants to check the weather, view forecasts, check temperature or conditions for a city, use the weather map, manage saved cities, check air quality, wind, UV index, humidity, or interact with the Weather app on their iPhone.
---

# iOS Weather App

Use the Apple Weather app to answer weather questions and manage saved cities. Take a fresh screenshot after every action and verify the resulting screen before continuing. Derive tap and gesture targets from visible labels, icons, and the current screenshot; never reuse memorized coordinates.

## TapKit Setup

Before acting, follow the core TapKit setup: `list_phones()` -> choose `phone_id` -> `get_phone_status(phone_id)`. Include that `phone_id` in every TapKit call.

For every text-entry action:

1. Focus the correct field using its visible label and the current screenshot.
2. Call `copy_text_to_phone(phone_id, text)`, then `long_press` on the focused field (duration: 1500) and tap **"Paste"** in the tooltip that appears. Don't try to tap individual keyboard keys.
3. Call `screenshot(phone_id)` and visually verify the rendered text in the intended field.
4. Stop before tapping Search, Add, Save, Done, Confirm, or an equivalent action.
5. Perform the separate action only when the user explicitly authorized that exact action and the visual verification succeeded.

## Privacy and City Scope

Resolve the city in this order:

1. Use the city explicitly supplied by the user.
2. If the user supplied no city, use the named city already selected in the City Detail view.
3. If neither is available, ask the user for a city. Do not guess, browse saved cities for a likely choice, or use **My Location** as a fallback.

Follow these rules throughout the task:

- Never infer, expose, or report the device's precise location, coordinates, neighborhood, address, or travel position.
- Treat **My Location**, a device-location marker, and location permission prompts as sensitive. Do not use them to identify a city or tap a map's location control.
- Do not derive location from map position, local time, weather patterns, saved-city order, or any other visual clue.
- A screenshot may reveal unrelated saved cities, cards, temperatures, alerts, and forecast details. Ignore them and never quote, summarize, or mention them in tool commentary or the final response.
- Inspect and report only the city and weather fields necessary for the user's request. If the target city cannot be distinguished from similarly named results, ask the user for a state, region, or country instead of guessing.
- Add, delete, or reorder saved cities only in response to the user's current explicit request naming the exact city or requested order. Verify the target immediately before the state-changing gesture or tap and verify the result afterward.

## Open Weather

1. Call `open_app("Weather")` and take a screenshot to verify that Weather opened.
2. If `open_app` fails, call `press_home(phone_id)`, take a screenshot, locate the Weather icon in the current screenshot, and tap its derived center point. If it is not visible, open App Library, take another screenshot, locate the labeled Weather icon, and tap its derived center point.
3. Take a screenshot and verify that Weather opened.
4. Resolve the city using **Privacy and City Scope** before reading or reporting conditions.

## App Structure

Weather has three main views:

1. **City Detail** — current conditions, forecasts, and weather widgets for one city
2. **City List** — saved-city cards, search, and city-management controls
3. **Full-Screen Map** — weather layers and timeline controls

On City Detail, identify the persistent bottom toolbar from the current screenshot:

| Visible element | Action |
|---|---|
| Map icon at the bottom-leading edge | Open the full-screen weather map |
| Page dots near the bottom center | Indicate the selected saved-city page |
| List icon at the bottom-trailing edge | Open the city list |

Do not act on these controls until their identity is clear in a fresh screenshot.

## City Detail

The top of a named city's detail view generally shows:

- city name
- current temperature
- feels-like temperature
- daily high and low
- short forecast summary

When scrolled down, the header collapses to a sticky bar showing the city name with temperature and feels-like values. The background dynamically reflects current weather conditions, which affects text readability in screenshots.

Below the header are horizontally scrollable hourly conditions, a 10-day forecast, air quality, a wind-map preview, and detail widgets such as UV index, wind, sunrise or sunset, precipitation, visibility, humidity, moon phase, averages, and pressure. Content availability may vary.

- **Hourly forecast**: horizontally scrollable row starting with "Now"; each entry shows time, weather icon, and temperature. Swipe left on the row to see more hours.
- **10-day forecast**: each row shows day name, weather icon (with precipitation % in cyan if applicable), low temperature, a color-coded temperature bar, and high temperature. Tapping a day opens the Conditions Detail modal.
- **Air quality**: AQI number, rating (e.g., "Moderate"), color-coded gradient bar, description text.
- **Wind-map preview**: embedded map with animated wind patterns; tapping it opens the full-screen map.

### Weather Widget Grid

The detail widgets appear in this order, laid out in a 2-column grid (half-width widgets side by side):

| Widget | Width |
|--------|-------|
| Feels Like | Half (left) |
| UV Index | Half (right) |
| Wind | Full |
| Sunrise/Sunset | Half (left) |
| Precipitation | Half (right) |
| Visibility | Half (left) |
| Humidity | Half (right) |
| Moon Phase | Full |
| Averages | Half (left) |
| Pressure | Half (right) |

All widgets are tappable — each opens a detailed Conditions modal.

To inspect lower sections, swipe upward within a clearly scrollable portion of the detail view, then take a screenshot. Use visible section headings rather than a fixed number of swipes. Tapping a forecast day or detail widget may open a Conditions sheet with charts and a condition selector.

### Navigate Between Cities

Navigate only when the user explicitly requests another city or a next/previous saved city.

- For next or previous, swipe horizontally within the City Detail content area, then verify the resulting city in a screenshot.
- For a named saved city, open City List from the visible list icon, locate only the requested city card, and tap its screenshot-derived center point.
- Do not enumerate or report other city cards encountered while navigating.
- If the resulting city does not match the requested city, stop without reporting its weather and ask for clarification.

## City List and Search

Open City List by locating and tapping the list icon in a current City Detail screenshot. Verify the **Weather** heading and the visible search field labeled **Search for a city or airport**. City cards each show city name, local time, forecast summary, current temperature, and H/L.

The overflow menu may provide Edit List, Notifications, temperature units, Units, and Report an Issue. Locate menu items by their visible labels after opening the menu; do not assume their screen positions.

### Look Up a User-Supplied City Without Saving It

The user's request for weather in an exact city authorizes looking up that city, but does not authorize saving it.

1. Open City List and take a screenshot.
2. Locate **Search for a city or airport** from the screenshot and tap its derived center point to focus it.
3. Call `copy_text_to_phone(phone_id, user_supplied_city)`, then `long_press` the search field (duration: 1500) and tap **"Paste"** in the tooltip.
4. Take a screenshot and verify both the rendered query and the intended autocomplete result. Ignore all unrelated cards and results.
5. If the result is ambiguous, stop and ask the user for a state, region, or country.
6. If the exact result is verified, tap it because the user's city-specific lookup request authorized that navigation.
7. Take a screenshot and verify the named City Detail view before reading requested conditions.
8. Do not tap **Add** unless the user separately and explicitly asked to save that exact city.

### Add a Saved City

Proceed only when the user explicitly asks to add an exact city.

1. Open City List and take a screenshot.
2. Locate and focus **Search for a city or airport** using the current screenshot.
3. Call `copy_text_to_phone(phone_id, exact_city)`, then `long_press` the search field (duration: 1500) and tap **"Paste"** in the tooltip.
4. Take a screenshot and verify the rendered query and exact matching result.
5. If multiple results could match, stop and ask the user to disambiguate.
6. Tap the exact result only after verification; the current explicit add request authorizes this navigation.
7. Take a screenshot and verify the intended named City Detail view.
8. Locate **Add** (top-right of the detail view), but tap it only if the current explicit request authorizes saving this exact verified city.
9. Take a screenshot and verify that the target city was saved. Do not report any unrelated saved-city details visible during verification.

### Delete a Saved City

Proceed only when the user's current request explicitly names the exact city to delete. Never infer a deletion target from position or recency.

1. Open City List and take a screenshot.
2. Locate the exact target card by its visible city label. Ignore and do not report all other cards.
3. If the exact target is not visible or is ambiguous, stop and ask for clarification.
4. Reveal the target card's delete affordance using a screenshot-derived gesture (swipe left on the card reveals a red trash button), or open **Edit List** and locate the exact target row (red minus button on the left, then trash to confirm).
5. Take a screenshot and verify that the destructive control belongs to the exact requested city.
6. Only after that verification, perform the final delete action authorized by the user's current explicit request.
7. Take a screenshot and verify that the target city was removed. Do not enumerate the remaining cities.

### Reorder Saved Cities

Proceed only when the user explicitly specifies the city to move and its destination or intended relative order.

1. Open the overflow menu by locating it in a current City List screenshot.
2. Tap the visible **Edit List** item and take a screenshot.
3. Verify the exact target row and intended destination. Inspect no more of the list than required to establish that relationship.
4. If the requested destination is ambiguous, stop and ask for clarification.
5. Use `hold_and_drag()` from the target row's visible drag handle (≡) to the screenshot-derived destination.
6. Take a screenshot and verify the requested relative order before exiting edit mode.
7. Locate and tap the visible completion control (checkmark) only because the current explicit reorder request authorizes committing that verified order.
8. Take a final screenshot and verify the target's placement without reporting unrelated cities.

## Units

Changing units modifies Weather settings. Do so only when the user explicitly requests the exact unit change.

1. Open City List and take a screenshot.
2. Locate and open the overflow menu from that screenshot.
3. Locate the requested visible temperature option, or tap **Units** for other measurements.
4. Take a screenshot and verify that the intended option is selected before applying or leaving the view.

Depending on the OS version, Units may include temperature, wind, precipitation, pressure, and distance choices. Trust the currently visible labels rather than a memorized list or position. For reference, the Units panel typically offers:

| Unit | Default | Options |
|------|---------|---------|
| **Temperature** | System setting | Celsius (°C), Fahrenheit (°F), Use system setting |
| **Wind** | mph | mph, km/h, m/s, bft, kn |
| **Precipitation** | in | Inches, Millimeters/centimeters |
| **Pressure** | inHg | mbar, inHg, mmHg, hPa, kPa |
| **Distance** | mi | Miles, Kilometers |

## Full-Screen Map

1. On a verified City Detail view for the scoped city, locate the map icon in the current screenshot.
2. Tap its derived center point and take a screenshot.
3. Locate the layer selector by its visible icon or label (top-trailing area) to choose a layer when the user asks. The **X** close control sits at the top-leading edge.
4. Use the visible play control (▶) or the time scrubber at the bottom of the map only when needed for the request, verifying the layer and time shown afterward.

Available map layers:

| Layer | What it Shows |
|-------|---------------|
| **Precipitation** | Rain/snow patterns with intensity |
| **Temperature** | Heat map of temperatures |
| **Air Quality** | AQI levels across region |
| **Wind** | Animated wind flow lines (default) |

Never tap the device-location control, describe a device-location marker, infer where the device is from the map, or repeat unrelated place labels visible on the map.

## Conditions Detail Modal

Access by tapping any **day in the 10-Day Forecast** or any **detail widget**.

- **Day selector**: Scrollable horizontal row of dates
- **Condition type dropdown**: Switch between Conditions, UV Index, Wind, Precipitation, Humidity, Visibility, Pressure
- **24-hour chart** with hourly values, color-coded area/line chart
- **Additional sections**: Chance of Precipitation, Precipitation Totals, Forecast text, Daily Comparison

## Common Workflows

### Check Current Conditions

1. Open Weather and verify the app.
2. Resolve the city using **Privacy and City Scope**.
3. For a user-supplied city, follow **Look Up a User-Supplied City Without Saving It**.
4. Verify the target city name and read only the requested current-condition fields.
5. Report only the scoped city and requested weather details.

### Get a Forecast

1. Open and verify the correct named City Detail view.
2. Swipe upward within the scrollable content until the requested forecast heading is visible.
3. Take a screenshot after each swipe and stop when the needed section is found.
4. Read only the target city's requested days or hours; do not include unrelated on-screen data.

### Check Rain, Air Quality, or Another Detail

1. Verify the correct named City Detail view.
2. Locate the relevant visible forecast row, heading, or widget. Precipitation probability appears as cyan/teal text below the weather icon in the 10-day forecast when rain is expected.
3. If more detail is necessary, tap that verified target and take a screenshot of the resulting Conditions view.
4. Report only the metric, time range, and city requested by the user.

## Safety Checks

- Stop whenever the city, search result, destructive target, or destination cannot be verified exactly.
- Never treat **My Location** as permission to access or reveal device location.
- Never read unrelated saved-city data aloud or include it in summaries.
- Never save a searched city merely because its detail view presents **Add**.
- Never mutate the saved-city list based on a general request to check weather.
- Dismiss an incidental banner (e.g., a Health Information popup) only when it blocks the requested workflow, then take a screenshot to verify the intended view returned.
