# Year Labels - Quick Implementation Guide

This guide provides step-by-step instructions for implementing year labels in the TownTuner episode list.

## Step 1: Update Episode Model

Add year calculation to your Episode model:

```swift
extension Episode {
    var year: Int {
        Calendar.current.component(.year, from: date)
    }
}

// Pre-calculate year label visibility for performance
struct EpisodeMetadata {
    let episode: Episode
    let showYearLabel: Bool
}

func prepareEpisodeMetadata(_ episodes: [Episode]) -> [EpisodeMetadata] {
    episodes.enumerated().map { index, episode in
        let showLabel = index == 0 || episode.year != episodes[index - 1].year
        return EpisodeMetadata(episode: episode, showYearLabel: showLabel)
    }
}
```

**Performance Note**: Pre-calculating year labels avoids O(n) lookups on each row render. With 360 episodes, this is critical for smooth scrolling.
```

## Step 2: Update Episode Row View

Modify the episode row to include year labels using pre-calculated metadata:

```swift
struct EpisodeRowView: View {
    let episode: Episode
    let showYearLabel: Bool  // Pre-calculated, not computed per render
    let isCurrentlyPlaying: Bool
    let isCompleted: Bool
    
    var body: some View {
        HStack(alignment: .top, spacing: 12) {
            // Year label column (44pt wide)
            Group {
                if showYearLabel {
                    Text("\(episode.year)")
                        .font(.system(.body, design: .monospaced))
                        .fontWeight(.bold)
                        .foregroundColor(.white.opacity(0.6))
                        .accessibilityLabel("Year \(episode.year)")
                } else {
                    Color.clear
                }
            }
            .frame(width: 44, alignment: .leading)
            
            // Episode content
            VStack(alignment: .leading, spacing: 4) {
                // Episode number and date
                HStack {
                    Text("[\(String(format: "#%03d", episode.number))]")
                        .font(.system(.body, design: .monospaced))
                    
                    Spacer()
                    
                    Text(formatDate(episode.date))
                        .font(.system(.caption, design: .monospaced))
                }
                
                // Comptroller name
                Text(episode.comptroller)
                    .font(.system(.caption))
                    .foregroundColor(.white.opacity(0.7))
                
                // Separator line
                Divider()
                    .background(Color.white.opacity(0.2))
            }
            .opacity(isCompleted ? 0.5 : 1.0)
        }
        .padding(.leading, 16)
        .padding(.trailing, 16)
        .padding(.vertical, 8)
        .background(isCurrentlyPlaying ? Color.white : Color.clear)
        .foregroundColor(isCurrentlyPlaying ? .black : .white)
    }
    
    private func formatDate(_ date: Date) -> String {
        let formatter = DateFormatter()
        formatter.dateFormat = "yyyy.MM.dd"
        return formatter.string(from: date)
    }
}
```

## Step 3: Update List View (Optional Scroll-to-Year)

Add year-based navigation using pre-calculated metadata:

```swift
struct EpisodeListView: View {
    @State private var episodes: [Episode]
    @State private var completedEpisodeIds: Set<Int>
    @State private var selectedYear: Int?
    
    // Pre-calculate episode metadata
    private var episodeMetadata: [EpisodeMetadata] {
        prepareEpisodeMetadata(episodes)
    }
    
    // Get unique years from episodes
    private var availableYears: [Int] {
        Array(Set(episodes.map { $0.year })).sorted()
    }
    
    var body: some View {
        VStack(spacing: 0) {
            // Header with optional year picker
            HStack {
                Text("TOWNTUNER")
                    .font(.system(.title3, design: .monospaced))
                    .fontWeight(.bold)
                
                Spacer()
                
                // Optional: Year jump menu
                yearPickerMenu
                
                // Prestige counter
                Text("Prestige: \(prestigeLevel)")
                    .font(.system(.caption, design: .monospaced))
            }
            .padding()
            
            // Episode list with year labels
            ScrollViewReader { scrollProxy in
                List(episodeMetadata, id: \.episode.id) { metadata in
                    EpisodeRowView(
                        episode: metadata.episode,
                        showYearLabel: metadata.showYearLabel,
                        isCurrentlyPlaying: metadata.episode.id == currentlyPlayingId,
                        isCompleted: completedEpisodeIds.contains(metadata.episode.id)
                    )
                    .id(metadata.episode.id)
                    .listRowInsets(EdgeInsets())
                    .listRowBackground(Color.black)
                    .onTapGesture {
                        playEpisode(metadata.episode)
                    }
                }
                .listStyle(.plain)
                .background(Color.black)
                .onChange(of: selectedYear) { newYear in
                    scrollToYear(newYear, using: scrollProxy)
                }
            }
        }
        .background(Color.black)
    }
    
    private var yearPickerMenu: some View {
        Menu {
            ForEach(availableYears, id: \.self) { year in
                Button(action: { selectedYear = year }) {
                    Text("\(year)")
                }
            }
        } label: {
            Image(systemName: "calendar")
                .font(.system(size: 18))
                .foregroundColor(.white.opacity(0.6))
        }
    }
    
    private func scrollToYear(_ year: Int?, using scrollProxy: ScrollViewProxy) {
        guard let year = year,
              let firstEpisode = episodes.first(where: { $0.year == year }) else {
            return
        }
        
        withAnimation(.easeInOut(duration: 0.4)) {
            scrollProxy.scrollTo(firstEpisode.id, anchor: .top)
        }
        
        // Haptic feedback
        let impactFeedback = UIImpactFeedbackGenerator(style: .light)
        impactFeedback.impactOccurred()
    }
}
```

## Step 4: Handle Filter Interactions

When "HIDE PLAYED" filter is active, re-calculate year label visibility for filtered episodes:

```swift
// In your view model or state management
func prepareFilteredEpisodes(allEpisodes: [Episode], hidePlayedEnabled: Bool, completedEpisodeIds: Set<Int>) -> [(episode: Episode, showYearLabel: Bool)] {
    // First filter the episodes if needed
    let visibleEpisodes = hidePlayedEnabled 
        ? allEpisodes.filter { !completedEpisodeIds.contains($0.id) }
        : allEpisodes
    
    // Then calculate year labels based on visible episodes
    return visibleEpisodes.enumerated().map { index, episode in
        let showLabel = index == 0 || episode.year != visibleEpisodes[index - 1].year
        return (episode, showLabel)
    }
}
```

Update your view to use pre-calculated metadata:

```swift
struct EpisodeListView: View {
    @State private var episodes: [Episode]
    @State private var completedEpisodeIds: Set<Int>
    @State private var hidePlayedEnabled: Bool
    
    private var episodeMetadata: [(episode: Episode, showYearLabel: Bool)] {
        prepareFilteredEpisodes(
            allEpisodes: episodes,
            hidePlayedEnabled: hidePlayedEnabled,
            completedEpisodeIds: completedEpisodeIds
        )
    }
    
    var body: some View {
        List(episodeMetadata, id: \.episode.id) { metadata in
            EpisodeRowView(
                episode: metadata.episode,
                showYearLabel: metadata.showYearLabel,
                isCurrentlyPlaying: metadata.episode.id == currentlyPlayingId,
                isCompleted: completedEpisodeIds.contains(metadata.episode.id)
            )
        }
    }
}

struct EpisodeRowView: View {
    let episode: Episode
    let showYearLabel: Bool
    let isCurrentlyPlaying: Bool
    let isCompleted: Bool
    
    // ... rest of the view
}
```

## Step 5: Add Subtle Animations (Optional)

Enhance the year label with subtle interactions:

```swift
struct YearLabel: View {
    let year: Int
    @State private var isHighlighted = false
    
    var body: some View {
        Text("\(year)")
            .font(.system(.body, design: .monospaced))
            .fontWeight(.bold)
            .foregroundColor(.white.opacity(isHighlighted ? 0.8 : 0.6))
            .accessibilityLabel("Year \(year)")
            .onTapGesture {
                withAnimation(.easeInOut(duration: 0.2)) {
                    isHighlighted = true
                }
                
                DispatchQueue.main.asyncAfter(deadline: .now() + 0.2) {
                    withAnimation(.easeInOut(duration: 0.2)) {
                        isHighlighted = false
                    }
                }
                
                // Haptic feedback
                let impactFeedback = UIImpactFeedbackGenerator(style: .light)
                impactFeedback.impactOccurred()
            }
    }
}
```

Then use it in your row:

```swift
// In EpisodeRowView
if showYearLabel {
    YearLabel(year: episode.year)
} else {
    Color.clear
}
```

## Testing Checklist

Once implemented, verify:

- [ ] Year labels appear only on first episode of each year
- [ ] Proper alignment maintained across all screen sizes
- [ ] Year labels visible for: 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2021
- [ ] Filter toggle (HIDE PLAYED) adjusts year labels correctly
- [ ] Currently playing episode (inverted colors) displays correctly with year label
- [ ] Completed episodes (0.5 opacity) show year labels properly
- [ ] VoiceOver announces year labels as "Year 2012", etc.
- [ ] Optional: Tapping year label provides feedback
- [ ] Optional: Year picker menu scrolls to correct episode

## Visual Verification

Your episode list should look like this:

```
┌───────────────────────────────────┐
│ 2012  [#001]  2012.07.04         │
│       Jeff Davis                  │
│       ───────────────────────     │
│       [#002]  2012.07.20         │
│       Jeff Davis                  │
│       ───────────────────────     │
│                                   │
│ 2013  [#017]  2013.01.05         │
│       Jeff Davis                  │
│       ───────────────────────     │
```

## Common Issues & Solutions

### Issue: Year labels not showing
**Solution:** Ensure episodes are sorted chronologically and year label metadata is pre-calculated correctly

### Issue: Year labels break with filter
**Solution:** Re-calculate metadata when filter changes using `prepareFilteredEpisodes()` function

### Issue: Performance issues with large episode list
**Solution:** Always use pre-calculated `EpisodeMetadata` - never compute year labels in view body or computed properties

## Performance Considerations

- Pre-calculate year label visibility when loading episodes to avoid O(n) lookups on each row render
- Use pre-computed metadata instead of calculating `isFirstOfYear` for each row:

```swift
// GOOD: Pre-calculate once when episodes load or filter changes
struct EpisodeMetadata {
    let episode: Episode
    let showYearLabel: Bool
}

func prepareEpisodes(_ episodes: [Episode]) -> [EpisodeMetadata] {
    episodes.enumerated().map { index, episode in
        let showLabel = index == 0 || episode.year != episodes[index - 1].year
        return EpisodeMetadata(episode: episode, showYearLabel: showLabel)
    }
}

// Then use in your List:
List(episodeMetadata, id: \.episode.id) { metadata in
    EpisodeRowView(
        episode: metadata.episode,
        showYearLabel: metadata.showYearLabel,
        // ...
    )
}
```

This approach is O(n) once when loading/filtering instead of O(n) per row render, resulting in smooth scrolling even with 360 episodes.
```

## Files Modified

Based on typical TownTuner project structure:
- `TownTuner/Models/Episode.swift` - Add year property and isFirstOfYear method
- `TownTuner/Views/EpisodeRowView.swift` - Add year label to row layout
- `TownTuner/Views/ContentView.swift` - Update list view with optional navigation
- `TownTuner/Views/Components/YearLabel.swift` (optional) - Separate year label component

## Next Steps

1. Implement basic year labels (Steps 1-2)
2. Test with all 360 episodes
3. Add optional scroll-to-year functionality (Step 3)
4. Enhance with filter support (Step 4)
5. Polish with animations (Step 5)
6. Submit for code review

For complete design specifications, see `YEAR_LABELS_SPEC.md`.
