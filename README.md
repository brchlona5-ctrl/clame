# Accessibility Widget - Build Prompt

## Reference Screenshots
All screenshots are in the `screenshots/` folder of this repo. Use them alongside this prompt.

| File | What it shows |
|------|--------------|
| 01-full-panel-default.png | Full widget panel — default state, no active buttons, profiles accordion closed |
| 02-profiles-open-all8.png | Profiles accordion open — all 8 profile cards visible |
| 03-motor-impaired-active-voice-input-speech.png | Motor Impaired profile active (red fill) + Voice Input active (red border + checkmark) + Speech Recognition popup open |
| 04-buttons-bottom-half-reset.png | Bottom half of buttons: Line Height, Saturation, Big Cursor, Contrast+, Reading Mask, Dyslexia Friendly, Tooltips, Pause Animations + Reset button |
| 05-sign-language-video-modal.png | Sign Language Video modal open: video player + Arabic subtitle + title + pagination 1/1 + X close |
| 06-bigger-text-2-multimode.png | Bigger Text button in mode 2 — showing multi-mode pagination dots |
| 07-screen-reader-read-normal.png | Full widget with Screen Reader active |
| 08-contrast-plus-invert-colors.png | Contrast+ in "Invert Colors" mode — icon changes to monitor/image icon |

---

## PROMPT FOR CLAUDE — BUILD IDENTICAL ACCESSIBILITY WIDGET

Build a fully self-contained, encapsulated accessibility widget as a **Shadow DOM Web Component** that is pixel-perfect identical to the one shown in the reference screenshots above.

---

### TECHNICAL ARCHITECTURE

- Implement as a **Shadow DOM Web Component** using `customElements.define()`
- All CSS must be **fully encapsulated** inside the shadow root (no leakage to host page)
- Mount as a single custom element: `<dameg-accessibility-widget>`
- The widget renders a **floating trigger button** (bottom-right corner) that opens/closes the panel
- The panel is **fixed position**, right side of viewport, full viewport height

---

### WIDGET TRIGGER BUTTON

- Fixed position: bottom-right corner (right: 20px, bottom: 20px)
- Circular button, ~56px diameter
- Background: red (#E60000)
- Icon: wheelchair/accessibility icon (white SVG)
- On click: toggles the panel open/closed

---

### PANEL LAYOUT

- **Width:** 400px
- **Height:** 100vh (full viewport height)
- **Position:** fixed, right: 0, top: 0
- **Background:** white (#FFFFFF)
- **Box shadow:** left-side shadow
- **Structure (top to bottom):**
  1. Header (fixed, red)
  2. Scrollable content area
     - Accessibility Profiles accordion
     - 2-column button grid (18 buttons)
  3. Footer (fixed, red Reset button)

---

### HEADER

- **Background:** #E60000 (Vodafone red)
- **Height:** ~60px
- **Layout:** flex row, space-between, vertically centered
- **Left:** Language selector button — globe icon + "EN" text + chevron-down arrow, white pill background (~36px tall, rounded)
- **Center:** Title text "Accessibility Menu (ALT + A)" — white, bold, ~16px
- **Right:** Close (X) button — circular, dark red/maroon background, white X icon, ~36px

---

### ACCESSIBILITY PROFILES ACCORDION

- Full-width row below header
- **Closed state:** Single row with wheelchair icon + "Accessibility Profiles" text + chevron-down arrow
- **Background:** white, 1px border-bottom
- **On click:** Expands to show 8 profile cards in 2-column grid
- **Open state:** Chevron rotates to chevron-up

**8 Profile Cards (2 columns × 4 rows):**
| Row | Left | Right |
|-----|------|-------|
| 1 | Motor Impaired (wheelchair icon) | Blind (waveform icon) |
| 2 | Color Blind (droplet icon) | Dyslexia (A= icon) |
| 3 | Low Vision (eye-off icon) | Cognitive Learning (puzzle piece icon) |
| 4 | Seizure Epileptic (brain icon) | ADHD (circle icon) |

**Profile Card Styling:**
- White background, rounded corners (~8px), subtle border
- Icon (left, ~24px) + label text (right)
- On hover: light grey background
- **Active state:** Full red (#E60000) background, white icon + white text, no border

**Profile Behaviors (what each activates automatically):**
- **Motor Impaired** → activates Voice Input button + opens Speech Recognition popup
- **Blind** → activates Screen Reader (Read Normal mode)
- **Color Blind** → activates Saturation (Desaturate mode)
- **Dyslexia** → activates Dyslexia Friendly button
- **Low Vision** → activates Bigger Text (level 2) + Contrast+ (Dark Contrast)
- **Cognitive Learning** → activates Reading Mask
- **Seizure Epileptic** → activates Pause Animations
- **ADHD** → activates Reading Mask + Highlight Links

---

### BUTTON GRID — 18 BUTTONS

Layout: 2-column CSS grid, equal columns, ~8px gap, inside scrollable area.

**Default Button Styling:**
- White background
- Rounded corners (~12px)
- 1px solid light grey border (#E5E7EB)
- Flex column: icon (top, ~32px) + label text (bottom, ~13px, dark grey)
- ~110px tall
- On hover: light grey background (#F9FAFB)
- Info icon (ⓘ) appears top-right on hover for multi-mode buttons

**Active Button Styling:**
- Background: light grey (#F3F4F6)
- Border: 2px solid #E60000 (red)
- Checkmark circle icon (⊙ red) appears top-left
- Label text changes to current mode name (see below)
- Pagination dots appear at bottom (filled dot = current mode, grey = other modes)

**Pagination Dots:**
- Small circles (~6px), horizontal row, centered at bottom of button
- Active/current mode dot: dark (#1F2937)
- Inactive mode dots: light grey (#D1D5DB)

---

### ALL 18 BUTTONS — COMPLETE SPECIFICATION

#### 1. Voice Input
- **Icon:** Microphone (lucide: Mic)
- **Type:** Simple toggle (ON/OFF)
- **ON behavior:** Opens Speech Recognition floating panel on page

**Speech Recognition Popup:**
- Floating panel, ~400px wide, positioned near widget
- Header: download/minimize icon (left) + "Speech Recognition" title (center)
- Blue info banner: "Click inside the input field where you want to type"
- Large grey textarea: placeholder "Start speaking..."
- Bottom-right: circular microphone button (~48px, grey background)

#### 2. Screen Reader
- **Icon:** Waveform/audio waves (lucide: AudioWaveform)
- **Type:** Multi-mode (3 modes + OFF)
- **Modes:** Read Normal → Read Fast → Read Slow → OFF
- **Dots:** 3 pagination dots

#### 3. Sign Language Video
- **Icon:** Play/video (lucide: MonitorPlay or similar)
- **Type:** Modal launcher (not a toggle — opens video modal)
- **ON behavior:** Opens Sign Language Video modal overlay

**Sign Language Video Modal:**
- Centered overlay modal with backdrop
- Header: video camera icon + "Sign Language Videos" title + X close button
- Native HTML5 video player (~670px wide)
- Arabic subtitle captions shown on video
- Below video: title "Sign Language - لغة الإشارة"
- Pagination: "1 / 1" centered
- No previous/next buttons (only 1 video)

#### 4. Sign Language
- **Icon:** Hand gesture (lucide: HandMetal or similar)
- **Type:** Multi-mode (2 modes + OFF)
- **Modes:** Full Page → Text Selection → OFF
- **Dots:** 2 pagination dots

#### 5. Bigger Text
- **Icon:** Large "TT" text size icon (lucide: CaseSensitive or TextCursorInput)
- **Type:** Multi-mode (4 modes + OFF)
- **Modes:** Bigger Text 1 → Bigger Text 2 → Bigger Text 3 → Bigger Text 4 → OFF
- **Dots:** 4 pagination dots
- **Effect:** Increases font-size of all page text progressively

#### 6. Highlight Links
- **Icon:** Chain/link (lucide: Link2)
- **Type:** Simple toggle (ON/OFF)
- **Effect:** Adds visible highlight/underline style to all `<a>` elements on page

#### 7. Hide Images
- **Icon:** Image with X (lucide: ImageOff)
- **Type:** Simple toggle (ON/OFF)
- **Effect:** Sets `visibility: hidden` or `display: none` on all `<img>` elements

#### 8. Text Spacing
- **Icon:** Letter A with spacing lines (lucide: ALargeSmall or spacing icon)
- **Type:** Multi-mode (3 modes + OFF)
- **Modes:** Text Spacing 1 → Text Spacing 2 → Text Spacing 3 → OFF
- **Dots:** 3 pagination dots

#### 9. Text Alignment
- **Icon:** Three horizontal lines (lucide: AlignCenter or similar)
- **Type:** Multi-mode (3 modes, loops)
- **Modes:** Right → Center → Left (loops back to Right)
- **Dots:** 3 pagination dots
- **Effect:** Changes `text-align` on page body

#### 10. Bold Text
- **Icon:** Bold "B" (lucide: Bold)
- **Type:** Multi-mode (3 modes + OFF)
- **Modes:** Bold Text 1 → Bold Text 2 → Bold Text 3 → OFF
- **Dots:** 3 pagination dots

#### 11. Line Height
- **Icon:** Lines with up-down arrows (lucide: ListCollapse or similar)
- **Type:** Multi-mode (3 modes + OFF)
- **Modes:** Line Height 1 → Line Height 2 → Line Height 3 → OFF
- **Dots:** 3 pagination dots

#### 12. Saturation
- **Icon:** Two overlapping circles (lucide: Blend or Aperture)
- **Type:** Multi-mode (3 modes + OFF)
- **Modes:** Low Saturation → High Saturation → Desaturate → OFF
- **Dots:** 3 pagination dots
- **Effect:** Applies CSS `filter: saturate()` to page

#### 13. Big Cursor
- **Icon:** Arrow cursor (lucide: MousePointer2)
- **Type:** Simple toggle (ON/OFF)
- **Effect:** Injects a large custom cursor div that visually follows the mouse; hides native cursor (`cursor: none` on body)

#### 14. Contrast +
- **Icon (default):** Half-filled circle (lucide: CircleHalf or Contrast)
- **Type:** Multi-mode (3 modes + OFF) — **ICON CHANGES PER MODE**
- **Modes & Icons:**
  - Mode 1: "Invert Colors" → icon changes to monitor with image (lucide: MonitorSmartphone or ImageIcon)
  - Mode 2: "Dark Contrast" → icon stays or changes
  - Mode 3: "Light Contrast" → icon changes
  - OFF → reverts to original half-circle icon
- **Dots:** 3 pagination dots
- **Effect:** Applies CSS filters to `<html>` or `<body>`

#### 15. Reading Mask
- **Icon:** Text with scan lines (lucide: ScanText or similar)
- **Type:** Multi-mode (2 modes + OFF)
- **Modes:** Reading Mask → Reading Line → OFF
- **Dots:** 2 pagination dots
- **Effect:** Overlays a semi-transparent mask that follows mouse vertically, highlighting the current line

#### 16. Dyslexia Friendly
- **Icon:** A= with baseline (lucide: Baseline or similar)
- **Type:** Simple toggle (ON/OFF)
- **Effect:** Applies OpenDyslexic or similar dyslexia-friendly font to page text

#### 17. Tooltips
- **Icon:** Speech bubble with exclamation (lucide: MessageSquareWarning or similar)
- **Type:** Simple toggle (ON/OFF)
- **Effect:** Enables enhanced tooltips on all elements that have `title` or `aria-label` attributes

#### 18. Pause Animations
- **Icon:** Pause symbol in circle (lucide: CirclePause)
- **Type:** Simple toggle (ON/OFF)
- **Effect:** Injects CSS `*, *::before, *::after { animation: none !important; transition: none !important; }`

---

### FOOTER — RESET BUTTON

- Full width, inside panel at bottom (sticky/fixed within panel)
- **Background:** #E60000 (red)
- **Text:** "Reset Accessibility Settings" with a refresh/rotate icon (left of text)
- **Text color:** white, bold
- **Height:** ~56px
- **Border radius:** ~8px
- **On click:** Immediately deactivates ALL 18 buttons, resets all modes to default, removes all page modifications, closes any open modals/popups

---

### LANGUAGE NOTE (bottom of panel)
Below the Reset button, outside the scrollable area:
- Small grey text: "Please confirm changing the language when switching the website to ensure all features work properly."
- Same text in Arabic below it

---

### COLORS
| Element | Color |
|---------|-------|
| Header background | #E60000 |
| Reset button background | #E60000 |
| Active button border | #E60000 |
| Active profile card background | #E60000 |
| Checkmark icon (active) | #E60000 |
| Panel background | #FFFFFF |
| Button default border | #E5E7EB |
| Button active background | #F3F4F6 |
| Label text (default) | #374151 |
| Pagination dot (active) | #1F2937 |
| Pagination dot (inactive) | #D1D5DB |
| Header text | #FFFFFF |

---

### ICONS
Use **Lucide icons** (https://lucide.dev) for all icons. Import via CDN:
```html
<script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
```

---

### IMPLEMENTATION NOTES
1. All page-modifying effects (font size, contrast, cursor, etc.) must apply to the **host page DOM**, not the shadow root
2. The widget itself must NOT be affected by its own accessibility features
3. Use `document.documentElement` or `document.body` as targets for page-level CSS changes
4. Inject a single `<style>` tag into `document.head` for page-level overrides, with a unique class prefix (e.g., `dameg-`) to avoid conflicts
5. The big cursor overlay should be a `position:fixed, top:0, left:0, pointer-events:none, z-index:99999` div injected into `document.body`
6. Reading Mask should be a `position:fixed` div following `mousemove` events
7. Speech Recognition popup should use the Web Speech API (`window.SpeechRecognition`)
8. All state should be stored in a JS object and the Reset button clears all state keys

---

*Reference: Vodafone Egypt accessibility widget — https://web.vodafone.com.eg/en/home*
