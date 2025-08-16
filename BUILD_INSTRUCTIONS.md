# TownTuner Build Instructions

## Prerequisites
- **Xcode 15+** installed
- **iOS 17.0+** deployment target
- **macOS** development machine

## Quick Start
1. **Open project**: Double-click `TownTuner.xcodeproj`
2. **Select target**: Choose "TownTuner" scheme
3. **Choose simulator**: iPhone 15 or any iOS 17+ device
4. **Build & Run**: Press ⌘+R or click ▶️ button

## Project Structure
```
TownTuner/
├── TownTuner.xcodeproj/
├── TownTuner/
│   ├── ContentView.swift      # Main episode list + tabs
│   ├── NowPlayingView.swift   # Audio player screen
│   ├── HistoryView.swift      # Stats and lifetime history
│   ├── CreditsView.swift      # Vodka bottle clicker
│   ├── AudioManager.swift     # Audio playback logic
│   ├── TownTunerApp.swift     # App entry point
│   └── Info.plist            # Background audio config
├── USER_QA_INSTRUCTIONS.md   # Testing guide
├── FEEDBACK_FORM.md          # Feedback collection
└── QA_TESTING_NOTES.md       # Technical notes
```

## Build Configuration
- **Deployment Target**: iOS 17.0
- **Language**: Swift
- **Framework**: SwiftUI
- **Audio**: AVFoundation + MediaPlayer
- **Background Modes**: Audio enabled

## Audio File Requirements
**FOR TESTING**: App includes test WAV file `001_1.wav` for demo playback.

**FOR REAL USE**: App expects Harmontown MP3 files in format:
```
001 - Achieve_Weightlessness.mp3
002 - The_Inception_Of_Girlfriends.mp3
003 - If_They_Have_Cubs,_We're_Already_Dead.mp3
```

**IMPORTANT**: You must add the `001_1.wav` file to the Xcode project target:
1. Right-click `001_1.wav` in Xcode navigator
2. Select "Add to Target" → TownTuner
3. Ensure it appears in Build Phases → Copy Bundle Resources

## Known Build Notes
- **No external dependencies** - uses only iOS system frameworks
- **Requires audio files** - No demo mode, needs real MP3s
- **Background audio** configured but requires device testing
- **Simulator compatible** for full feature testing
- **iOS-only features** - Audio session and remote controls are iOS-specific

## Recent Fixes
- ✅ **macOS compatibility** - Audio session code now iOS-only
- ✅ **MediaPlayer framework** - Remote controls iOS-only  
- ✅ **Cross-platform build** - Should compile on macOS now

## If Build Fails
1. **Clean build folder**: Product → Clean Build Folder (⌘+Shift+K)
2. **Reset simulator**: Device → Erase All Content and Settings
3. **Check Xcode version**: Needs Xcode 15+ for iOS 17 features
4. **Verify scheme**: Make sure "TownTuner" scheme is selected

## Testing on Device
To test background audio and lock screen controls:
1. Connect physical iPhone/iPad
2. Trust development certificate
3. Install and run on device
4. Test lock screen controls with simulated playback

## Success Criteria
App builds and shows:
- ✅ Black episode list with white text
- ✅ Tab navigation (Episodes/History/Credits)  
- ✅ Vodka bottle that can be tapped
- ✅ Demo playback when episode is tapped
- ✅ Now Playing screen with controls

Ready for QA testing when these basics work!