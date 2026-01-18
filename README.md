# AViTA Bridge: Sound ↔ Meaning

This app creates the critical connection between acoustic patterns and semantic meaning in the Tripod Oral Pipeline.

## The Problem It Solves

In the Tripod Oral Pipeline, we have:

1. **Meaning Maps** — structured semantic representations (agent, event, evidentiality, etc.)
2. **Acoustic Units** — discrete sound patterns discovered by HuBERT/BPE (U35, U97, etc.)

But these two worlds are disconnected. The model doesn't know that the sound pattern "U35_U97" means "evidential marker" in Sateré-Mawé.

**AViTA Bridge creates that connection.**

## How It Works

### Step 1: Load Your Data

- **Acoustic Units JSON** — from Colab (the output of Phase 2.1-2.2)
- **Audio Files** — the segmented recordings

### Step 2: Tag Regions

When you select a region on the waveform, the app shows you:

- **Time range**: 1.20s - 1.45s
- **Acoustic units in that range**: U35_U97_U36

Then you label what that sound MEANS:
- Type: "morpheme"
- Label: "evidential_witnessed"

### Step 3: Export the Bridge

The export contains **sound-meaning pairs**:

```json
{
  "meaning": "evidential_witnessed",
  "type": "morpheme",
  "acoustic_units": ["U35", "U97"],
  "unit_pattern": "U35_U97",
  "time_range": { "start_ms": 1200, "end_ms": 1450 }
}
```

## What This Proves

When you tag multiple instances of the same meaning, you can verify:

1. **Consistency**: Does "evidential_witnessed" always produce similar unit patterns?
2. **Distinctiveness**: Do different meanings produce different unit patterns?
3. **Compositionality**: Can morphemes be identified as consistent sub-patterns?

If yes to all three, the acoustic model has learned the language's sound structure.

## The Full Pipeline

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Meaning Map   │     │  AViTA Bridge   │     │ Acoustic Units  │
│                 │     │                 │     │                 │
│ agent: Peter    │────▶│  Sound-Meaning  │◀────│ U62_U33_U48    │
│ event: pray     │     │     Pairs       │     │ U35_U97        │
│ evidential: vis │     │                 │     │ U81_U82        │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Training Data for  │
                    │    Transformer      │
                    │                     │
                    │ "evidential=visual" │
                    │    ↓                │
                    │ U35_U97             │
                    └─────────────────────┘
```

## File Structure

```
avita-bridge/
├── index.html          # The app
├── README.md           # This file
└── data/
    └── acoustic_units.json  # From Colab
```

## Using with Colab Output

After running Phase 2.1-2.2 in Colab:

1. Download `acoustic_units.json` from Google Drive
2. Download the segmented audio files
3. Load both into AViTA Bridge
4. Start tagging!

## Export Format

The JSON export includes:

- **pattern_dictionary**: Maps unit patterns to their meanings
- **sound_meaning_pairs**: Individual tagged instances
- **segments**: Full segment data with all tags

This format is ready for training a Meaning Map → Audio transformer.

---

*Ready Vessels Project — University of the Nations / YWAM Kansas City*
