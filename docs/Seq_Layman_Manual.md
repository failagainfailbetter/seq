# Seq Beginner Manual (Windows Guide)

This guide is for **Windows users** who want to **make music with Seq** without reading technical developer docs first.

If you are on GitHub, open this file and click **Download raw file** to save it for offline use.

---

## 1) What Seq is (plain English)

Seq is a desktop app for building songs from reusable musical blocks.

Most sequencers are timeline-first (left to right). Seq is different: you create small pieces, then combine and re-combine them into bigger sections until you have a full song.

In Seq, those building blocks are called **motifs** (explained in the glossary at the end).

---

## 2) System requirements / prerequisites (Windows)

- **OS:** Windows 10 or newer (Windows 11 recommended)
- **Java:** **Java 20 or newer** (required for `.jar` launch and source builds)
- **For making sound:** A MIDI destination (hardware synth, virtual synth, DAW MIDI port, etc.)
- **Optional:** MIDI keyboard/controller for recording notes in real time
- **For Reaper integration:** Cockos Reaper (any recent version)

Notes:
- Seq is written in Java and runs on Windows via the `.jar` file.
- Windows 10/11 include native virtual MIDI support; no third-party software required.

---

## 3) Installation guide (Windows)

### Step 1: Install Java 20 or newer

1. Visit [adoptium.net](https://adoptium.net) or [oracle.com/java](https://www.oracle.com/java/technologies/downloads/).
2. Download the **Windows x64 Installer** (LTS version 20 or newer).
3. Run the installer and follow the prompts. Accept default paths.
4. **Restart your computer** to ensure Java is added to your system PATH.
5. Verify installation:
   - Open **Command Prompt** (Win+R, type `cmd`, press Enter).
   - Type: `java -version`
   - You should see version 20 or higher. Example output:
     ```
     java version "21.0.1" 2023-10-17 LTS
     ```

### Step 2: Obtain and launch Seq

1. Download `seq.jar` from the project repository (link in `README.md`) or build `install/seq.jar` from source.
2. Save it to a folder on your computer (e.g., `C:\Users\[YourName]\Music\Seq\` or Desktop).
3. **Easiest launch:** Double-click `seq.jar`.
   - If Java is properly installed, the app should open immediately.
4. **If double-click doesn't work:**
   - Right-click `seq.jar` → **Open with** → **Java(TM) Platform SE binary**.
   - If Java is not listed, skip to the terminal method below.
5. **Terminal launch (fallback):**
   - Open **Command Prompt** in the folder containing `seq.jar` (Shift+Right-click in the folder → **Open PowerShell window here**).
   - Type: `java -jar seq.jar`
   - Press Enter.

### Step 3: Configure MIDI output

1. After Seq opens, go to **MIDI → Set MIDI Devices**.
2. Under **MIDI Outputs**, select at least one destination:
   - A hardware synth or MIDI device
   - A DAW (e.g., Reaper) listening on a virtual MIDI port
   - A software synth that accepts MIDI input
3. Choose the appropriate channel (or leave as **All Channels** for now).
4. Click **Set**.

### Step 4: Verify sound works

1. Open **File → Load Sequence...** and load `songs/demo1.seq` from the repo.
2. Press **Play** in the Transport (top-left controls).
3. You should hear music from your synth/DAW.
4. If no sound:
   - Re-check MIDI output assignment in **MIDI → Set MIDI Devices**.
   - Confirm the destination (synth, DAW, etc.) is armed and listening for MIDI.
   - Try a different output device.
5. Press **Stop** when done.

---

## 4) Build-from-source guide (Windows)

### Prerequisites
- Java 20+ installed (see Step 1 above).
- GNU Make and a C/C++ compiler installed (e.g., MinGW64 or MSVC).
- The Seq repository cloned locally.

### Build steps

From the repository root (e.g., `C:\path\to\seq`):

1. Open **Command Prompt** or **PowerShell**.
2. Change to the repo folder:
   ```cmd
   cd C:\path\to\seq
   ```
3. Compile all classes and resources:
   ```cmd
   make all
   ```
4. Build a runnable JAR:
   ```cmd
   make jar
   ```
   This produces: `install\seq.jar`

5. Launch the built JAR:
   ```cmd
   java -jar install\seq.jar
   ```

### Classpath launch (advanced)

If you only want to run after compiling (without building a JAR):

```cmd
java -cp ".;libraries\*" seq.gui.SeqUI
```

---

## 5) How to launch Seq

### Standard launch (recommended)

```cmd
java -jar seq.jar
```

Or simply double-click `seq.jar`.

### Low-latency launch (for better performance)

Use the ZGC garbage collector to reduce audio glitches:

```cmd
java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar
```

---

## 6) First run / first song walkthrough

Use the included demo song: `songs/demo1.seq`.

1. Start Seq.
2. Open **MIDI → Set MIDI Devices**.
3. Select an output device and channel, then click **Set**.
4. Open **File → Load Sequence...** and select `songs/demo1.seq`.
5. Press **Play** in the Transport (top-left controls).
6. You should hear the demo song play.
7. If no sound:
   - Verify MIDI output is set to an active device.
   - Check that your synth/DAW is armed and listening.
   - Try **MIDI → Panic** to clear any stuck notes.
8. Press **Stop** when done.

---

## 7) Core concepts in simple terms

- **Motif**: A reusable music block (pattern, MIDI clip, generator, or a block that combines others).
- **Hierarchy**: Blocks can contain other blocks. Small blocks build medium blocks, then whole songs.
- **Modular reuse**: The same motif can be reused in more than one place.
- **Root motif**: The top/start motif that Seq plays when you press Play.
- **Transport**: Play/Stop/Record/Pause/Loop controls.
- **Recording**: Capturing incoming MIDI into recordable motifs (especially **Notes** motifs when armed).
- **MIDI input/output**:
  - Input = where MIDI comes in (keyboard/controller)
  - Output = where MIDI goes out (synth/device/software instrument)

---

## 8) Practical step-by-step: make a simple song

This path keeps things simple and uses common motifs.

### A) Make a drum/groove pattern
1. In the **Motif List**, click **+** and add a **Step Sequence** motif.
2. Click steps to turn them on/off (red intensity = velocity).
3. Set tempo in **Clock Options** (under the Transport).
4. Press **Play** to hear the pattern.

### B) Add a melody clip
1. Add a **Notes** motif.
2. Draw notes in the piano-roll area (or arm and record from MIDI input).
3. Set its output in the Notes inspector if needed.

### C) Arrange sections
1. Add a **Series** motif (plays children one after another).
2. Drag your Step Sequence and Notes motifs into the Series child list.
3. Adjust repeats/probability if desired.
4. Set this Series motif as **Root Motif** (Motif menu option), then press Play.

### D) Layer parts
1. Add a **Parallel** motif (plays children at the same time).
2. Drag motifs into it to layer drums + melody + other parts.
3. Use child delay/gain/probability for variation.

### E) Iterate quickly
1. Rename motifs clearly.
2. Duplicate motifs, change only a few details, and reuse them in Series/Parallel.
3. Save versions often.

---

## 9) Cockos Reaper DAW integration

**Reaper** is a popular, affordable DAW that works perfectly with Seq via Windows MIDI routing.

### Why Seq + Reaper?
- Seq excels at **hierarchical, modular composition** (creating reusable building blocks).
- Reaper excels at **timeline editing, mixing, effects, and exporting**.
- Together: compose with Seq's motif-based workflow, then arrange and mix in Reaper.

### Step 1: Create a virtual MIDI port (Windows 10/11 native)

Windows 10/11 include built-in virtual MIDI support. Create a loopback port using native Windows:

1. Open **Settings** → search for **"MIDI"** → select **MIDI Settings**.
2. Look for built-in virtual MIDI ports under **Inputs** and **Outputs**.
3. If none exist, Windows will automatically create one when Seq and Reaper both request MIDI routing.

### Step 2: Create a virtual MIDI loopback (if needed)

If Windows doesn't expose virtual ports, use **VB-Audio Virtual MIDI Router** (free, lightweight):

1. Download from [vb-audio.com](https://vb-audio.com/Voicemeeter/index.htm) or search "Virtual MIDI Router Windows 11".
2. Install and restart your computer.
3. A virtual MIDI port will be available in Seq and Reaper MIDI device lists.

### Step 3: Configure Seq MIDI output

1. Open Seq.
2. Go **MIDI → Set MIDI Devices**.
3. Under **MIDI Outputs**, enable:
   - The virtual MIDI loopback port (e.g., "Virtual MIDI Router Out" or Windows built-in port)
   - Any hardware synths you're using directly
4. Click **Set**.

### Step 4: Configure Reaper to receive Seq MIDI

1. Open **Reaper**.
2. Go **Options → Preferences → MIDI Devices**.
3. Under **MIDI inputs**, enable the same virtual port that Seq outputs to.
4. Set to **Enabled + Control** if you want Reaper to receive sync/transport from Seq (optional).
5. Click **OK**.

### Step 5: Create a Reaper track for Seq

1. In Reaper, click **Track → Insert new track**.
2. Double-click the new track to open its properties.
3. Under **Input**:
   - Set to: **MIDI → [Your virtual port] → All Channels** (or a specific channel).
4. Under **Input mode**, ensure it's set to monitor incoming MIDI in real time.
5. Add a virtual instrument (VSTi) to the track:
   - Click **FX** button → insert a synth (e.g., ReaSynth, Synth1, etc.).
6. Arm the track for recording (click the red **Rec** button).

### Step 6: Record Seq into Reaper

1. In Reaper, press **Record** (default shortcut: Ctrl+Alt+Space).
2. Start playback in Seq (press **Play**).
3. Seq's MIDI flows into the Reaper track in real time.
4. Perform/compose live in Seq while Reaper captures it.
5. Stop Reaper's recording when done. The MIDI clip is now on the track.

### Step 7: Edit, mix, and export

In Reaper, you can now:
- Edit the MIDI clip with Reaper's piano roll.
- Layer multiple Seq recordings on different tracks.
- Add effects, EQ, compression to each track.
- Arrange clips on the timeline.
- Export as MP3, WAV, FLAC, etc.

### Seq + Reaper workflow tips

- **Live performance:** Keep Reaper recording; perform live variations in Seq by changing root motifs or adjusting probability/delays.
- **Layered arrangements:** Record one Seq session, copy the MIDI clip, and edit for variations.
- **Tempo sync:** Match Seq's **Clock Options** tempo to Reaper's project tempo (shown in Reaper's top toolbar).
- **Channel organization:** Use separate MIDI channels per instrument (e.g., drums ch10, bass ch2, leads ch3) for cleaner routing.
- **Save templates:** Save Reaper templates with track layouts + VSTi chains, and Seq starter projects, so you can load them together.

### Troubleshooting Seq + Reaper

**No MIDI in Reaper:**
- Confirm the virtual MIDI port exists in Windows MIDI Settings.
- In Seq, check **MIDI → Set MIDI Devices** and verify the output port is enabled.
- In Reaper, go **Options → Preferences → MIDI Devices** and enable the same input port.
- Restart Reaper if changes don't take effect.

**Track meters move but no sound:**
- Ensure a VSTi is loaded on the track (click **FX** and insert a synth).
- Enable input monitoring on the track (look for a speaker icon or **Monitor** button).
- Check track volume fader is not at zero.

**High latency / timing issues:**
- In Reaper: **Options → Preferences → Audio Device** → lower buffer size (start at 64–256 samples).
- In Seq: use low-latency launch (`java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar`).
- Reduce background CPU load (close other apps).

**Stuck/repeating notes:**
- In Seq: use **MIDI → Panic** to send all-notes-off.
- In Reaper: stop playback and use **MIDI → Clear all notes**.

---

## 10) Advanced compositional examples

These examples are designed to be built directly with standard Seq motifs.

### Example 1: Generative variation with probability

Goal: make a repeating groove that evolves without manually rewriting patterns.

**Setup:**
1. Create a base **Step Sequence** motif for drums: `Drums_Base`.
2. Duplicate it 2–3 times and alter only a few hits/velocities: `Drums_Var1`, `Drums_Var2`.
3. Create a **Series** motif called `Drums_Evolving`.
4. Add the base and variants as children in the Series.
5. Set child probabilities (e.g., Base 70%, Var1 20%, Var2 10%) so the base dominates but variations emerge.
6. Add slight delay offsets on rare variants for a humanized feel.

**Result:** A drum groove that stays recognizable but never repeats exactly.

---

### Example 2: Polyrhythmic layering (3 against 4)

Goal: create motion by superimposing contrasting cycle lengths.

**Setup:**
1. Create a **Step Sequence** with an accent pattern repeating every 3 steps: `Accent_Triplet`.
2. Create another **Step Sequence** with pulses repeating every 4 steps: `Pulse_Quad`.
3. Place both in a **Parallel** motif called `Polyrhythm`.
4. Route each to different MIDI channels/instruments for timbral contrast.
5. Let playback run long enough (12–16 bars) to hear the full phase relationship and re-alignment.

**Result:** A mesmerizing polyrhythmic texture that resolves periodically.

---

### Example 3: Hierarchical theme and variations

Goal: keep one musical identity while evolving arrangement depth.

**Setup:**
1. Write a short **Notes** motif (4 bars) as the core theme: `Theme_A`.
2. Duplicate and transpose/modify to create variations: `Theme_A_Inv` (inversion), `Theme_A_Retrograde`.
3. Group variations inside a **Series** motif:
   - Child 1: `Theme_A` (statement)
   - Child 2: `Theme_A_Inv` (answer)
   - Child 3: `Theme_A` (reprise)
4. Wrap the Series in a **Parallel** motif with a constant harmonic pad: `Theme_With_Harmony`.
5. Set this top Parallel as the **Root Motif**.

**Result:** A classical-style theme-and-variations form emerges hierarchically.

---

### Example 4: Stochastic composition with generators

Goal: produce controlled randomness for ambient or algorithmic music.

**Setup:**
1. Use a **Generator** motif (if available) or create a Notes motif with random note selections.
2. Set pitch range constraints (e.g., C3–C5).
3. Limit rhythmic density (e.g., 40% rest probability, note lengths between 1/8 and 1 bar).
4. Place the generator output in a **Parallel** motif with a slower harmonic backing.
5. Record several passes into Reaper and comp (copy best sections) to create a finished piece.
6. Freeze best random states by copying generated sequences into fixed Notes motifs.

**Result:** Evolving, never-quite-the-same sections suitable for ambient or algorithmic music.

---

### Example 5: Call-and-response song form

Goal: alternate musical statements between two voices.

**Setup:**
1. Create a **Notes** motif for a melodic "call" (4 bars, ends in mid-phrase): `Call_Lead`.
2. Create a matching "response" **Notes** motif (4 bars, ends with resolution): `Response_Bass`.
3. Create a **Series** motif alternating them:
   - Child 1: `Call_Lead`
   - Child 2: `Response_Bass`
   - Child 3: `Call_Lead` (repeat)
   - Child 4: `Response_Bass_Variation` (different ending)
4. Add occasional probability reduction on later responses (e.g., 80% vs 100%) to create tension.
5. Wrap in a **Parallel** with a background groove motif to tie sections together.

**Result:** A structured dialogue form that feels composed yet modular.

---

### Example 6: Fibonacci rhythmic scaling

Goal: shape phrase lengths using natural growth (1-2-3-5-8 bars).

**Setup:**
1. Create a short **Step Sequence** or **Notes** motif with a 1-bar rhythmic cell: `Cell_1bar`.
2. Duplicate and extend to 2, 3, 5, and 8 bars:
   - `Phrase_2bars` (2× the cell)
   - `Phrase_3bars` (3× the cell)
   - `Phrase_5bars` (5× the cell)
   - `Phrase_8bars` (8× the cell)
3. Create a **Series** motif stacking them in Fibonacci order:
   - Child 1: `Phrase_1bar`
   - Child 2: `Phrase_2bars`
   - Child 3: `Phrase_3bars`
   - Child 4: `Phrase_5bars`
   - Child 5: `Phrase_8bars`
4. Keep harmony constant while rhythmic density increases.
5. Later in the song, reverse the order (8→5→3→2→1) for a natural cooldown.

**Result:** A hypnotic sense of inevitability as patterns unfold in natural mathematical ratios.

---

### Example 7: Instrumental texture mixing

Goal: blend foreground, midground, and background layers to evolve a texture over time.

**Setup:**
1. Create separate **Notes** motifs for each role:
   - `Foreground_Lead` (bright, sparse high notes on channel 3)
   - `Midground_Arp` (quick arpeggios on channel 2)
   - `Background_Pad` (sustained chords on channel 1)
   - `Percussion_Bed` (drums/percussion on channel 10)
2. Create a **Parallel** motif called `Full_Texture`.
3. Add all four as children with staggered timing:
   - `Background_Pad`: delay 0 (starts immediately)
   - `Percussion_Bed`: delay 0 bars
   - `Midground_Arp`: delay 0.5 bars (enters halfway through)
   - `Foreground_Lead`: delay 1 bar (last to enter)
4. Each child routes to a different MIDI channel so they hit different synths in Reaper.
5. Vary probability/gain per child by section for dynamic texture evolution.

**Result:** A richly layered, gradually unfolding texture where each element enters at its own time, then can be swapped for variations.

---

## 11) Saving, loading, exporting

- **Save sequence:** `File → Save Sequence` / `Save Sequence As...`
- **Load sequence:** `File → Load Sequence...`
- **Merge another sequence into current project:** `File → Merge Sequence...`
- **Export rooted sequence:** `File → Export Root As...`  
  (exports from the current root and drops unrelated motifs)
- **Load MIDI file into Seq motifs:** `File → Load MIDI File...`

Seq project files use the `.seq` extension.

---

## 12) Troubleshooting common problems

### Java not found / "java" is not recognized

- Restart your computer after installing Java (you installed Java in Step 1, right?).
- Open **Command Prompt** again and type `java -version`.
- If still missing, Java may not be in your system PATH. Re-run the Java installer and ensure you check "Add to PATH" or reinstall with admin rights.

### Double-clicking `.jar` opens Windows Archive Explorer or does nothing

- Right-click `seq.jar` → **Properties** → check the **Opens with** field.
- If it shows "Archive Explorer" or "Windows Explorer":
  - Click **Change...** → scroll down → find **Java(TM) Platform SE binary** or similar.
  - If Java is not listed, click "More apps" → "Look for another app on this PC" → navigate to Java installation (e.g., `C:\Program Files\Eclipse Adoptium\jdk-21.0.1-hotspot\bin\javaw.exe`).
  - Set as default and try double-clicking again.
- If this doesn't work, fall back to **Terminal launch** (see Step 2 above).

### UI window is tiny / blurry on high-DPI monitors

- Ensure you have Java 20 or newer installed (DPI scaling works better in newer versions).
- Try the command-line launch:
  ```cmd
  java -jar seq.jar
  ```
- If still blurry, try:
  ```cmd
  java -Dsun.java2d.uiScale=2.0 -jar seq.jar
  ```
  (Adjust `2.0` to `1.5` or `3.0` depending on your monitor scaling.)

### No sound / MIDI not working

- Go to **MIDI → Set MIDI Devices** and verify at least one output is enabled.
- Confirm your destination (DAW, synth, etc.) is running and listening for MIDI.
- If using Reaper: verify the Reaper track input is set to the correct virtual MIDI port and monitoring is on.
- Try **MIDI → Panic** to clear stuck notes, then test again.

### Recording does nothing

- Ensure the target motif supports recording (typically **Notes** or generator motifs).
- Arm the motif (look for an **Arm** button in the motif inspector).
- Verify MIDI input is assigned in **MIDI → Set MIDI Devices**.

### Stuck/hanging notes

- Use **MIDI → Panic** immediately.
- If still stuck, stop playback and use Reaper's **MIDI → Clear all notes** (if recording into Reaper).

### Playback has glitches, clicks, or pops

- Try low-latency launch:
  ```cmd
  java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar
  ```
- Lower audio buffer size in Reaper (if using Reaper): **Options → Preferences → Audio Device** (start at 64–256 samples).
- Close other CPU-heavy applications.

### Virtual MIDI port not appearing in Seq/Reaper

- Restart both Seq and Reaper.
- Restart Windows if using built-in Windows MIDI loopback.
- If using VB-Audio Virtual MIDI Router, restart after installation.
- Recheck **MIDI → Set MIDI Devices** in Seq and MIDI Preferences in Reaper.

---

## 13) Short glossary

- **Motif:** A building block in Seq.
- **Root motif:** The motif playback starts from.
- **Child motif:** A motif used inside another motif.
- **Series motif:** Plays child motifs in sequence.
- **Parallel motif:** Plays child motifs together.
- **Notes motif:** MIDI note/event clip editor + recorder.
- **Step Sequence motif:** Grid-based pattern sequencer.
- **Transport:** Play/Record/Pause/Stop/Loop controls.
- **Quantize:** Snap notes/events to a timing grid.
- **MIDI:** A standard language devices use to send musical note/control messages.
- **Probability:** A percentage chance that a motif will play (used in generative composition).
- **Delay (child):** Time offset before a child motif starts.
- **Virtual MIDI port:** A software bridge allowing MIDI data to flow between apps.

---

## 14) Advanced reading

- Technical source manual in this repo: `docs/manual.tex`
- Public PDF manual link listed in `README.md`
