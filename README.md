# Photozip

An iOS-first photo bundler built with Expo, React Native, TypeScript, and NativeWind 4 (Tailwind CSS 3).

## Architecture

The app uses Expo Router and feature modules. Routes live in `src/app`, screen layouts in `src/screens`, photo/archive services and components in `src/features`, and reusable UI in `src/shared/ui`. Shared workflow state survives navigation through an application-level provider.

See [architecture and contribution conventions](docs/architecture.md) for the folder tree and dependency rules.

See the [ads and monetization guide](docs/monetization.md) for the proposed AdMob setup, development-build requirements, and payment information.

## Run

```sh
npm install
npm run web          # Browser UI preview
npx expo start --go  # Preview the UI in Expo Go
npm run ios:build    # Build the native app with Xcode on macOS
```

ZIP creation and the iOS share sheet work in Expo Go for selections up to 100 photos totaling 25 MB. Larger bundles use native ZIP creation in a development build. The browser remains a UI preview. After installing the development build, use `npm start` for subsequent sessions. A physical iPhone is needed to validate real photo-library permissions, iCloud downloads, and share destinations.

## Included

- Three-step first-launch onboarding with Skip, Back, and Next; completion is saved locally. Replay it using the header help button.
- Minimal home screen focused on choosing photos, the gallery, and ZIP & Share.

- Device photo gallery with permission-aware loading; no sample photos.
- Multiple-photo picker, full permitted-library browsing with pagination, select all, and selected-only view.
- Choose photos, review your selection, then tap ZIP & Share to create the ZIP and open the native share sheet. Selecting photos alone does not create or share a ZIP.
- Archive naming, preparation/ZIP progress, and re-sharing from My ZIPs.
- On-device native ZIP packaging with original bytes, unique entry names, and staging cleanup.
- Current-session archive list with re-sharing.
- Permission, limited access, empty, and error states.

Personal photos are processed locally. The first-use demo uses illustrations only and never creates or shares files. The browser supports interface exploration and picking files, but ZIP and share actions require iOS or Android.

## Styling

Use NativeWind's `className` on React Native components:

```tsx
<View className="rounded-2xl bg-paper p-6">
  <Text className="text-xl font-semibold text-ink">Your photos, together.</Text>
</View>
```

Theme colors are in `tailwind.config.js`. `global.css`, `babel.config.js`, and `metro.config.js` configure NativeWind. Ordinary SCSS selectors do not style native iOS views.

## Practical limits and next steps

- iOS may grant access to only selected photos. Select all includes only accessible photos.
- iCloud originals must download before packaging. Keep the app open during creation.
- Destination apps must accept ZIP attachments and may impose size limits.
- Photos are stored without recompression for fast lossless packaging; ZIP is not a photo-size reduction tool.
- Staging copies and the ZIP require available device storage. Very large libraries need device profiling, storage preflight, cancellation, and potentially splitting into multiple archives before production release.
- ZIPs are saved in the app's document directory, but the archive index is currently session-only. Durable archive management and deletion are future work.
- Live Photos are treated as still images; paired motion resources are not included.

## Validation

```sh
npm test
npm run lint
npm run typecheck
npm run format:check
npx expo export --platform web
npx expo export --platform ios
```

Service tests validate ZIP contents and CRC, original bytes, duplicate filenames, input limits, cleanup, and the share handoff using mocked device APIs. Lint, typechecking, and exports validate source and bundling; they do not replace device testing.
# photozipper
