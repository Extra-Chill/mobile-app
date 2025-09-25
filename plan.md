# extrachill-app — Implementation Plan

## Purpose
Provide a cross-platform React Native app that gives users a lightweight mobile experience for community.extrachill.com (bbPress forum) and extrachill.com (articles). Use the existing WordPress + bbPress data via REST; no new central database for the MVP. Goal: improve mobile UX and increase forum engagement. Future work will integrate the store at shop.extrachill.com (WooCommerce).

## Key decisions (to confirm before dev)
- Platform: React Native cross-platform for MVP (iOS + Android simultaneously).
- Auth: **Migration in progress** - WordPress multisite now provides native cross-domain authentication. Legacy session token system (`session-tokens.php`) maintained for mobile app compatibility during transition. Use `Authorization: Bearer <token>` and store tokens in React Native Keychain until native WordPress authentication is fully integrated.
- API: core WP REST API + small custom extrachill endpoints. Server must accept Authorization header for mobile clients and not depend on browser cookies.
- Access model / monetization: decide whether the app is free to full-readers or gated to logged-in users only (see Monetization & Ads section).

## MVP features
- Login / logout via community theme integration (primary entry point)
- Browse forums: topic lists and thread view (core functionality)
- Create topic and reply to threads (engagement features)
- Basic search (posts & threads)
- Browse article feed (list by latest/category) with images (secondary priority)
- Article detail view with comments (secondary priority)
- Secure token storage (React Native Keychain) and lightweight local cache for recently viewed content

## Server/API requirements
- Verify and harden endpoints the app will consume:
  - Login: `/wp-json/extrachill/v1/handle_external_login` (`extrachill-integration/seamless-login.php`)
  - Token validate: `/wp-json/extrachill/v1/validate_token` (`extrachill-integration/validate-session.php`)
  - User details: `/wp-json/extrachill/v1/user_details` (`extrachill-integration/get-user-details.php`)
  - Forum endpoints: bbPress REST routes + custom community endpoints (`extrachill-custom/community-integration/`)
- Add / confirm: device token registration (APNs) if planning push, pagination & search params optimized for mobile, and rate limiting protections.
- Ensure Authorization header handling and HTTPS enforcement; avoid tokens in URLs.

## Themes & code to coordinate
- `extrachill` theme — blog REST compatibility: `/Users/chubes/Local Sites/community-stage/app/public/wp-content/themes/extrachill`
- `extrachill-community` theme — main user & bbPress integration; confirm REST hooks and session flow: `/Users/chubes/Local Sites/community-stage/app/public/wp-content/themes/extrachill-community`
- `extrachill-shop` — future WooCommerce store integration (shop.extrachill.com); plan deep-linking and product endpoints for Phase 2: `/Users/chubes/Local Sites/community-stage/app/public/wp-content/themes/extrachill-shop`
- Integration plugins / PHP files to audit/extend:
  - `extrachill-integration/session-tokens.php`
  - `extrachill-integration/validate-session.php`
  - `extrachill-integration/seamless-login.php`
  - `extrachill-integration/get-user-details.php`
  - `extrachill-custom/community-integration/community-comments.php`
- See `CLAUDE.md` for background and architecture documentation.

## Monetization & Ads (Mediavine)
- Current site revenue comes from Mediavine ads injected into web pages. Native apps do not run the same JS ad slots by default.
- Options to consider:
  1. Do not embed Mediavine ads in the native app; rely on increased engagement, subscriptions, or e‑commerce later. Potentially require login to access full content (incentivize registered users).
  2. Render articles in a secure in-app webview to preserve existing ad slots (simple but reduces native UX and requires cookie/JS handling). Ensure you handle ad policies and consent flows.
  3. Integrate a native ad solution (AdMob / other) inside the app and map placements to web layout; requires implementing ad SDKs and updating privacy/consent flows.
- Recommendation for MVP: do not attempt Mediavine inside the native app. Launch without ads and require account sign-in for access OR provide limited preview to anonymous users. Track metrics and revisit monetization in Phase 2.

## React Native technical notes
- Networking: central APIClient using Axios/fetch with `Authorization: Bearer` headers; support pagination and error handling.
- Storage: React Native Keychain or encrypted AsyncStorage for tokens and profile cache; Phase 2 adds offline database (SQLite/Realm) for caching and write queue.
- UI: React Native components with React Navigation; FlatList for efficient feeds; Fast Image library for image loading and caching.
- State Management: Context API or Redux for global state management; async/await for data fetching.
- Launch flow: validate token on app startup; prompt re-login on invalid/expired token.
- WebView fallback: implement react-native-webview component for cases where showing the original page (with ads or complex embeds) is necessary.

## First sprint (deliverables)
1. Backend audit: confirm community theme endpoints, bbPress integration, Authorization header handling, and CORS behavior for mobile.
2. Decide access model (open vs account-required) and note any server gating required.
3. React Native scaffold: project setup, APIClient, secure storage helper, Login screen via community theme.
4. Forum core functionality: topic list + thread view; implement create‑reply and create‑topic flows.
5. Forum authentication: login/logout flow integrated with community theme endpoints.
6. QA: login/logout, token validation, forum posting capabilities, and basic automated tests.

## Timeline (example)
- Week 1: Backend audit + React Native scaffold + community theme login integration
- Week 2: Forum browse & basic posting functionality  
- Week 3: Complete forum features + article integration (secondary)
- Week 4: QA, polish, build distribution (iOS + Android)

## Acceptance criteria
- Secure login via community theme integration with token stored in React Native secure storage and used for protected requests.
- Users can browse forum threads and topics reliably in the app (primary functionality).
- Users can create topics/replies and see them on the live community site.
- Users can browse articles (secondary functionality after forum features are complete).
- No new central DB for MVP; use existing WP + bbPress tables.

## Open questions
- Keep current long‑lived server tokens or implement short access + refresh tokens now?
- Required offline support level for MVP (none / shallow cache / full DB)?
- Include push notifications in Phase 1 or defer?
- Access model: require account to access app or allow anonymous preview?

## Next steps
Confirm token lifecycle choice, offline caching level, React Native cross-platform decision, and access/monetization approach. After confirmation I will provide detailed task breakdowns, example API request/response shapes, and starter React Native code for the APIClient, secure storage helper, and WebView component.