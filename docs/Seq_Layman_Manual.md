# Seq Beginner Manual (Layman-Friendly)

This guide is for people who want to **make music with Seq** without reading technical developer docs first.

If you are on GitHub, open this file and click **Download raw file** to save it for offline use.
You can also download a ready-made PDF from the repository **Releases** page (look for the `Seq_Layman_Manual.pdf` asset).

---

## 1) What Seq is (plain English)

Seq is a desktop app for building songs from reusable musical blocks.

Most sequencers are timeline-first (left to right). Seq is different: you create small pieces, then combine and re-combine them into bigger sections until you have a full song.

In Seq, those building blocks are called **motifs** (explained in the glossary at the end).

---

## 2) System requirements / prerequisites

- **OS:** Windows, macOS, or Linux
- **Java:** **Java 20 or newer** (required for `.jar` launch and source builds)
- **For making sound:** A MIDI destination (hardware synth, virtual synth, DAW MIDI port, etc.)
- **Optional:** MIDI keyboard/controller for recording notes in real time

Notes:
- Seq is written in Java and is cross-platform.
- The repository includes a Mac app package route and cross-platform JAR workflow.

---

## 3) Installation guide (Windows, macOS, Linux)

### macOS (easy path)
1. Download the macOS app package from the project link in `README.md` (`Seq.dmg`).
2. Drag `Seq.app` to `/Applications`.
3. Open Seq.

If macOS blocks launch:
- **Older macOS:** Control-click app icon -> **Open**
- **Sequoia and later:** run in Terminal:

```bash
sudo xattr -cr /Applications/Seq.app
```

Then launch again.

### Windows
1. Install **Java 20 or newer**:
   - Download a JDK from Adoptium, Oracle, or Microsoft OpenJDK.
   - In PowerShell, confirm install:
     ```powershell
     java -version
     ```
   - If Java is not found, reopen terminal and verify Java is on `PATH`.
2. Download `seq.jar` (project link in `README.md`) or build `install/seq.jar` from source.
3. If double-click does not launch, run from Command Prompt:
   ```bat
   java -jar seq.jar
   ```
4. Configure MIDI output in Seq:
   - Open **MIDI -> Set MIDI Devices**
   - Select your output destination (hardware synth, virtual MIDI port, or DAW bridge)
   - Choose channel and click **Set**
5. Verify sound:
   - Load `songs/demo1.seq`
   - Press **Play**
   - Confirm receiving synth/DAW track is armed to monitor MIDI
6. Optional low-latency launch:
   ```bat
   java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar
   ```

### Linux
1. Install Java 20+ (OpenJDK is fine).
2. Download `seq.jar`.
3. If needed, set `.jar` files to open with Java.
4. Double-click `seq.jar` (or use command line launch below).

---

## 4) Build-from-source guide

From the repository root:

```bash
cd /path/to/seq
make all
```

This compiles Seq using the bundled libraries in `libraries/`.

To build a runnable JAR:

```bash
make jar
```

This produces:
- `install/seq.jar`

If you only want to run after compiling classes, launch with:

```bash
java -cp "./libraries/*:." seq.gui.SeqUI
```

Windows classpath variant:

```bat
java -cp ".;libraries/*" seq.gui.SeqUI
```

---

## 5) How to launch Seq

### Double-click launch
- macOS app: open `Seq.app`
- Windows/Linux JAR: double-click `seq.jar` (or `install/seq.jar` if built locally)

### Command line launch
Basic:

```bash
java -jar seq.jar
```

Low-latency garbage collector option (recommended by project docs):

```bash
java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar
```

On newer Java/macOS, you may eventually need:

```bash
java --enable-native-access=ALL-UNNAMED -jar seq.jar
```

---

## 6) First run / first song walkthrough

Use the included example in the repo: `songs/demo1.seq`.

1. Start Seq.
2. Open **MIDI -> Set MIDI Devices**.
3. Choose at least one output (your synth/plugin/device + channel), then click **Set**.
4. Open **File -> Load Sequence...** and load `songs/demo1.seq`.
5. Press **Play** in the Transport (top-left controls).
6. If no sound:
   - verify MIDI output assignment
   - verify the receiving synth is running/listening
   - check channel matches
7. Press **Stop** when done.

Tip: If notes get stuck, use **MIDI -> Panic**.

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

## 9) Saving, loading, exporting

- **Save sequence:** `File -> Save Sequence` / `Save Sequence As...`
- **Load sequence:** `File -> Load Sequence...`
- **Merge another sequence into current project:** `File -> Merge Sequence...`
- **Export rooted sequence:** `File -> Export Root As...`  
  (exports from the current root and drops unrelated motifs)
- **Load MIDI file into Seq motifs:** `File -> Load MIDI File...`

Seq project files use the `.seq` extension.

---

## 10) Troubleshooting common problems

### App will not open on macOS
- Use Control-click -> Open, or run:
  `sudo xattr -cr /Applications/Seq.app`

### No sound
- Set MIDI outputs in **MIDI -> Set MIDI Devices**
- Confirm destination synth/instrument is active
- Check channel routing

### Recording does nothing
- Ensure target motif supports recording (typically **Notes**)
- Arm the motif
- Verify MIDI input assignment

### Stuck/hanging notes
- Use **MIDI -> Panic**

### Tiny UI window on high-resolution screen (Windows)
- Use Java 20+ (older Java versions are known to have UI scaling issues)

### Double-clicking `.jar` opens archive app or does nothing (Windows)
- Right-click `seq.jar` -> **Open with** -> **Java(TM) Platform SE binary**.
- If Java is missing from the app list, reinstall JDK 20+ and ensure `.jar` is associated with `javaw.exe`.
- Fallback launch from terminal:
  `java -jar seq.jar`

### Playback glitches/clicks
- Try command-line launch with:
  `java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar`

---

## 11) Cockos Reaper DAW integration

This setup lets Seq drive Reaper instruments in real time and record the performance.

### A) Create a virtual MIDI port between Seq and Reaper

#### Windows (loopMIDI)
1. Install **loopMIDI**.
2. Open loopMIDI and create a port (example: `SeqToReaper`).
3. Leave loopMIDI running while using Seq/Reaper.

#### macOS (IAC Driver)
1. Open **Audio MIDI Setup** -> **Window -> Show MIDI Studio**.
2. Double-click **IAC Driver** and enable **Device is online**.
3. Add one bus (example: `SeqToReaper`).

### B) Configure Seq as MIDI sender
1. Start Seq.
2. Open **MIDI -> Set MIDI Devices**.
3. Set output device to `SeqToReaper`.
4. Pick the channel(s) you want to route (or use separate channels per motif/part).

### C) Configure Reaper to receive Seq MIDI
1. In Reaper: **Options -> Preferences -> MIDI Devices**.
2. Enable your virtual input (`SeqToReaper`) and set it to **Enabled + Control** (if needed for transport mappings).
3. Create a new track and insert a virtual instrument (VSTi/AUi).
4. Click the track record-arm button.
5. Set input to: **Input: MIDI -> SeqToReaper -> All Channels** (or a specific channel).
6. Turn on input monitoring.

### D) Record Seq output into Reaper
1. In Reaper, set track record mode to MIDI (or Record: output if you want rendered audio from the instrument track).
2. Press **Record** in Reaper.
3. Start playback in Seq.
4. Stop recording in Reaper and save the take.
5. Quantize/edit in Reaper piano roll if desired.

### E) Live performance workflow tips
- Use one virtual port and assign stable channel conventions (for example: drums ch10, bass ch2, leads ch3).
- Build Seq motifs as scenes (intro/verse/chorus/break) and switch roots between song sections.
- In Reaper, keep one armed "capture" track plus separate instrument tracks for fast rerouting.
- Save Reaper templates and Seq starter projects together per set/song.

### F) Reaper/virtual MIDI troubleshooting
- **No MIDI in Reaper:** confirm virtual port exists, is enabled in Reaper Preferences, and selected in Seq output.
- **Track meters move but no sound:** ensure a VSTi is loaded and monitoring is enabled.
- **High latency:** lower audio buffer in Reaper audio device settings (start 64–256 samples), then test CPU load.
- **Timing drift/flams:** avoid routing the same channel to multiple unintended tracks.
- **Stuck notes:** use **MIDI -> Panic** in Seq; in Reaper, stop transport and send all-notes-off.

---

## 12) Advanced compositional examples

These examples are designed to be built directly with standard Seq motifs.

### 1) Generative variation with probability
Goal: make a repeating groove that evolves without rewriting patterns.
1. Create a base **Step Sequence** motif for drums.
2. Duplicate it 2-3 times and alter only a few hits/velocities.
3. Put variants into a **Series** motif.
4. Set child probabilities (for example 70/20/10) so the base groove dominates but variations appear.
5. Add slight delay offsets on rare variants for humanized feel.

### 2) Polyrhythmic layering (3 against 4)
Goal: create motion by superimposing contrasting cycle lengths.
1. Create one motif with a 3-step accent cycle.
2. Create another motif with a 4-step pulse.
3. Place both in a **Parallel** motif.
4. Route each to different instruments/timbres for clarity.
5. Let playback run long enough to hear the full phase relationship reset.

### 3) Hierarchical theme and variations
Goal: keep one musical identity while evolving arrangement depth.
1. Write a short **Notes** motif as the core theme.
2. Duplicate and create subtle pitch/rhythm variants.
3. Group variants inside a **Series** motif (A, A', A'', B-return).
4. Wrap that Series in a **Parallel** motif with a constant harmonic pad.
5. Reuse the same theme motifs in multiple song sections to maintain cohesion.

### 4) Stochastic composition with generators
Goal: produce controlled randomness for ambient/algorithmic writing.
1. Add a generator-style motif and set pitch range constraints.
2. Limit rhythmic density (rest probability, note length bounds).
3. Feed output into a slower harmonic backing motif in **Parallel**.
4. Record several passes into Reaper/DAW and comp favorite sections.
5. Freeze best random states by copying generated ideas into fixed Notes motifs.

### 5) Call-and-response song form
Goal: alternate musical statements between two voices.
1. Create a "call" Notes motif (instrument A).
2. Create a matching "response" Notes motif (instrument B), leaving space after each call.
3. Arrange as alternating children in a **Series** motif.
4. Add occasional response probability reduction to create tension.
5. Use a **Parallel** background groove to tie sections together.

### 6) Fibonacci rhythmic scaling
Goal: shape phrase lengths using 1-2-3-5-8 style growth.
1. Build a motif with a 1-bar cell.
2. Duplicate and expand phrase repeats to 2, 3, 5, then 8 bars.
3. Place these motifs in a **Series** motif in Fibonacci order.
4. Keep harmony constant while rhythmic density increases each stage.
5. Reverse the order later in the song for a natural cooldown arc.

### 7) Instrumental texture mixing
Goal: blend foreground, midground, and background layers.
1. Assign motifs into three roles:
   - foreground (lead hooks)
   - midground (chords/arps)
   - background (pads/noise/percussion bed)
2. Combine roles in a **Parallel** motif.
3. Use per-child gain/probability to open/close texture by section.
4. Route roles to separate MIDI channels for DAW-side processing.
5. Automate transitions by swapping child motifs rather than rewriting entire sections.

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

---

## 14) Advanced reading

- Technical source manual in this repo: `docs/manual.tex`
- Public PDF manual link listed in `README.md`
