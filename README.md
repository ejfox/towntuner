# TownTuner 🎧

A dedicated podcast player for all 360 episodes of Harmontown, designed specifically for nightly sleep listening with gamification elements. Built with SwiftUI for iOS featuring a brutalist monochrome design with cutting-edge liquid glass effects.

![TownTuner Screenshots](https://github.com/user-attachments/assets/5a5ccd8a-76a5-4bff-9ebc-d392e5a16a0e)

## ✨ Key Features

### 🎮 Core Experience
- **Prestige System**: Complete all 360 episodes chronologically to earn prestige levels
- **Smart Shuffle**: Randomize only unplayed episodes from current prestige level
- **Sleep Timer**: Automatic fade-out for bedtime listening
- **Episode Tracking**: 85% completion rule with persistent progress tracking

### 🎨 Design & Animation
- **Liquid Glass UI**: Ultra-thin material backgrounds showing content beneath
- **Hero Transitions**: Smooth matched geometry animations between screens
- **Brutalist Aesthetic**: Monochrome design with professional SF Symbol effects
- **Sleep-Optimized**: Designed for half-asleep operation with thumb-first navigation

### 🎵 Professional Audio
- **Lock Screen Controls**: Full media integration with custom episode artwork
- **Background Playback**: Continues when app is backgrounded or device locked
- **AirPlay & CarPlay**: Seamless integration with external audio systems
- **Enhanced Speeds**: 0.5x-3x playback with special mode detection

### ☁️ Cloud Integration
- **iCloud Sync**: Episode progress and listening history across all devices
- **Real Statistics**: Actual listening time, completion tracking, streak calculation
- **Offline First**: Works without internet, syncs when available
- **Play History**: Complete log of every listening session with timestamps

## Core Mechanics

### The Prestige System
- Start at Episode 1, work through all 360 episodes chronologically
- Episodes marked complete at 85% playback
- Completing all episodes = 1 Prestige level
- Prestige counter permanently displayed
- Episodes reset to unplayed upon prestiging

### UI/UX Principles
- **Brutalist Glass Aesthetic**: Black background with iOS 18+ liquid glass effects
- **Thumb-First Design**: All primary actions reachable with right thumb
- **Information Hierarchy**: Episode number > Date > Comptroller
- **Enhanced Animations**: Spring physics with glass morphing transitions

## Technical Stack

- **Platform**: iOS 18+ (Swift/SwiftUI)
- **Glass Effects**: Custom liquid/ultra/morphing glass view extensions using SwiftUI materials
- **Audio**: AVFoundation with background playback, lock screen controls, and AirPlay
- **Storage**: Core Data for play history and progress tracking
- **Deployment**: App Store ready with bundled sample content

## 🚀 Implementation Status

### ✅ COMPLETED FEATURES
**Core App Foundation**
- ✅ SwiftUI app structure with iOS/macOS cross-platform support
- ✅ Episode data model with complete metadata for all 360 episodes
- ✅ Prestige system with animated episode resets and progress tracking

**Audio Engine & Media Integration**
- ✅ Professional AVFoundation audio manager with background playback
- ✅ Lock screen controls with custom generated episode artwork
- ✅ AirPlay, CarPlay, and Bluetooth audio support
- ✅ Sleep timer with smooth fade-out functionality
- ✅ Enhanced playback speeds (0.5x-3x) with special mode detection

**User Interface & Animation**
- ✅ Liquid glass UI with ultra-thin material backgrounds
- ✅ Hero transitions with matched geometry effects between screens
- ✅ Professional SF Symbol animations and haptic feedback
- ✅ Native iOS progress slider with functional seek controls
- ✅ Brutalist monochrome design with proper dark mode support

**Data & Cloud Integration**
- ✅ Complete CloudKit integration for cross-device sync
- ✅ Real statistics tracking with actual listening data
- ✅ Play history with timestamped session logging
- ✅ Smart episode completion tracking (85% rule)
- ✅ Streak calculation based on consecutive listening days

**Native iOS Features**
- ✅ Proper sheet presentation with corner radius and materials
- ✅ Full accessibility support with VoiceOver labels and hints
- ✅ Background audio session management with interruption handling
- ✅ App Store compliance with bundled sample episode

### 📋 FUTURE ENHANCEMENTS
**Advanced Features**
- 🔮 Transcript search integration with embedding API
- 🔮 Guest search functionality with frequent guest filters
- 🔮 TownTuner Wrapped annual statistics with sharing
- 🔮 Vodka bottle clicker mini-game in credits screen

**Platform Extensions**
- 🔮 iOS widget showing current episode and progress
- 🔮 Apple Watch companion app with big shuffle button
- 🔮 Siri Shortcuts for voice control integration
- 🔮 CSV export for comprehensive data analysis

## Unique Features

- **Vodka Bottle Clicker**: Cookie-clicker style mini-game in credits (planned)
- **TownTuner Wrapped**: Annual stats with both legitimate and satirical metrics (planned)
- **Guest Search**: Quick access to episodes by frequent guests (planned)
- **Lifetime History**: Every play session logged with timestamps (in progress)

## Development Philosophy

The app embodies the philosophy of "would I use this at 2 AM with a cat on my chest?" - optimizing for barely conscious operation while maintaining meaningful engagement through the prestige system and beautiful glass visual effects.

## Getting Started

1. Clone the repository
2. Open `TownTuner.xcodeproj` in Xcode 16+
3. Build and run on iOS 18+ simulator or device
4. The app includes Episode #1 as a bundled sample

See `CLAUDE.md` for detailed technical requirements and complete screen specifications.

---

*This is a labor of love, not money. No ads, no IAP, no bullshit.*