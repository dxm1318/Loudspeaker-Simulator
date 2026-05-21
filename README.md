# Loudspeaker Simulator

A MATLAB App Designer application for modeling and predicting loudspeaker performance in sealed (closed-box) enclosures using Thiele-Small parameters. To see the code, download the raw file. 

## Overview

The app takes driver and enclosure specifications as inputs, derives missing parameters automatically, and outputs frequency-domain plots of magnitude response (dB SPL) and impedance, along with key performance metrics.

## Requirements

- MATLAB R2016a or later (App Designer support)
- No additional toolboxes required

## Usage

Open `Transducer_sim.mlapp` in MATLAB and click **Run**, or double-click the file if MATLAB is your default handler.

1. Enter the known driver parameters in the left-hand input fields.
2. Enter box volume (`Vb`) and filling percentage.
3. Click **Calculate** to run the simulation.
4. Use **Clear** to reset all fields to zero.

## Input Parameters

### Driver (Thiele-Small)

| Parameter | Unit | Description |
|-----------|------|-------------|
| `Sd` | cm² | Cone surface area |
| `Dd` | mm | Cone diameter (used to derive `Sd` if not provided) |
| `Mms` | g | Moving mass (diaphragm + air load) |
| `Mmd` | g | Diaphragm mass alone (used to derive `Mms` if not provided) |
| `BL` | T·m | Force factor |
| `Re` | Ω | Voice coil DC resistance |
| `Le` | mH | Voice coil inductance |
| `Vas` | L | Equivalent acoustic compliance volume |
| `Cms` | mm/N | Mechanical compliance (used to derive `Vas` if not provided) |
| `Rms` | N·s/m | Mechanical damping |
| `QMS` | — | Mechanical Q factor (used to derive `Rms` if not provided) |
| `QES` | — | Electrical Q factor (derived) |
| `QTS` | — | Total Q factor (derived) |
| `fs` | Hz | Free-air resonant frequency (derived) |
| `Xmax` | mm | Maximum linear excursion |
| `Vd` | cm³ | Driver displacement volume |

### Enclosure

| Parameter | Unit | Description |
|-----------|------|-------------|
| `Vb` | L | Total box volume |
| `Filling` | % | Acoustic fill material as a fraction of `Vb` |
| Box Width / Height / Length | m | Derived from `Vb` using golden-ratio proportions (1.618 : 1 : 0.618) |

The minimum required inputs are: `Vd`, `Vb`, `BL`, `Re`, `Le`, and at least one value from each of the pairs (`Vas` or `Cms`), (`Mmd` or `Mms`), (`Sd` or `Dd`), (`Rms` or `QMS`).

## Derived / Output Parameters

| Parameter | Description |
|-----------|-------------|
| `Vab` | Effective acoustic box volume (accounts for filling mass loading) |
| `fc` | Closed-box resonant frequency |
| `QTC` / `QEC` / `QMC` | Q factors with box loading |
| `f3` | −3 dB frequency |
| `fu` | Upper frequency limit |
| `f_breakup` | Estimated diaphragm breakup frequency |
| Sensitivity | Acoustic sensitivity (dB SPL @ 1 W, 1 m) |
| Efficiency | Acoustic efficiency (%) |
| Acoustic Power | Reference acoustic power output |

## Plot Output

**Magnitude Response and Impedance** — log-frequency from 20 Hz to 20 kHz:

- **Blue**: SPL magnitude response (dB, left axis)
- **Red**: Impedance curve (Ω, right axis)
- **Black dashed**: Closed-box resonance (`fc`)
- **Green**: Upper frequency limit (`fu`)
- **Magenta**: Diaphragm breakup frequency (`f_breakup`)

## Physical Constants

| Symbol | Value | Description |
|--------|-------|-------------|
| ρ₀ | 1.2 kg/m³ | Air density |
| c | 345 m/s | Speed of sound |
| p_ref | 20 µPa | Reference acoustic pressure |
| B | 0.65 | Fill material mass loading factor |

## Reference

Parameter definitions and modeling approach follow:

> Pirkle, W. — *Transducer Theory*, v2.0 (see `TransducerTheory_v2.0_WillPirkle.pdf`)

Symbol conventions are per `Transducer_Symbols.pdf`, both included in this directory.
