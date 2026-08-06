# World Clock UI

Read this reference only when the request needs city search, removal, reordering, or interpretation of World Clock entries.

## List map

Use the current screenshot to locate:

- The **World Clock** section label.
- The add control.
- **Edit** when available.
- City rows showing a city name, local time, and a day or time-offset relationship.

Do not infer a city from a partially visible row. Do not report unrelated saved cities when the user asked about only one.

## Add a city

1. Tap the visible add control and verify the search screen.
2. Call `copy_text_to_phone(phone_id, "requested city")` to load the city name onto the clipboard.
3. Focus the search field and screenshot to verify focus.
4. Long-press (~1500ms) the search field near the caret and tap **Paste** in the tooltip.
5. Screenshot and verify the complete rendered query and intended city result.
6. Stop before selection.
7. Add the matching city only as a separate, explicitly authorized action.
8. Verify that the intended city appears in the list.

If several places share a name, ask the user to disambiguate before selecting.

## Remove a city

1. Screenshot and identify the exact visible city row.
2. Present the city to be removed and ask for confirmation unless the user has just confirmed that exact visible row.
3. After confirmation, screenshot again and verify the same row.
4. Use only a currently visible delete affordance.
5. Verify that the city is absent and neighboring cities are unchanged.

## Reorder cities

1. Screenshot the current order.
2. Enter the visible editing mode.
3. Locate the intended city's current reorder handle.
4. Drag it relative to named neighboring cities rather than to a remembered screen point.
5. Screenshot and verify the new order before leaving edit mode.

## Interpret entries

Read the city name, local time, and visible day or hour-offset text together. Relative-day labels can change while the workflow is running, so take a fresh screenshot before reporting them.
