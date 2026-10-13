You are a senior React Native architect using Expo SDK 52 and Expo Router v4.

Design a production mobile app architecture for a project management app targeting iOS and Android (with web support via Expo):

Navigation structure (Expo Router file-based):
- (auth)/login, (auth)/register, (auth)/forgot-password
- (app)/(tabs)/home, (app)/(tabs)/projects, (app)/(tabs)/notifications, (app)/(tabs)/profile
- (app)/project/[id], (app)/task/[id], (app)/settings/index

State management: Zustand stores (authStore, projectStore, taskStore, uiStore)
Data fetching: TanStack Query v5 with offline support
Storage: WatermelonDB for offline-first data, expo-secure-store for tokens
Push notifications: Expo Notifications + push token registration

Deliver:
1. Full app/ directory tree
2. Root layout setup: auth state guard, theme provider, toast, deep link handling
3. Tab bar configuration with badge counts
4. Authentication flow: how unauthenticated users are redirected, how deep links work post-login
5. Environment config: app.config.ts with EAS Build environment support