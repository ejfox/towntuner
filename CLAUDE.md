# TOWNTUNER

Development Document for the Imbeciles

A Harmontown Sleep App for the Devoted

---

## PROJECT OVERVIEW

**App Name:** TownTuner  
**Purpose:** A dedicated podcast player for all 360 episodes of Harmontown, designed specifically for nightly sleep listening with gamification elements  
**Platform:** iOS (Swift/SwiftUI)  
**Visual Style:** Brutalist monochrome - black, white, grays only. No colors. No gradients. No mercy.

---

## CORE MECHANICS

### The Prestige System
• Users start at Episode 1 and work through all 360 episodes chronologically  
• Completing all 360 episodes = 1 Prestige  
• Upon prestiging, all episodes reset to unplayed  
• Prestige counter displays permanently (e.g., "Prestige III, Episode 247/360")  
• Each prestige could unlock a new shade of gray for the UI (getting lighter each time until you eventually get white text on white background at Prestige 100)

### Episode Completion Rules
• An episode is marked "complete" when 85% has been played  
• Completed episodes drop to 0.5 opacity in the list  
• Completed episodes are removed from the shuffle pool  
• Progress persists through app closes

---

## SCREEN SPECIFICATIONS

### 1. MAIN EPISODE LIST SCREEN

**Layout:**
• Header: "TOWNTUNER" in brutalist sans-serif, small prestige counter in top right  
• Filter toggle (top left): "HIDE PLAYED" / "SHOW ALL"  
• Episode rows (scrollable list)  
• Big shuffle button (floating, bottom right)

**Episode Row Design:**
```
[#001]  2012.06.16
Jeff Davis
-------------------
[#002]  2012.06.23  
Erin McGathy
-------------------
```

• Episode number in brackets, monospace font  
• Original air date in YYYY.MM.DD format  
• Comptroller name  
• Thin gray line separator between episodes  
• Played episodes: 0.5 opacity  
• Currently playing: Inverted colors (white background, black text)  
• Tappable area: Entire row

**The Big Shuffle Button:**
• Position: Bottom right, 80pt from bottom, 20pt from right  
• Size: 60x60pt circle  
• Icon: Dice or shuffle icon  
• Behavior: Immediately plays random unplayed episode from current prestige  
• Animation: Subtle rotation when pressed  
• If all episodes played: Button disabled, shows "PRESTIGE" instead

### 2. NOW PLAYING SCREEN

**Layout (top to bottom):**
```
[Back Arrow]                           [Queue Icon]

EPISODE #178
2015.12.20

"WIDE"

Comptroller: Jeff Davis
Guests: Kumail Nanjiani, Emily Gordon

[Massive Album Art Placeholder - just episode number in huge text]

[Progress Bar]
23:41 ----------------------o------------ 1:24:36

[15sec back] [Play/Pause - BIG] [30sec forward]

[Playback Speed: 1x]
```

**Controls:**
• Play/pause: 80pt circle, center  
• Skip buttons: 44pt circles  
• Progress bar: Draggable, shows time on left and right  
• Playback speed: Tap to cycle through 0.5x, 0.75x, 1x, 1.25x, 1.5x, 2x  
• All buttons have 48pt minimum touch targets

### 3. HISTORY / LIFE LIST SCREEN

**Layout:**
```
LIFETIME HISTORY
Prestige: III
Total Plays: 1,847
Total Hours: 2,764

[Scrollable list of every play ever]
2025.01.14 - 11:47 PM - Episode #234 (completed)
2025.01.14 - 10:15 PM - Episode #045 (abandoned at 45:23)
2025.01.13 - 11:23 PM - Episode #301 (completed)
[... continues forever ...]
```

**Features:**
• Every single play logged with timestamp  
• Shows whether episode was completed or abandoned (with timestamp)  
• Searchable by date or episode number  
• Export option (dumps to CSV for the real sickos)

### 4. TRANSCRIPT SEARCH SCREEN

**Layout:**
```
[Search Bar: "Search all transcripts..."]

[Recent Searches:]
"moon colony"
"chicken noodle"
"D&D"

[Results:]
Episode #45 - 3 matches
"...talking about moon colony economics..."
[Show More]

Episode #178 - 1 match  
"...moon colony would never work because..."
[Show More]
```

**Technical Note:**
• Integrates with existing embedding API (provided by third party)  
• Shows snippet of text around search match  
• Tap result to jump to episode at that timestamp  
• Search history persists

### 5. GUEST SEARCH SCREEN

**Top Section - Guest Circles:**
```
[Dan] [Jeff] [Spencer] [Erin] [Kumail]
[Rob] [Curtis] [Steve] [Dino] [Brandon]
[More frequent guests in circles...]
```

• 44pt circles with initials or tiny photos  
• Tap to instantly filter to their episodes  
• Arranged by frequency of appearance

**Search Section:**
```
[Search Bar: "Search guests..."]

Results:
Kumail Nanjiani - 23 episodes
#14, #45, #67, #89, #103, #145...
```

### 6. TOWNTUNER WRAPPED (Stats Screen)

**Legitimate Stats:**
```
TOWNTUNER WRAPPED 2025

Total Hours: 1,847
Current Streak: 47 days  
Most Played: Episode #178 (67 times)
Favorite Guest: Kumail (234 hours)
Prestige Level: III
Episodes This Year: 534
```

**Satirical Stats (randomize some each viewing):**
• You fell asleep to Dan's rants: 423 times  
• Jeff made airplane noises for: 73 total hours  
• You're in the top 0.1% of Spencer Crittenden listeners  
• Most played breakdown: Episode #178 (Dan cries about moon colonies)  
• Comptroller Alignment: Jeff Davis Girlie ✓  
• You heard "Jane, you ignorant slut": 89 times  
• Shadowrun mentions endured: 445  
• Times you woke up to audience members trying: 67  
• Your para-social relationship strength: CONCERNING  
• Therapy avoided via podcast: $12,400 worth

**Share Button:**
• Generates image of stats for social media  
• Includes "I prestiged [X] times" as flex

### 7. CREDITS SCREEN

**Layout:**
```
TOWNTUNER
Made with love and desperation

[VODKA BOTTLE CLICKER GAME]
Bottles clicked: 45,847
Global bottles: 45,839,221,094

"Dan, just one show a year.
Please. We'll take anything.
Even a Zoom call."

Days since last episode: 2,047

Created by: [Credits]
Transcript API: [Credit to whoever]
Episode source: Your hard drive, apparently

[Tiny text]: This app is not affiliated with Harmontown, 
Dan Harmon, or anyone who could sue us
```

**Cookie Clicker Mechanics:**
• Tap vodka bottle, it spins and makes a "clink" sound  
• Counter increases  
• Every 100 clicks: Bottle briefly turns gold  
• Every 1000 clicks: Unlock new bottle design  
• Connects to Firebase to update global counter  
• No rewards, just numbers going up

---

## TECHNICAL REQUIREMENTS

### Data Storage
• Episodes stored locally (user provides files)  
• Core Data or SQLite for:
  - Play history
  - Episode progress
  - Prestige count
  - Stats tracking
  - Guest metadata

### Audio Player
• AVFoundation for playbook  
• Background audio support (configure Info.plist)  
• Lock screen controls  
• AirPlay support  
• Remember playback position per episode

### File Structure Expected
```
/Harmontown Episodes/
  001 - Achieve Weightlessness.mp3
  002 - Chicken Noodle Dick.mp3
  ...
  360 - The Last One.mp3
```

### API Integrations
• Transcript search: [Existing embedding API endpoint]  
• Global vodka counter: Simple Firebase counter  
• No other external dependencies

---

## UI/UX PRINCIPLES

### 1. BRUTALIST AESTHETIC
• Black background  
• White text  
• System fonts only (SF Pro Display)  
• No rounded corners over 4pt radius  
• No animations over 0.2 seconds  
• No colors except grayscale

### 2. THUMB-FIRST DESIGN
• All primary actions reachable with right thumb  
• Minimum touch targets: 48x48pt  
• Shuffle button always accessible  
• Designed for half-asleep operation

### 3. INFORMATION HIERARCHY
• Episode number > Date > Comptroller  
• Prestige always visible but subtle  
• Playing state obvious (inverted colors)  
• Unplayed episodes prominent, played episodes ghost

---

## LAUNCH REQUIREMENTS

### MVP Features (Version 1.0)
• All 7 screens functional  
• Prestige system working  
• Local file playback  
• Basic stats tracking  
• Shuffle button

### Post-Launch (Version 1.1)
• Transcript search integration  
• Global vodka counter  
• Wrapped stats sharing  
• iOS widget showing current episode  
• Apple Watch app (just a big shuffle button)

### Dream Features (Version 2.0)
• "Spencer Mode" - only episodes where Spencer DMs  
• "Comptroller Mode" - Jeff episodes vs Spencer episodes  
• Sleep detection - auto-pause if you wake up  
• Dan Harmon detection - sends notification if he mentions the app (he won't)

---

## FINAL NOTES FOR THE IMBECILES

• This is a labor of love, not money  
• No ads, no IAP, no bullshit  
• If Dan's lawyers contact us, immediately capitulate  
• The vodka bottle clicker is non-negotiable  
• Prestige system must feel meaningful but ultimately pointless  
• Every design decision should make someone who's barely conscious go "yeah that makes sense"  
• If you're adding a feature, ask yourself: "Would I use this at 2 AM with a cat on my chest?" If no, delete it

---

## BUILD THIS EXACTLY AS SPECIFIED OR THE GHOST OF HARMONTOWN WILL HAUNT YOUR STANDUPS

End of document. Go forth and code, you beautiful imbeciles.

---

## IMPLEMENTATION STATUS

### ✅ COMPLETED FEATURES
- Core SwiftUI app structure with iOS/macOS platform support
- Episode data model with prestige system tracking
- Main episode list with glass morphing effects
- Now playing screen with professional audio controls
- Audio manager with AVFoundation integration
- Background playback and lock screen controls
- Sleep timer with fade-out functionality
- Prestige system with animated episode resets
- Smart shuffle (unplayed episodes only)
- Episode completion tracking (85% rule)
- Enhanced glass button styles and animations
- App Store compliance preparation
- Bundled sample episode for App Store submission

### 🔄 IN PROGRESS
- History/stats tracking implementation
- Transcript search integration
- Guest search functionality

### 📋 TODO
- TownTuner Wrapped stats screen
- Vodka bottle clicker mini-game
- Firebase global counter integration
- CSV export functionality
- iOS widget development
- Apple Watch companion app