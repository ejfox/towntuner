# Dynamic Island & Modern iOS Media Integration

TownTuner is fully optimized for iOS 26 and modern iOS media features including Dynamic Island, Lock Screen widgets, and Control Center integration.

## ✅ Supported Features

### Dynamic Island (iPhone 14 Pro+)
- **Live Playback State**: Shows accurate playing/paused state in real-time
- **Progress Visualization**: Smooth progress bar with accurate scrubbing
- **Episode Metadata**: Displays episode title, number, and comptroller
- **High-Quality Artwork**: 1024x1024 retina artwork with intelligent scaling
- **Tap Actions**: Tap to expand full player controls
- **Long Press**: Quick access to playback speed and skip controls

### Lock Screen Integration
- **Now Playing Widget**: Full episode info with artwork
- **Playback Controls**: Play/pause, skip 15s back, skip 30s forward
- **Scrubber**: Precise seeking with visual progress
- **Speed Control**: Access all 8 playback speeds (0.5x - 3.0x)
- **Sleep Timer**: Visible timer countdown when active

### Control Center
- **Persistent Media Card**: Shows current episode while app is backgrounded
- **AirPlay/Bluetooth**: Seamless device switching
- **Volume Control**: Independent podcast volume
- **Quick Actions**: One-tap access to common controls

### Continuity Features
- **Handoff**: Continue playback across devices
- **External Content ID**: Unique episode identifiers for continuity
- **Service Identifier**: Proper app attribution in system UI
- **Playback History**: Track progress across app launches

## 🎨 Artwork Optimization

### Resolution
- **Base Size**: 1024x1024 (retina quality)
- **Dynamic Scaling**: Automatically scales to requested size
- **Performance**: Optimized rendering for different contexts

### Design
- **Brutalist Aesthetic**: Black background, white text
- **Episode Number**: Large monospaced font (#001-360)
- **Episode Title**: Readable subtitle text
- **Branding**: "HARMONTOWN" logo text

### Display Contexts
| Context | Size | Quality |
|---------|------|---------|
| Dynamic Island (compact) | 44x44 | Auto-scaled |
| Dynamic Island (expanded) | 200x200 | Auto-scaled |
| Lock Screen | 512x512 | Auto-scaled |
| Control Center | 256x256 | Auto-scaled |
| Now Playing (full) | 1024x1024 | Native |

## 📊 Metadata Properties

### Basic Info
```swift
MPMediaItemPropertyTitle            // Episode title (e.g., "Achieve Weightlessness")
MPMediaItemPropertyArtist           // "TownTuner"
MPMediaItemPropertyAlbumTitle       // "Harmontown"
MPMediaItemPropertyAlbumArtist      // Comptroller name
MPMediaItemPropertyComposer         // "Dan Harmon"
MPMediaItemPropertyGenre            // "Comedy Podcast"
```

### Playback State
```swift
MPNowPlayingInfoPropertyElapsedPlaybackTime    // Current position
MPMediaItemPropertyPlaybackDuration            // Total duration
MPNowPlayingInfoPropertyPlaybackRate           // Current speed (0.5x - 3.0x)
MPNowPlayingInfoPropertyPlaybackProgress       // 0.0 to 1.0 progress
MPNowPlayingInfoCenter.playbackState           // .playing or .paused (iOS 16+)
```

### System Integration
```swift
MPNowPlayingInfoPropertyIsLiveStream                  // false (on-demand)
MPNowPlayingInfoPropertyExternalContentIdentifier     // "harmontown-episode-123"
MPNowPlayingInfoPropertyServiceIdentifier             // "com.towntuner.harmontown"
MPNowPlayingInfoPropertyCurrentPlaybackDate           // Timestamp
MPMediaItemPropertyMediaType                          // .podcast
```

## 🎛️ Remote Commands

### Enabled Controls
| Command | Action | Skip Amount |
|---------|--------|-------------|
| Play | Resume playback | - |
| Pause | Pause playback | - |
| Toggle Play/Pause | Smart toggle | - |
| Skip Backward | Jump back | 15 seconds |
| Skip Forward | Jump ahead | 30 seconds |
| Seek Position | Scrub timeline | Variable |
| Change Rate | Cycle speeds | 0.5x - 3.0x |

### Disabled Controls
- Previous Track (single episode focus)
- Next Track (shuffle-only navigation)
- Seeking (use position change instead)
- Rating/Like/Dislike (not applicable)
- Bookmarks (use sleep timer instead)

## 🔊 Background Playback

### Audio Session
```swift
Category: .playback
Mode: .spokenAudio
Options: [.allowAirPlay, .allowBluetoothA2DP, .duckOthers]
```

### iOS 14.5+ Optimization
```swift
audiovisualBackgroundPlaybackPolicy = .continuesIfPossible
```

### Interruption Handling
- **Phone Calls**: Auto-pause, optional auto-resume
- **Siri Requests**: Pause during interaction
- **Route Changes**: Pause on headphone disconnect
- **System Alerts**: Duck volume, resume after

## 📱 Platform Requirements

| Feature | Minimum iOS Version |
|---------|-------------------|
| Dynamic Island | iOS 16.0 (iPhone 14 Pro+) |
| Playback State | iOS 16.0 |
| Background Policy | iOS 14.5 |
| Now Playing Info | iOS 8.0 |
| Remote Commands | iOS 7.1 |

## 🧪 Testing Checklist

### Dynamic Island
- [ ] Play episode - verify artwork appears
- [ ] Check progress bar updates smoothly
- [ ] Tap to expand - verify full controls
- [ ] Change speed - verify UI updates
- [ ] Pause/resume - verify state changes
- [ ] Skip forward/back - verify UI reflects changes

### Lock Screen
- [ ] Lock device while playing
- [ ] Verify artwork displays correctly
- [ ] Test all control buttons
- [ ] Scrub timeline - check accuracy
- [ ] Change playback speed
- [ ] Verify sleep timer countdown

### Control Center
- [ ] Swipe down while playing
- [ ] Check media card displays
- [ ] Test AirPlay switching
- [ ] Verify volume control independence
- [ ] Test with other apps playing

### Background Playback
- [ ] Switch to other apps
- [ ] Lock device
- [ ] Connect/disconnect Bluetooth
- [ ] Receive phone call
- [ ] Invoke Siri
- [ ] Play other media

## 🎯 Best Practices

### Artwork Generation
- Generate once per episode, cache result
- Use vector-based rendering when possible
- Keep file size reasonable (< 500KB)
- Ensure text is readable at small sizes

### Metadata Updates
- Update every 5 seconds during playback (not every frame)
- Always update after state changes (play/pause/seek)
- Batch updates when multiple properties change
- Clear metadata when no episode is playing

### Performance
- Use AVPlayer's native time observer (not Timer)
- Minimize main thread work in update callbacks
- Cache artwork to avoid regeneration
- Profile artwork rendering in Instruments

## 🚀 Future Enhancements

### Potential Additions
- [ ] Live Activities (iOS 16.1+) for extended progress tracking
- [ ] StandBy mode support (iOS 17+)
- [ ] Interactive widgets (iOS 17+)
- [ ] CarPlay integration
- [ ] Apple Watch complications
- [ ] HomePod metadata support
- [ ] SharePlay integration (iOS 15+)

### iOS 26+ Features (If Available)
- [ ] Enhanced Dynamic Island animations
- [ ] Spatial audio support
- [ ] ProMotion-optimized progress updates
- [ ] Always-On Display optimizations

## 📚 Resources

- [MPNowPlayingInfoCenter Documentation](https://developer.apple.com/documentation/mediaplayer/mpnowplayinginfocenter)
- [WWDC22: Media Metadata](https://developer.apple.com/videos/play/wwdc2022/110338/)
- [Background Audio Guidelines](https://developer.apple.com/documentation/avfoundation/media_playback/configuring_your_app_for_background_audio)
- [Dynamic Island HIG](https://developer.apple.com/design/human-interface-guidelines/live-activities)

---

**Last Updated**: 2025-11-03
**TownTuner Version**: 1.1.0+
**Tested On**: iOS 26 beta, iPhone 15 Pro
