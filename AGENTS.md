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
- The original logo, Colombo label, header notification icon, and notification overlay have been removed. QuietPlace uses a text wordmark; Settings still has a preview-only check-in updates toggle.
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
- Settings contains rounded grouped rows for map radius, preview check-in updates, Face ID, and About QuietPlace.
- Face ID and check-in updates are design previews only. They never request biometric or notification access, secure an account, or deliver alerts; their toggles are kept only in memory.
- Check-in requires a noise level. Wifi, outlets, and crowding are optional two-way controls. Positive values are Wifi OK, Outlets Free, and Uncrowded; negative values are Spotty Wifi, Outlets Full, and Crowded.
- New check-ins and community posts appear immediately for the current session. Cafés, favourites, reports, filters, preferences, radius, Face ID and notification preview states, the preview account, and posts otherwise live in memory and reset on refresh.
- Only the onboarding-complete flag uses `sessionStorage`. Returning visitors still need to create or sign into a preview account after a refresh.

## Intended functionality by screen

Use these screen contracts when maintaining the HTML prototype or translating it into SwiftUI. A screen describes a user destination; sheets and modals are listed separately because they are presented over a destination. Shared café, favourite, report, and post state must stay consistent wherever it appears. The prototype uses sample data and an in-memory preview account; its account, password reset, Face ID, map, and notification controls do not connect to live services.

1. **Landing / Welcome**
   - Introduce café discovery through recent community check-ins and establish the app identity.
   - Show this screen at the start of a new session before onboarding is complete. `Sign in to continue` opens Preview authentication; there is no guest route.

2. **Preview authentication**
   - Let users switch between `Sign in` and `Create account` on one screen. Sign in shows email and password; account creation also shows a display name used for new community posts.
   - Submitting either form creates an in-memory preview identity. The password is not checked, sent, or stored; a missing name falls back to a preview name. There is no guest route. Current Firebase wording in the HTML is aspirational, not implemented behavior.
   - First-time visitors continue to Preference onboarding. Visitors with the onboarding flag already set go to Home. `Forgot password?` opens Reset password.

3. **Reset password**
   - Present an email field and a `Send reset link` action reachable from Sign in, with a back action to Sign in.
   - In this prototype, submitting shows a confirmation and returns to authentication. It does not send an email or verify whether an account exists. Any UI copy suggesting a real reset link or Firebase service is aspirational and must not be treated as implemented behavior.

4. **Preference onboarding**
   - Users can select or clear Quiet, Fast wifi, Outlets, and Not crowded preference cards.
   - The selection is optional and currently affects only in-memory preference state; it does not automatically filter Explore.
   - `Continue to QuietPlace` stores only the onboarding-complete flag in `sessionStorage` and opens Home.

5. **Home**
   - Summarize the latest reports from saved cafés, show all saved cafés, and show the cafés with the most recent activity. Show an empty prompt leading to Explore when nothing is saved.
   - Selecting any café opens Café Detail. Each section's `View all` action opens Explore. Changes to favourites and check-ins appear here during the current session.

6. **Explore**
   - Search café names and areas as the user types. Show a result count and a useful empty state when nothing matches.
   - Let users combine Quiet, Wifi, Outlets, and Uncrowded filters. Filters use each café's latest check-in; `All cafés` clears them.
   - Each card shows the café name, area, distance, latest noise level and report age, plus Wifi, outlet, and crowd condition tags when reported. Positive tags are green and negative tags are red.
   - Selecting a card opens Café Detail. Its heart toggles the shared favourite state without opening the card. Wide layouts may show a detail preview beside results.

7. **Café Detail**
   - Show the café image, name, area, distance, opening-hours sample text, latest noise level, and recent check-ins, with older reports visually subdued.
   - The heart toggles the same favourite state used on Explore and Community. `Check In Here` opens the Check-in sheet for this café.
   - Back returns to the screen that opened the café, including Home, Explore, Community, Map, or Profile.

8. **Check-in sheet**
   - Open for the selected café from its detail page or map sheet. Require one noise value: Quiet, Moderate, or Loud; Quiet is selected when the sheet opens.
   - Wifi, outlets, and crowding are optional. Each can be unset or set to one of two mutually exclusive conditions: Wifi OK / Spotty Wifi, Outlets Free / Outlets Full, and Uncrowded / Crowded. Selecting an active option again clears that condition.
   - `Share check-in` puts a new report first in that café's list for the current session and refreshes its latest noise, tags, and any affected views.
   - The close control, backdrop, or Escape key dismisses the sheet without submitting.

9. **Community**
   - Show recommendations with author, age, café, note, and a favourite action. New posts appear at the top of the feed during the current session.
   - Selecting the café opens Café Detail. `Save café` / `Saved` toggles the same favourite state as other screens while keeping its text label.
   - `Share a favourite` opens the sharing modal. When no cafés are saved, direct the user to Explore to save one first.

10. **Share a favourite modal**
    - Offer only saved cafés in the custom café selector and require a written recommendation, up to 240 characters.
    - Submitting adds a post under the preview identity to Community for the current session. The close control, backdrop, or Escape key discards the draft.

11. **Map**
    - Show an illustrative café map with pins coloured by the latest noise level, an approximate location marker, a radius ring, a radius chip, and café search. It does not use live location or map data.
    - Search hides pins whose café name or area does not match. The chip and visual ring reflect the radius selected in Settings; changing radius does not fetch or geographically filter cafés.
    - Selecting a pin opens the corresponding café sheet directly above the mobile tab bar.

12. **Map café sheet**
    - Show the selected café, distance, latest noise level, and recent check-ins. Do not put a favourite heart beside its name.
    - `Details` opens Café Detail and `Check in here` opens the Check-in sheet. The close control or Escape key dismisses the map sheet.

13. **Profile**
    - Show the preview identity and all saved cafés, or an empty message when none are saved. Selecting a saved café opens Café Detail.
    - Keep Settings and Sign out in the Account group. Settings opens its Profile subpage. Sign out clears the in-memory account and returns to Preview authentication.

14. **Settings**
    - Open from Profile and return there with Back. Group discovery, notification preview, security, and app information controls.
    - Let users set a 100–1000 m map radius in 100 m steps. The choice updates the Map radius chip and visual ring for the current session.
    - `Check-in updates` toggles an in-memory presentation state only; it does not request permission, schedule alerts, or deliver notifications. Face ID opens its setup preview. About QuietPlace opens the informational modal.

15. **Face ID preview**
    - Explain how biometric sign-in could work in a native app. Its on/off action changes only an in-memory preview setting and never scans a face or protects the preview account.
    - Back and Done return to the originating screen.

16. **About QuietPlace modal**
    - Explain the café discovery, favourites, and community check-in concept. Its close control, confirmation button, backdrop, or Escape key dismisses it.

17. **Primary navigation**
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
- Do not add guest access, credential checks, backend claims, live Firebase behavior, live Face ID behavior, live location, delivered notifications, non-café content, or venue-type filters unless requirements explicitly change.
- Verify JavaScript edits with `node --check app.js`. Check CSS brace balance and inspect affected screens at narrow and wide widths when a browser is available. There is no automated test command.
