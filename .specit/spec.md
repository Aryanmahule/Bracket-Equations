# Specification: Bracket Equations (Grade 7 / Lower Secondary)

## 1. Overview & Objective
- **Title**: Algebra Quest: Bracket Equations
- **Target Audience**: Grade 7 / Secondary 1 Mathematics (aligned with Singapore MOE curriculum)
- **Theme**: Singapore Urban & Classroom setting with **Maya** (curious math student) and **Archie** (friendly orange fox companion)
- **Metaphor**: The Distributive Law is like a delivery robot knocking on every door inside the house: $a(b + c) = ab + ac$. An outside multiplier groups items into identical bundles.
- **Goal**: Master the foundational principles of equations with brackets:
  1. Distributive property ($a(b + c) = ab + ac$) and expanding brackets cleanly.
  2. Solving 1-step and 2-step bracket equations ($a(x + b) = c$) using two strategic paths: expanding first vs dividing first.
  3. Handling negative multipliers outside brackets (e.g., $-2(x - 5) = 14 \implies -2x + 10 = 14$).
  4. Solving equations with brackets on both sides ($a(x + b) = c(x + d)$).
  5. Translating real-world Singapore word scenarios into bracket equations.

---

## 2. Architecture & UI Shell
- **Viewport**: Single-screen fixed `100vh` layout (`overflow: hidden`) on all lesson phases (`wonder`, `story`, `stations`, `practice`, `reflect`, `summary`). `intro` allows smooth scrolling on mobile/compact viewports. Breakpoint: `@media (max-width: 868px)`.
- **Top Header Bar**:
  - `[ 🏠 Home ]` button (top-left) jumps back to Intro screen, stopping narration.
  - XP counter badge (`⭐ {xp} XP`) immediately following Home.
  - 5-Step Journey Bar (top-center):
    - `01 ❄️ Wonder`
    - `02 📖 Story`
    - `03 ⚙️ Simulate`
    - `04 🎯 Practice`
    - `05 🎖️ Reflect`
  - Audio Mute Toggle `[ 🔊 / 🔇 ]` located immediately adjacent to the journey bar.
  - Cyan Square Close Button `[ ✕ ]` on the top-right.
- **Visual Design System**:
  - Deep royal purple radial gradient background (`#29104e` -> `#15062a` -> `#0c0217`)
  - Glassmorphic panels with purple border glow (`rgba(168, 85, 247, 0.35)`)
  - Accent colors: Radiant Gold (`#ffb800`), Cyan/Teal (`#00e5ff`), Coral (`#ff5e62`), Emerald (`#10b981`)
  - Typography: `Outfit` (Primary UI), `Fredoka` (Hero Titles & Badges), `Space Grotesk` (Monospace & Math variables)
  - Floating background watermarks: `54 100`, `91 11`, `66`, `30 500`, `69`, `90 347`, `x`, `a`, `n`, `200`, `123 999`, and equation watermarks `( )`, `[ ]`, `{ }`, `a(b + c) = ab + ac`, `3(x + 4) = 24`, `4(x − 2) = 20`.

---

## 3. State Management & Persistence
- **Storage Key**: `intellia_bracket_equations_v1`
- **State Schema**:
  ```ts
  interface ModuleState {
    phase: 'intro' | 'wonder' | 'story' | 'stations' | 'practice' | 'reflect' | 'summary';
    xp: number;
    streak: number;
    highestStreak: number;
    completedPhases: string[];
    stationsProgress: {
      station1: boolean;
      station2: boolean;
      station3: boolean;
    };
    practiceStats: {
      attempted: number;
      correct: number;
      currentStreak: number;
      bestStreak: number;
    };
    reflectionAnswers: Record<string, string>;
    isMuted: boolean;
  }
  ```
- **Actions & Reducers**:
  - `SET_PHASE`, `COMPLETE_PHASE`, `ADD_XP`
  - `COMPLETE_STATION`: records completion of individual stations and awards +50 XP.
  - `RECORD_PRACTICE`: records attempted, correct, streak, highest streak, and awards scaled XP (+15 XP + streak bonus).
  - `SAVE_REFLECTION`: stores student reflection responses and awards +40 XP.
  - `TOGGLE_MUTE`, `RESET_ALL`.

---

## 4. Phase Details & Content

### 4.1 Intro Screen (Exact Match to Reference UI)
- Top Bar: `✦ MOE Curriculum • Grade 7` pill badge + cyan square `✕` close button.
- Hero Title:
  - `Algebra Quest` (White, Fredoka bold)
  - `Bracket Equations` (Radiant yellow glow, Fredoka bold)
- Mascot Pill: Circular avatar with orange fox (`🦊`) + white speech pill: `Ready to crack the code of brackets? 🔐` (clickable with voice audio).
- Mission Subtitle: *"Join Archie & Maya on a mission to expand, simplify, and solve equations with brackets using the distributive property, balance scales, and real-life mysteries!"*
- Learning Journey Glass Card:
  - Top Label: `YOUR LEARNING JOURNEY` (yellow monospace)
  - Row 1: `🔍 Wonder (Spark curiosity)` $\to$ `📖 Story (Hear the tale)` $\to$ `🧪 Simulate (Explore & discover)`
  - Row 2: `🎮 Practice (Test your skills)` $\to$ `📓 Reflect (What did you learn?)`
- Call to Action Button: `🚀 Begin Your Journey!` (Large orange-gold gradient with glowing shadow).
- Bottom 3 Feature Cards:
  1. Blue Badge `Letters & Terms`: Distributive expansion
  2. Red Badge `Simulations`: Interactive labs
  3. Gold Badge `Game Worlds`: 10 Game Worlds / XP & awards

### 4.2 Phase 01: Wonder (Curiosity Accordion & Interactive Transformer)
- Interactive Toggle: Compare $2 + 3 \times 4 = 14$ vs $(2 + 3) \times 4 = 20$.
- 3 interactive mystery cards with spoken narration by Alice:
  1. **Why do brackets exist?** (Brackets overrule standard calculation priority, forcing addition before multiplication).
  2. **Does a number outside brackets multiply everything inside?** (Yes, the distributive knock visits every term: $3(x + 4) = 3x + 12$).
  3. **For 4(x + 2) = 24, should you expand first or divide first?** (Both paths lead to $x = 4$; dividing first is a fast shortcut when clean).

### 4.3 Phase 02: Story (5 Visual Slides matching Image 2 UI)
- Top yellow progress bar with counter (`1/5` to `5/5`).
- 2-Column main glass card:
  - Left: Scene illustration.
  - Right:
    - Golden-yellow heading.
    - Story narrative paragraph.
    - Sparkle question capsule: `✦ "..." ✦ 🔊` (click to listen).
    - White fox mascot speech pill: `[🦊] Guidance hint`.
- 5 Story Slides:
  1. `slide1_bags.jpg`: **Maya's Gift Bags** — 4 identical gift bags each holding 1 crystal $x$ and 3 coins ($4(x + 3) = 28$).
  2. `slide2_distributive.jpg`: **The Distributive Knock** — $a(b + c) = ab + ac$; rainbow arrows to all terms.
  3. `slide3_twopaths.jpg`: **Two Paths to the Truth** — Expand first vs divide first strategies.
  4. `slide4_negative.jpg`: **The Sneaky Negative Sign** — Distributing negative numbers: $-2(x - 5) = 14 \implies -2x + 10 = 14$.
  5. `slide5_balance.jpg`: **Brackets on Both Sides** — Solving $3(x + 2) = 2(x + 5)$.

### 4.4 Phase 03: Simulate (3 Interactive Stations)
- **Station 1: Distributive Area Model & Place Brackets Challenge**
  - Interactive rectangular area model with factor $a$ (height) and widths $x$ and $b$.
  - "Place Brackets" challenge: insert brackets into $2 + 3 \times 4$ to reach target 20.
- **Station 2: The Two-Way Solver**
  - Step through Method 1 (Expand First) vs Method 2 (Divide First) for $4(x + 3) = 28$.
- **Station 3: Real Singapore Scenes**
  - Sentosa Cable Car tickets ($4(p + 3) = 48$) and Old Seng Choong cookie packs ($5(c + 4) = 45$).

### 4.5 Phase 04: Practice Arena
- 6 procedurally generated question types with step-by-step explanations, streak counter, XP rewards, and confetti celebrations.

### 4.6 Phase 05: Reflect & Summary
- Self-assessment cards, confidence selector, strategy chips, and final "Bracket Master 🛡️" award screen.
