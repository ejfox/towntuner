# CHANGELOG

All notable changes to TownTuner will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-11-03

### Changed - Modern Audio Architecture Rewrite

**Complete modernization of audio playback system using 2025 Swift best practices**

#### Core Architecture
- **Replaced AVAudioPlayer with AVPlayer** for superior streaming capabilities and future-proof architecture
- **Migrated to @Observable macro** (iOS 17+) for cleaner state management
  - Removed `ObservableObject` boilerplate
  - Automatic view tracking without `@Published` decorators
  - Improved performance with granular observation
- **Full async/await implementation** throughout audio system
  - Episode loading, playback controls, and audio session setup all async
  - Better error handling with structured concurrency
  - Improved battery efficiency

#### Technical Improvements
- **Modern time tracking** using `AVPlayer.addPeriodicTimeObserver()` instead of Timer
  - Smoother progress updates with CMTime precision
  - Native sync with audio playback state
- **Structured concurrency** for all long-running operations
  - Sleep timer uses Swift Task with proper cancellation
  - Async fade-out implementation
  - KVO observation through async sequences
- **MainActor isolation** for thread-safe UI updates
- **Custom AudioError enum** with typed error cases and localized descriptions
- **Improved delegate pattern** compatible with SwiftUI structs

#### Files Modified
- `AudioManager.swift` - Complete rewrite with modern patterns
- `NowPlayingView.swift` - Updated for async controls
- `EpisodeListView.swift` - Task-wrapped async calls
- `EpisodeListView_macOS.swift` - Task-wrapped async calls
- `ContentView_iOS.swift` - Modern state management
- `ContentView_macOS.swift` - Modern state management
- `HistoryView.swift` - Platform-specific modifiers

#### Performance & Reliability
- Better memory management with automatic cleanup
- Reduced battery usage with native AVPlayer observers
- Improved audio interruption handling (calls, Siri, route changes)
- More robust error recovery

#### Developer Experience
- Cleaner, more maintainable code
- Better type safety with Swift 6 compatibility
- Future-ready for iOS 18+ features
- Prepared for advanced features: streaming, CarPlay, watch complications

### Fixed
- Resolved macOS build compatibility issues
- Fixed platform-specific navigation modifiers
- Improved audio session lifecycle management

---

## [1.0.0] - 2025-08-16

### Added
- Initial release of TownTuner
- Core episode playback functionality
- Prestige system (reset all episodes after completing 360)
- Sleep timer with fade-out
- Variable playback speeds (0.5x to 3.0x)
- Lock screen controls and background audio
- Episode progress tracking (85% completion rule)
- Smart shuffle (unplayed episodes only)
- Glass morphism UI with brutalist aesthetics
- iOS and macOS support
- CloudKit integration for progress sync
- Episode history tracking
- 360 Harmontown episodes with metadata

### Features
- Play/pause, skip forward (30s), skip backward (15s)
- Now Playing screen with episode metadata
- Episode list with completion indicators
- Special playback modes (Dan's Philosophy Mode, Spencer's D&D Rush)
- AirPlay and Bluetooth support
- Audio interruption handling (calls, etc.)
- Session duration tracking
