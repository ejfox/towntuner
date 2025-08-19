# TownTuner

A dedicated podcast player for all 360 episodes of Harmontown, designed specifically for nightly sleep listening with gamification elements. Features a prestige system, brutalist monochrome design, and bleeding-edge iOS glass effects.

![TownTuner Screenshots](https://github.com/user-attachments/assets/5a5ccd8a-76a5-4bff-9ebc-d392e5a16a0e)

## Key Features

- **Prestige System**: Complete all 360 episodes chronologically to earn prestige levels
- **Brutalist Glass Design**: Monochrome aesthetic with advanced iOS 18+ glass morphing effects
- **Sleep-Optimized**: Designed for half-asleep operation with thumb-first navigation
- **Episode Tracking**: 85% completion rule with persistent progress tracking
- **Smart Shuffle**: Randomize only unplayed episodes from current prestige level
- **Professional Audio**: Background playback, lock screen controls, and sleep timer

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

## Implementation Status

### ✅ COMPLETED (MVP Ready)
- Core SwiftUI app structure with iOS/macOS platform support
- Episode data model with prestige system tracking
- Main episode list with bleeding-edge glass morphing effects
- Now playing screen with professional audio controls
- Audio manager with AVFoundation integration
- Background playback and lock screen controls
- Sleep timer with fade-out functionality
- Prestige system with animated episode resets
- Smart shuffle (unplayed episodes only)
- Episode completion tracking (85% rule)
- Enhanced glass button styles and animations
- App Store compliance preparation
- Bundled sample episode for submission

### 🔄 IN PROGRESS
- History/stats tracking implementation
- Transcript search integration
- Guest search functionality

### 📋 FUTURE FEATURES
- TownTuner Wrapped stats screen
- Vodka bottle clicker mini-game
- Firebase global counter integration
- CSV export functionality
- iOS widget development
- Apple Watch companion app

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