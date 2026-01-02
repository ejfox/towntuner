# Year Labels in Episode List - Implementation Specification

## Overview
Add subtle year labels on the left side of the episode list to provide temporal context and quick navigation shortcuts for all 360 Harmontown episodes spanning 2012-2019 (plus one episode in 2021).

## Design Requirements

### Visual Design (Maintaining Brutalist Aesthetic)
```
2012  [#001]  2012.07.04
      Jeff Davis
      -------------------
      [#002]  2012.07.20
      Jeff Davis
      -------------------
      ...
      [#019]  2012.12.05
      Jeff Davis
      -------------------

2013  [#020]  2013.01.05
      Jeff Davis
      -------------------
      [#021]  2013.01.06
      Jeff Davis
      -------------------
```

### Year Label Specifications

**Positioning:**
- Left-aligned, 16pt from screen edge
- Appears adjacent to the first episode of each year
- Vertically centered with the episode number line
- 12pt spacing between year label and episode content

**Typography:**
- Font: System font (SF Pro), monospace variant
- Size: 14pt (slightly smaller than episode numbers)
- Weight: Bold
- Color: White with 0.6 opacity (subtle but readable)
- When scrolled past: Could optionally stick to top or remain in place

**Behavior:**
- Only displays on the first episode of each year
- Subsequent episodes in the same year have no year label (just empty space)
- Tappable area: Entire year label acts as anchor/jump target
- Accessibility: VoiceOver reads "Year 2012" etc.

### Episode Count by Year (From Data Analysis)
- 2012: 16 episodes (Jul-Dec) - Episodes #1-16
- 2013: 33 episodes - Episodes #17-49
- 2014: 41 episodes - Episodes #50-90
- 2015: 40 episodes - Episodes #91-130
- 2016: 44 episodes - Episodes #131-174
- 2017: 43 episodes - Episodes #175-217
- 2018: 39 episodes - Episodes #218-256
- 2019: 40 episodes - Episodes #257-296
- 2021: 1 episode - Episode #360 (finale)

**Note:** There are gaps in the numbering (2020 had episodes, no 2021 recording data in CSV)

## Implementation Approach

### 1. Data Model Enhancement

Add year detection logic to episode grouping:

```swift
struct Episode: Identifiable {
    let id: Int
    let number: Int
    let title: String
    let date: Date
    let comptroller: String
    
    var year: Int {
        Calendar.current.component(.year, from: date)
    }
    
    func isFirstOfYear(in episodes: [Episode]) -> Bool {
        guard let index = episodes.firstIndex(where: { $0.id == self.id }) else {
            return false
        }
        
        if index == 0 { return true }
        
        let previousEpisode = episodes[index - 1]
        return self.year != previousEpisode.year
    }
}
```

### 2. SwiftUI View Structure

Update the episode list row to include year labels:

```swift
struct EpisodeRowView: View {
    let episode: Episode
    let allEpisodes: [Episode]
    let isCurrentlyPlaying: Bool
    let isCompleted: Bool
    
    var body: some View {
        HStack(alignment: .top, spacing: 12) {
            // Year label (only for first episode of year)
            if episode.isFirstOfYear(in: allEpisodes) {
                Text("\(episode.year)")
                    .font(.system(.body, design: .monospaced))
                    .fontWeight(.bold)
                    .foregroundColor(.white.opacity(0.6))
                    .frame(width: 44, alignment: .leading)
                    .accessibilityLabel("Year \(episode.year)")
            } else {
                // Empty space to maintain alignment
                Color.clear
                    .frame(width: 44)
            }
            
            // Existing episode content
            VStack(alignment: .leading, spacing: 4) {
                HStack {
                    Text("[\(String(format: "#%03d", episode.number))]")
                        .font(.system(.body, design: .monospaced))
                    
                    Text(episode.date, style: .date)
                        .font(.system(.caption, design: .monospaced))
                }
                
                Text(episode.comptroller)
                    .font(.system(.caption))
                    .foregroundColor(.white.opacity(0.7))
                
                Divider()
                    .background(Color.white.opacity(0.2))
            }
            .opacity(isCompleted ? 0.5 : 1.0)
        }
        .padding(.leading, 16)
        .padding(.vertical, 8)
        .background(isCurrentlyPlaying ? Color.white : Color.clear)
        .foregroundColor(isCurrentlyPlaying ? .black : .white)
    }
}
```

### 3. List Container with Year Navigation

Add optional year-based scrolling/navigation:

```swift
struct EpisodeListView: View {
    @State private var episodes: [Episode]
    @State private var selectedYear: Int?
    
    var body: some View {
        ScrollViewReader { scrollProxy in
            List(episodes) { episode in
                EpisodeRowView(
                    episode: episode,
                    allEpisodes: episodes,
                    isCurrentlyPlaying: episode.id == currentlyPlayingId,
                    isCompleted: completedEpisodes.contains(episode.id)
                )
                .id(episode.id)
                .onTapGesture {
                    playEpisode(episode)
                }
            }
            .listStyle(.plain)
            .background(Color.black)
            .onChange(of: selectedYear) { newYear in
                if let newYear = newYear,
                   let firstEpisode = episodes.first(where: { $0.year == newYear }) {
                    withAnimation {
                        scrollProxy.scrollTo(firstEpisode.id, anchor: .top)
                    }
                }
            }
        }
    }
}
```

### 4. Optional: Year Jump Menu

Add a subtle year picker/menu (perhaps as a long-press action or separate control):

```swift
struct YearJumpMenu: View {
    let years: [Int]
    @Binding var selectedYear: Int?
    
    var body: some View {
        Menu {
            ForEach(years, id: \.self) { year in
                Button("\(year)") {
                    selectedYear = year
                }
            }
        } label: {
            Image(systemName: "calendar")
                .font(.system(size: 20))
                .foregroundColor(.white.opacity(0.6))
        }
    }
}
```

## Accessibility Considerations

1. **VoiceOver Support:**
   - Year labels announce as "Year 2012", "Year 2013", etc.
   - Episode rows announce with context: "Episode 1, Year 2012, July 4, Jeff Davis"
   
2. **Dynamic Type:**
   - Year labels scale with user's font size preferences
   - Maintain relative size difference to episode numbers
   
3. **Voice Control:**
   - Year labels are tappable targets: "Tap 2015"
   - Episode rows remain individually tappable

## Animation & Interaction

### Subtle Interactions:
1. **Year Label Fade-in:** 
   - When scrolling into view, year labels fade in from 0.3 to 0.6 opacity (0.2s ease)
   
2. **Touch Feedback:**
   - Year label briefly increases opacity to 0.8 on tap
   - Haptic feedback (light impact) on year label tap
   
3. **Scroll-to Animation:**
   - When jumping to a year, smooth scroll with .easeInOut curve (0.4s)
   - Briefly highlight the target year label (pulse effect, 0.3s)

## Edge Cases & Considerations

1. **Missing Years:**
   - 2020 appears to be missing from the dataset
   - Handle gracefully - no special UI needed, just natural gap
   
2. **Single Episode Years:**
   - 2021 has only 1 episode (the finale)
   - Year label still appears, same styling
   
3. **Filter Interactions:**
   - When "HIDE PLAYED" filter is active:
     - If all episodes from a year are hidden, hide the year label
     - If first episode of year is hidden, move year label to first visible episode of that year
   
4. **Prestige System:**
   - Year labels remain unchanged during prestige resets
   - Episodes reset but chronology stays the same
   
5. **Search/Filter Results:**
   - If implementing search, maintain year labels in results
   - Could add "X episodes in 2015" summary in search context

## Testing Checklist

- [ ] Year labels appear only on first episode of each year
- [ ] Alignment is consistent for all episodes
- [ ] Completed episodes maintain proper year label visibility
- [ ] Currently playing episode (inverted colors) doesn't break year label layout
- [ ] VoiceOver reads year labels correctly
- [ ] Tapping year labels provides feedback (if interactive)
- [ ] Year labels respect HIDE PLAYED filter
- [ ] Layout works on various iPhone screen sizes (SE, Pro, Pro Max)
- [ ] Dark mode compatibility (already black background)
- [ ] Landscape orientation maintains layout integrity

## Future Enhancements (Optional)

1. **Sticky Year Headers:**
   - Year label could "stick" to top of screen while scrolling through that year's episodes
   - iOS-style section headers behavior
   
2. **Year Statistics:**
   - Long-press on year label to show "2015: 40 episodes, 35 completed"
   - Contextual quick stats
   
3. **Timeline Scrubber:**
   - Vertical timeline on far left showing all years
   - Drag to quickly jump between years
   - Think iOS Photos app year scrubber
   
4. **Year Badges:**
   - Small badge showing completion percentage per year
   - "2015: 87%" in tiny text below year label

## Implementation Priority

**Phase 1 (MVP):**
- ✅ Year labels on first episode of each year
- ✅ Proper spacing and alignment
- ✅ Basic accessibility support

**Phase 2 (Polish):**
- Optional: Tap to scroll behavior
- Subtle animations
- Enhanced accessibility

**Phase 3 (Advanced):**
- Year jump menu
- Sticky headers
- Statistics overlay

## Code Files to Modify

Based on BUILD_INSTRUCTIONS.md structure:
- `TownTuner/ContentView.swift` - Main episode list
- `TownTuner/Episode.swift` (or model file) - Add year detection logic
- `TownTuner/EpisodeRowView.swift` (if separated) - Update row layout

## Visual Mockup Reference

```
┌─────────────────────────────────────┐
│  TOWNTUNER         Prestige: I      │
│  [HIDE PLAYED]                      │
├─────────────────────────────────────┤
│                                     │
│ 2012  [#001]  2012.07.04           │
│       Jeff Davis                    │
│       ─────────────────────────     │
│       [#002]  2012.07.20           │
│       Jeff Davis                    │
│       ─────────────────────────     │
│       [#003]  2012.07.30           │
│       Jeff Davis                    │
│       ─────────────────────────     │
│       ...                           │
│       [#016]  2012.12.05           │
│       Jeff Davis                    │
│       ─────────────────────────     │
│                                     │
│ 2013  [#017]  2013.01.05           │
│       Jeff Davis                    │
│       ─────────────────────────     │
│       [#018]  2013.01.06           │
│       Jeff Davis                    │
│       ─────────────────────────     │
│       ...                           │
│                                     │
└─────────────────────────────────────┘
```

## Conclusion

This feature enhances navigation and temporal awareness in the episode list while maintaining the brutalist aesthetic and "2 AM with a cat on your chest" usability principle. The implementation is straightforward in SwiftUI and provides meaningful value for users working through 360 episodes chronologically.

The year labels serve as both passive wayfinding (showing where you are) and active navigation (potential jump points), making it easier to skip to specific eras of Harmontown or understand the show's timeline at a glance.
