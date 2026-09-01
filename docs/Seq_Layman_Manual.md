# Seq Beginner Manual (Layman-Friendly)

This guide is for people who want to **make music with Seq** without reading technical developer docs first.

If you are on GitHub, open this file and click **Download raw file** to save it for offline use.

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
1. Install Java 20+.
2. Download `seq.jar` (project link in `README.md`).
3. Double-click `seq.jar` (or use command line launch below).

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

### Playback glitches/clicks
- Try command-line launch with:
  `java -XX:+UseZGC -XX:MaxGCPauseMillis=1 -jar seq.jar`

---

## 11) Short glossary

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

## Advanced reading

- Technical source manual in this repo: `docs/manual.tex`
- Public PDF manual link listed in `README.md`
