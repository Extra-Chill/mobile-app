# extrachill-app — Implementation Plan

**CURRENT STATUS**: Planning stage only - no React Native implementation exists yet (Rob Coker has started a prototype)

## Purpose
Provide a cross-platform React Native app as part of the Extra Chill multisite ecosystem. The app will be its own WordPress multisite installation with React Native frontend, leveraging WordPress multisite native authentication and content management. Goal: provide native mobile experience for community and content consumption while maintaining seamless integration with the broader Extra Chill platform.

## Implementation Status
- **Planning**: Complete - comprehensive architectural planning documented below
- **WordPress Backend**: Not started - no multisite installation created yet
- **React Native Frontend**: Not started - no React Native project initialized
- **Development Environment**: Not configured - no package.json, dependencies, or build scripts exist
- **Current Directory Contents**: Only this planning document (plan.md)

## Architecture Overview
- **Platform**: WordPress multisite installation with React Native frontend
- **Database**: Own WordPress database as part of multisite network (standard multisite architecture)
- **Content Management**: WordPress backend with Gutenberg blocks using Block Data API
- **Authentication**: Native WordPress multisite authentication across all Extra Chill properties
- **Data Access**: WordPress multisite native functions (no REST API dependencies for internal content)
- **Frontend**: React Native consuming WordPress Block Data API and native multisite functions

## Key Technical Decisions
- **Platform**: React Native cross-platform for iOS + Android
- **Backend**: WordPress multisite installation (app.extrachill.com or similar)
- **Authentication**: Native WordPress multisite - users automatically logged in across all Extra Chill sites
- **Content**: Gutenberg blocks with Block Data API for flexible, native content management
- **Data Flow**: Multisite native functions for cross-site content (forum activity, user data, etc.)
- **API Layer**: WordPress Block Data API + WordPress REST API for React Native integration

## MVP Features
- **Native Authentication**: Seamless login via WordPress multisite (no custom tokens needed)
- **Forum Integration**: Browse community.extrachill.com forums via multisite functions
- **Content Consumption**: Articles from extrachill.com via multisite content sharing
- **Gutenberg Content**: Native WordPress content management with blocks
- **User Profiles**: Unified user experience across entire multisite network
- **Search**: Cross-site search using multisite native functions
- **Notifications**: Push notifications for forum activity and content updates

## Technical Architecture
- **WordPress Multisite**: New site in Extra Chill network (e.g., app.extrachill.com)
- **React Native Frontend**: Consumes WordPress Block Data API for content rendering
- **Content Management**: Admin uses Gutenberg blocks for app-specific content
- **Cross-Site Integration**: Uses `switch_to_blog()` / `restore_current_blog()` patterns for forum/content access
- **Authentication Flow**: Native WordPress authentication eliminates custom session management
- **Block Data API**: Structured content delivery optimized for mobile consumption

## Multisite Integration Patterns
The app leverages existing multisite functions from the Extra Chill ecosystem:

### **Forum Integration**
- Uses `ec_fetch_forum_results_multisite()` for cross-site forum search
- Leverages `ec_fetch_recent_activity_multisite()` for community activity feeds
- Access bbPress data via `switch_to_blog()` patterns to community.extrachill.com

### **Content Integration**
- Articles from extrachill.com via multisite content queries
- User authentication via native WordPress multisite functions
- Cross-site user data access using `preload_user_details()` patterns

### **Native Functions Available**
- Direct blog ID access (hardcoded as 2) - Community site access
- `preload_user_details()` - User authentication state
- `is_user_ad_free()` - User subscription status
- All existing multisite patterns from themes and plugins

## Monetization & Ads (Mediavine)
- Current site revenue comes from Mediavine ads injected into web pages. Native apps do not run the same JS ad slots by default.
- Options to consider:
  1. Do not embed Mediavine ads in the native app; rely on increased engagement, subscriptions, or e‑commerce later. Potentially require login to access full content (incentivize registered users).
  2. Render articles in a secure in-app webview to preserve existing ad slots (simple but reduces native UX and requires cookie/JS handling). Ensure you handle ad policies and consent flows.
  3. Integrate a native ad solution (AdMob / other) inside the app and map placements to web layout; requires implementing ad SDKs and updating privacy/consent flows.
- Recommendation for MVP: do not attempt Mediavine inside the native app. Launch without ads and require account sign-in for access OR provide limited preview to anonymous users. Track metrics and revisit monetization in Phase 2.

## React Native Technical Implementation
- **Content API**: WordPress Block Data API for structured content consumption
- **Authentication**: Native WordPress multisite - no custom token management needed
- **Data Layer**: WordPress REST API + custom multisite function endpoints
- **Storage**: React Native storage for app preferences; WordPress handles all content and user data
- **UI Components**: React Native with React Navigation; render Gutenberg blocks as native components
- **State Management**: Context API for app state; WordPress multisite provides user authentication state
- **Content Rendering**: Parse Gutenberg block data into React Native components
- **Offline Support**: Optional caching layer for content consumption when offline

## Development Approach
- **WordPress Backend**: Set up new multisite installation (app.extrachill.com)
- **Gutenberg Integration**: Create mobile-optimized block templates for app content
- **React Native Setup**: Configure RN app to consume WordPress Block Data API
- **Multisite Functions**: Leverage existing forum/content integration patterns
- **Block Rendering**: Map Gutenberg blocks to React Native components

## Implementation Phases

### **Phase 1: Multisite WordPress Backend**
1. **Multisite Setup**: Create new WordPress site in Extra Chill network (app.extrachill.com)
2. **Gutenberg Configuration**: Set up mobile-optimized block templates and content structure
3. **Multisite Integration**: Implement cross-site content access using existing patterns
4. **Block Data API**: Configure WordPress to serve structured block data for React Native
5. **Authentication Testing**: Verify native WordPress multisite authentication works across sites

### **Phase 2: React Native Frontend**
1. **React Native Project**: Initialize cross-platform RN project with WordPress Block Data API integration
2. **Block Rendering**: Create React Native components that render Gutenberg blocks
3. **Navigation**: Implement React Navigation with app screens (Forum, Content, Profile, etc.)
4. **Authentication Integration**: Connect to WordPress multisite authentication state
5. **Content Display**: Forum integration using multisite functions, article consumption

### **Phase 3: Forum Integration**
1. **Community Access**: Leverage `ec_fetch_forum_results_multisite()` and activity functions
2. **Forum Browsing**: Topic lists, thread views using existing multisite patterns
3. **User Interaction**: Topic/reply creation through WordPress API integration
4. **Search Integration**: Cross-site search using existing multisite search functions

### **Phase 4: Polish & Launch**
1. **Performance Optimization**: Caching strategies for content and user data
2. **Push Notifications**: Community activity and content update notifications
3. **Testing**: Comprehensive testing across iOS/Android platforms
4. **App Store Deployment**: Build and deploy to App Store and Google Play

## Acceptance Criteria
- **Native Authentication**: Users automatically logged in via WordPress multisite
- **Content Management**: Admin can create app content using Gutenberg blocks
- **Forum Integration**: Users can browse and interact with community.extrachill.com forums
- **Cross-Site Content**: Seamless access to extrachill.com articles and content
- **Unified Experience**: Consistent user experience across entire Extra Chill network

## Technical Benefits
- **No Custom Authentication**: WordPress multisite handles all user management
- **Content Flexibility**: Gutenberg blocks provide rich, flexible content management
- **Performance**: Direct multisite queries eliminate HTTP API overhead for internal content
- **Scalability**: Standard WordPress multisite architecture with proven patterns
- **Integration**: Leverages all existing Extra Chill multisite functions and data flows

## Next Steps (Implementation Readiness)

### Phase 1: Development Environment Setup
1. **React Native Project**: Initialize React Native project with proper directory structure
2. **Package Dependencies**: Set up package.json with required React Native and WordPress integration dependencies
3. **Development Scripts**: Create npm scripts for development, build, and testing
4. **Environment Configuration**: Configure development environment for WordPress API integration

### Phase 2: WordPress Backend Implementation
1. **Multisite Planning**: Confirm subdomain (app.extrachill.com) and multisite installation approach
2. **Content Strategy**: Define Gutenberg block templates and content structure for mobile optimization
3. **API Endpoints**: Set up WordPress Block Data API and custom endpoints for React Native

### Phase 3: React Native Frontend Development
1. **React Native Architecture**: Implement Block Data API integration patterns and component mapping
2. **Integration Scope**: Leverage existing multisite functions and extend as needed
3. **Development Timeline**: Establish development milestones and deployment timeline

**Current Priority**: Phase 1 - Development Environment Setup (no React Native code exists yet)

After architectural confirmation, detailed implementation guides will be provided for:
- WordPress multisite setup and configuration
- Gutenberg Block Data API integration
- React Native block rendering components
- Multisite function integration patterns
- Authentication flow implementation