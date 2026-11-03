# Einstein roadmap — the hybrid dual-band station

Honest separation of **software-now** vs **needs-hardware/desktop-helper**, so the
project is a strong contender without overpromising.

## Phase 1 — cross-platform app (DONE)
- Signal analyzer (spectrum + waterfall + SNR) from audio input.
- Audio recorder.
- Crossband repeater **controller/monitor** (VOX detect + keying decision).
- Propagation probing & logging: grid DX, SST→duct/inversion prediction, sea-Doppler
  vs rig-drift classifier, WSJT-X import, ADIF/CSV, dashboard.
- Tools: MMSI, Selcall tables, international ALE channels.

## Phase 2 — desktop helper (real-time radio I/O; Windows/Linux)
Needs raw sockets/serial, so it ships in the Electron desktop build's Node process:
- **Hamlib `rigctld` CAT** — frequency, mode, **PTT** for IC-9100/FT-991A/FTM-500D/etc.
- **WSJT-X UDP** decode capture → auto-log FT8/FT4/Q65/FST4W/MSK144.
- **PSK Reporter** spotting.
- **Crossband repeater keying** — RX on radio A → key radio B via PTT, with the Phase-1
  controller logic. Two radios or a dual-band crossband rig; authorisation required.
- **Marine-weather auto-fill** for the duct predictor (wind/SST/pressure from an API).

## Phase 3 — SDR dual-band receiver (needs an SDR device)
- IQ front-end from **RTL-SDR / ADALM-Pluto / USRP** (and direct-sampling HF).
- True **VHF + HF dual-band RX** with wide spectrum/waterfall and per-signal decode,
  feeding the analyzer and the log.
- The HOMER/AVIAN and OSM (ocean-scatter) decoders run here as the original modes.

## Phase 4 — transmitter / neural receiver
- **Transmitter**: the Pigeon HOMER/AVIAN beacon + data modes through the rig (soundcard
  or IQ), with ALE/Selcall addressing and scheduled beaconing.
- **Neural receiver** (the genuine AI step): a sequence-model decoder trained in MATLAB
  (see the Pigeon repo `rf/RECEIVER_ARCHITECTURE.md`), exported via ONNX to run here —
  shipped only if it beats the classical soft-Viterbi on the same channel.

## Non-goals (stated honestly)
- Not a re-implementation of WSJT-X's FT8/EME/Q65 decoders — those stay in WSJT-X.
- Not MIL-STD-188-141 2G ALE interoperable yet (needs the standardized 8-FSK ALE
  waveform + Golay FEC) — the current ALE/Selcall is clear-text and Pigeon-native.
