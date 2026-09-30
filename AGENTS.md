# QuietPlace

QuietPlace is a responsive, iOS styled coursework prototype for discovering cafés through recent community check-ins. It is a static client side app with sample café data, preview account screens, favourites, check-ins, community posts, and an illustrative map. There is no backend, Firebase connection, build step, or automated test suite.

## Files and running

- `index.html` contains the app shell, device status area, tab and sidebar navigation containers, modal root, toast region, and Lucide CDN link.
- `styles.css` contains the iOS inspired design system, safe-area handling, grouped surfaces, responsive navigation, light and dark appearances, modal styling, and mobile and desktop layouts.
- `app.js` contains the sample cafés and reports, in-memory state, screen templates, delegated event handling, filters, favourites, check-ins, and community sharing.
- Open `index.html` directly in a browser. Lucide icons and Unsplash photos require a network connection; text icon fallbacks are included.

## Current implementation

- The app is café-only. Do not add other venue types, venue filters, or language suggesting that QuietPlace discovers general places.
- The main navigation contains Home, Explore, Community, Map, and Profile. Settings remains a subpage of Profile.
- The interface follows Apple Human Interface Guidelines with system typography, readable hierarchy, safe-area spacing, touch-sized controls, neutral grouped surfaces, accessible light and dark appearances, and a restrained warm brown accent.
- The original logo, Colombo label, notification icon, and notification overlay have been removed. QuietPlace uses a text wordmark.
- A preview account is mandatory. There is no guest path and no credential validation. Auth inputs are not sent, stored, or checked, and no Firebase service is connected.
- Home shows favourite café updates, saved cafés, and recent café activity. The saved café feature uses a compact warm brown surface with corrected internal spacing.
- Explore searches café names and areas. Its multi-select filters are All cafés, Quiet, Wifi, Outlets, and Uncrowded.
- Explore café cards show the café name, area, distance, current noise level, report age, and compact Wifi, outlet, and crowd condition tags. Positive tags use green; negative tags use red.
- Favourite controls use the same outlined and filled heart states in Explore, Community posts, and café detail. Community keeps its `Save café` or `Saved` text and does not show a rectangular pressed background.
- Café detail shows the image, venue information, recent check-ins, the universal favourite control, and the check-in action.
- Community shows sample recommendations. Users can save the shared café or open it. `Share a favourite` opens a themed modal with a custom café list and note field; only saved cafés can be shared.
- Map is an illustrative mock map, not live geolocation. It has a compact `Cafés nearby` label, radius chip, outline-free search field, café pins, and a radius ring controlled by Settings.
- Selecting a map pin opens a café sheet directly above the tab bar. The sheet shows recent check-ins, a Details link, close control, and check-in action. It does not show a favourite heart beside the café name.
- Profile shows the preview identity, favourite cafés, and an Account group containing Settings and Sign out with a subtle divider.
- Settings contains rounded grouped rows for map radius, Face ID, and About QuietPlace.
- Face ID is a design preview only. It never requests biometric access or secures an account, and its toggle is kept only in memory.
- Check-in requires a noise level. Wifi, outlets, and crowding are optional two-way controls. Positive values are Wifi OK, Outlets Free, and Uncrowded; negative values are Spotty Wifi, Outlets Full, and Crowded.
- New check-ins and community posts appear immediately for the current session. Cafés, favourites, reports, filters, preferences, radius, Face ID state, the preview account, and posts otherwise live in memory and reset on refresh.
- Only the onboarding-complete flag uses `sessionStorage`. Returning visitors still need to create or sign into a preview account after a refresh.

## Screen flow

1. **Landing / Welcome**
   - The app opens on the QuietPlace welcome screen for a new session.
   - `Sign in to continue` opens the mandatory account screen. There is no guest option.

2. **Preview authentication**
   - Users switch between `Sign in` and `Create account` on the same screen.
   - Submitting the form creates an in-memory preview identity. Credentials are accepted without validation and are never sent or stored.
   - First-time visitors continue to Preferences. Visitors with the onboarding flag already set continue directly to Home.

3. **Preference onboarding**
   - Users can select or clear Quiet, Fast wifi, Outlets, and Not crowded preference cards.
   - `Continue to QuietPlace` stores only the onboarding-complete flag in `sessionStorage` and opens Home.

4. **Home**
   - Home summarizes saved café updates, displays all saved cafés, and lists recent café activity.
   - Selecting a café opens Café Detail.
   - `View all` opens Explore.

5. **Explore**
   - Search filters cafés by name or area as the user types.
   - Filter chips can combine Quiet, Wifi, Outlets, and Uncrowded conditions. `All cafés` clears active filters.
   - Each café card includes current condition tags and a favourite heart.
   - Selecting a card opens Café Detail. Selecting its heart updates favourites without opening the card.

6. **Café Detail**
   - The detail screen shows café information and recent check-ins.
   - The heart adds or removes the café from favourites.
   - `Check In Here` opens the Check-in sheet.
   - Back returns to the screen that opened the café.

7. **Check-in sheet**
   - Users choose Quiet, Moderate, or Loud.
   - Wifi, outlets, and crowding can each be left empty or set to one of two conditions.
   - `Share check-in` adds the report to the selected café for the current session and refreshes recent reports and condition tags.
   - The close control, backdrop, or Escape key dismisses the sheet without submitting.

8. **Community**
   - Community lists sample recommendations with author, café, note, and favourite action.
   - Selecting the café opens Café Detail. `Save café` toggles its favourite state while retaining the text label.
   - `Share a favourite` opens the sharing modal. If there are no saved cafés, the user is redirected to Explore.
   - The modal uses a custom dropdown to select a saved café and a text area for the recommendation. Submitting adds the post to the current session feed.

9. **Map**
   - Search hides map pins that do not match the café name or area.
   - The radius chip and ring reflect the value selected in Settings.
   - Selecting a pin opens the café sheet above the tab bar.
   - `Details` opens Café Detail, `Check in here` opens the Check-in sheet, and the close control or Escape key dismisses the café sheet.

10. **Profile**
    - Profile shows the current preview identity and saved cafés.
    - Selecting a saved café opens Café Detail.
    - Settings opens the Settings subpage. Sign out clears the in-memory account and returns to Preview authentication.

11. **Settings and supporting screens**
    - Map radius changes the visual radius chip and ring on Map.
    - Face ID opens the preview setup screen. Its toggle changes only in-memory presentation state; Back or Done returns to the originating screen.
    - About QuietPlace opens an informational modal that can be closed with its controls, backdrop, or Escape key.

12. **Primary navigation**
    - The five-tab bar moves directly among Home, Explore, Community, Map, and Profile on mobile.
    - Wide layouts replace the tab bar with a sidebar while keeping the same destinations and state.

## Working conventions

- Follow Apple Human Interface Guidelines for hierarchy, navigation, system typography, safe areas, touch targets, grouped settings, and accessible light and dark appearances. Keep the warm brown tint as an accent with neutral system-like surfaces.
- Preserve button semantics, labels, visible keyboard focus, `aria-pressed`, `aria-checked`, `aria-expanded`, modal roles, and responsive behavior. Intentional removal of focus decoration from search and custom modal fields must not remove their accessible labels.
- Keep five top-level tabs and the Profile → Settings hierarchy.
- Rendering is template-string based. Update the relevant view, delegated event handling, and CSS together. Escape user-authored text with `escapeHtml()` before inserting it into HTML.
- Keep favourite state and heart behavior consistent in Explore, Community, and Café Detail. Use `aria-pressed` on interactive favourite controls.
- Keep `reportDetails()` compatible with legacy sample `tags` arrays and new structured `wifiStatus`, `outletStatus`, and `crowdStatus` reports.
- Use `Uncrowded` in visible report and filter labels. Positive condition tags use the shared green treatment; negative condition tags use the shared red treatment.
- Do not add guest access, credential checks, backend claims, live Firebase behavior, live Face ID behavior, live location, notifications, non-café content, or venue-type filters unless requirements explicitly change.
- Verify JavaScript edits with `node --check app.js`. Check CSS brace balance and inspect affected screens at narrow and wide widths when a browser is available. There is no automated test command.
