# Luxsin-X9-API
UNOFFICIAL details of the Luxsin X9 Pre-Amplifier HTTP API for building custom UIs

WARNING -  Be aware:  The documentation below may be wrong. Usage could damage your device incLuding bricking it. I have used it without trouble. USE IT AR YOUR OWN RISK. I would AVOID SENDING OUT-OF-BOUND settings I have no affiliation with the manufacturer etc.

Thanks in advance for for making corrections! Happy building!

# Amp (Non-Proxied) API Documentation

This document describes the API for the Luxsin X9 audio pre-amplifier (Luxsin/Sinilink based). All communication is performed over HTTP directly to the device IP on port 80.

## Overview

- **Protocol**: HTTP
- **Base URL**: `http://<device-ip>/`
- **Authentication**: None
- **Data Encoding**: Most responses and some POST bodies are encoded using a custom Base64 alphabet.

### Custom Base64 Alphabet

To decode the data, translate characters from the **Custom** alphabet to the **Standard** alphabet, then perform a standard Base64 decode.

- **Custom**: `KLMPQRSTUVWXYZABCGHdefIJjkNOlmnopqrstuvwxyzabcghiDEF34501289+67/`
- **Standard**: `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/`

---

## Endpoints

### 1. Get Device Status
**Endpoint**: `GET /dev/info.cgi?action=syncData`

Returns the current state of the device in encoded JSON format.

**JSON Fields (Decoded)**:

| Field | Description |
| :--- | :--- |
| `device` | Device model/type ("Luxsin-X9"). |
| `version` | Firmware version. |
| `mac` | Device MAC address. |
| `language` | Current UI language (0: English, 1: ZH-HK, 2: ZH). |
| `volume` | Current volume (0-200). |
| `soundStep` | Volume increment step. |
| `input` | Current input source index. |
| `output` | Current output destination index (0: XLR, 1: RCA, 2: Headset). |
| `audioFormat` | Description of current audio (e.g., "PCM 44.1 KHz"). |
| `pcm` | PCM mode/status. |
| `vu` | Current VU meter index. |
| `vu_count` | Total number of available VU meter styles. |
| `screenLight` | Screen brightness level (0: Bright, 1: Medium, 2: Dark). |
| `knob_breathlight` | Knob brightness level (0: Off, 1: Bright, 2: Medium, 3: Dark). |
| `buttonLight` | Button light status (0: On, 1: Off). |
| `buttonShort` | Button short press function. |
| `screenOff` | Auto screen off timer (0: Off, 1: 30s, 2: 1m, 3: 3m, 4: 5m). |
| `sleep` | Sleep timer (0: Off, 1: 1m, 2: 5m, 3: 10m). |
| `autoHome` | Automatic return to home screen (0: Off, 1: 20s, 2: 40s, 3: 60s). |
| `bootSound` | Boot sound status (0: Off, 1: On). |
| `dsp_enable` | DSP status (0: Enabled, 1: Disabled). |
| `audio_enable` | Audio output status (1: Enabled, 0: Disabled). |
| `peqEnable` | Parametric EQ status (1: Enabled, 0: Disable). |
| `peqSelect` | Selected PEQ preset index. |
| `balance` | L/R balance (-10 to 10). |
| `xlr` | XLR port polarity (0: Normal, 1: Reverse). |
| `dacGain` | DAC gain setting. |
| `dacArc` | DAC ARC mode (0: ARC, 1: EARC). |
| `dacImpedance` | DAC impedance setting. |
| `dacVolumeDirect` | DAC direct volume control. |
| `analogGain` | Analog gain setting. |
| `effect_enable` | Effect status (1: Enable, 0: Disable). |
| `effect_value` | Effect intensity. |
| `width_enable` | Soundstage width status. |
| `width_value` | Soundstage width value. |
| `scene_enable` | Scene mode status. |
| `scene_value` | Scene mode value. |
| `crossfeed_enable` | Crossfeed status. |
| `crossfeed_value` | Crossfeed value. |
| `subwoofer_enable` | Subwoofer output status. |
| `subwoofer_value` | Subwoofer crossover frequency. |
| `subwoofer_rate` | Subwoofer slope. |
| `subwoofer_gain` | Subwoofer gain. |
| `loudness_enable` | Loudness compensation status. |
| `loudness_bass_gain`| Loudness bass boost level. |
| `loudness_treble_gain`| Loudness treble boost level. |
| `loudness_threshold_gain`| Loudness activation threshold. |
| `bt_status` | Bluetooth status (0: Disconnected, 1: Playing, 2: Paused). |
| `bt_srcname` | Name of the connected Bluetooth device. |
| `bt_title` | Title of the current track (BT source). |
| `bt_artist` | Artist of the current track (BT source). |
| `msgCount` | Current state change counter. |

### 2. Update Setting
**Endpoint**: `GET /dev/info.cgi?action=setting&<PARAM>=<VALUE>`

Changes a specific device setting. Multiple parameters can be combined in one request (e.g., `&volume=100&input=4`).

**Common Parameters**:

| Parameter | Value Range | Description |
| :--- | :--- | :--- |
| `volume` | 0 - 200 | Set system volume. |
| `input` | 0 - 6 | Change input source (0: USB, 1: USB-C, 2: Coaxial, 3: Optical, 4: Bluetooth, 5: HDMI-ARC, 6: RCA). |
| `output` | 0 - 2 | Change output destination (0: XLR, 1: RCA, 2: Headset). |
| `vu` | 0 - (vu_count-1) | Change VU meter display style. |
| `screenLight` | 0, 1, 2 | Set screen brightness. |
| `knob_breathlight`| 0, 1, 2, 3 | Set knob brightness. |
| `buttonLight` | 0, 1 | Set button light (0: On, 1: Off). |
| `screenOff` | 0 - 4 | Set auto screen off time. |
| `sleep` | 0 - 3 | Set sleep timer. |
| `autoHome` | 0 - 3 | Set auto-home timer. |
| `dsp_enable` | 0, 1 | Set DSP status (0: Enable, 1: Disable). |
| `audio_enable`| 0, 1 | Set audio output status (1: Enable, 0: Disable). |
| `peqEnable` | 0, 1 | Set Parametric EQ status (1: Enable, 0: Disable). |
| `peqSelect` | Index | Select a PEQ preset. |
| `bt_play` | 1 | Toggle Play/Pause (for Bluetooth sources). |
| `bt_next` | 1, 0 | Transport control (1: Next, 0: Prev). |
| `language` | 0, 1, 2 | Set device language (0: English, 1: ZH-HK, 2: ZH). |
| `balance` | -10 to 10 | Set L/R balance. |
| `loudness_enable` | 0, 1 | Enable (1) or disable (0) loudness. |
| `subwoofer_enable`| 0, 1 | Enable (1) or disable (0) subwoofer output. |

### 3. Get PEQ Status
**Endpoint**: `GET /dev/info.cgi?action=syncPeq`

Returns detailed Parametric EQ configuration.

### 4. Change Detection (Message Counter)
**Endpoint**: `GET /msgCount`

Returns a simple integer (e.g., `42`).

**Purpose**: Lightweight polling mechanism.
- The device increments this counter whenever a state change occurs (volume change, input switch, track change, etc.).
- The client should poll this endpoint frequently (e.g., every 3 seconds).
- If the returned value differs from the last known value, the client should then call `/dev/info.cgi?action=syncData` to fetch the full updated state.
- This avoids the overhead of fetching and parsing the full JSON state when nothing has changed.

### 5. Update PEQ Configuration (Advanced)
**Endpoint**: `POST /dev/info.cgi`
**Body**: `json=<ENCODED_JSON>`

Updates multiple PEQ filters at once. The JSON contains a `peqChange` object with an array of `filters`.

**Filter Properties**:
- `fc`: Frequency (Hz).
- `gain`: Gain (dB).
- `q`: Q-factor.
- `type`: Filter type index:
    - `0`: Low Pass
    - `1`: High Pass
    - `2`: Band Pass
    - `3`: Notch
    - `4`: Peak
    - `5`: Low Shelf
    - `6`: High Shelf
    - `7`: All Pass

---
