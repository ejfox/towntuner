# TownTuner QA Testing Notes

## ✅ What's Working

### Episode List Screen
- Real Harmontown episode data (episodes 1-15, 178, 360)
- Brutalist black/white monospace styling
- Episode completion tracking (85% rule)
- Filter toggle: "HIDE PLAYED" / "SHOW ALL"
- Prestige counter display
- Tap episodes to play
- Currently playing episode shows inverted colors
- Completed episodes show at 0.5 opacity

### Now Playing Screen
- Episode metadata display (number, date, title, comptroller, guests)
- Large episode number "album art"
- Working progress bar with real-time updates
- Play/pause, 15sec back, 30sec forward controls
- Playback speed cycling (0.5x - 2.0x)
- Status messages with Harmontown celebration

### Audio System
- Background audio support configured
- Lock screen controls (Remote Command Center)
- Demo mode simulation when no audio files present
- Progress tracking and persistence
- AirPlay support

### History Screen
- Lifetime statistics tracking
- Prestige level display
- Play session history (mock data)
- "Harmontown Memories" celebration stats

### Credits Screen
- Vodka bottle clicker with persistence
- Golden bottle animation every 100 clicks
- Global counter simulation
- Days since last episode counter
- Proper love letter to the community

### Prestige System
- 85% completion rule
- Shuffle only plays unplayed episodes
- Auto-prestige when all episodes completed
- Episode reset on prestige

## ⚠️ Known Issues for Testing

### Audio Handling
- App tries multiple file names: `sample.mp3`, `test.mp3`, `demo.mp3`, `harmontown_sample.mp3`
- Falls back to demo mode with simulated progress if no audio found
- Real audio playback depends on having MP3 files in bundle

### Data Persistence
- Episode progress/completion not persisted (Core Data not implemented)
- Only vodka bottle clicks persist via UserDefaults
- Prestige level resets on app restart
- Play history is currently mock data

### Episode Data
- Only 17 episodes loaded (should have all 364)
- Guest data incomplete for later episodes
- No episode descriptions or additional metadata

### Navigation
- Tab bar might need more styling refinement
- Sheet presentations work but could be smoother

## 🎯 Testing Focus Areas

### User Flow Testing
1. **Episode Selection**: Tap episodes, verify they "play" (demo mode)
2. **Now Playing**: Test all controls (play/pause, skip, speed)
3. **Filter Toggle**: Hide/show completed episodes
4. **Shuffle**: Verify only unplayed episodes play
5. **Prestige**: Complete all episodes to trigger prestige
6. **Tabs**: Navigate between all three screens
7. **Vodka Clicker**: Click bottle, verify counts persist

### Visual Testing
- Verify brutalist aesthetic (black/white/gray only)
- Check monospace fonts throughout
- Test completed episode opacity
- Verify currently playing inversion
- Check all text is readable

### Edge Cases
- What happens when no episodes available for shuffle?
- Long episode titles wrapping properly
- Very high vodka bottle click counts
- Multiple rapid taps on controls

## 📱 Device Testing Recommendations
- Test on various screen sizes (iPhone SE to Pro Max)
- Verify lock screen controls work
- Test background audio continuation
- Check tab bar visibility on different devices

## 🔧 Quick Fixes for Real Deployment
1. Load all 364 episodes from RSS/API
2. Implement Core Data for persistence
3. Add real audio file handling/streaming
4. Enhanced error handling for network issues
5. Add app icon and launch screen

## 💝 Community Feedback Areas
- Does this feel like a proper celebration of Harmontown?
- Are the episode titles/data accurate?
- Is the brutalist aesthetic working?
- Does the vodka clicker bring joy?
- Any missing essential Harmontown references?

---

**Ready for community testing!** The core experience is functional and celebrates Harmontown properly. Audio works in demo mode, all navigation flows work, and the brutalist aesthetic is consistent.