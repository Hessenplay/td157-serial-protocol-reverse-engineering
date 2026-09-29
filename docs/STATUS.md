# Evidence and confidence matrix

This file separates documented, reconstructed, hardware-tested, and independently captured behavior.

| Item | MANUAL | RE | HW | CAPTURE | Publication status |
|---|:---:|:---:|:---:|:---:|---|
| PC pager input range 1–997 | ✓ |  |  |  | Confirmed behavior |
| Pairing/programming supported | ✓ | ✓ | ✓ |  | Confirmed behavior |
| Pairing range up to 998 | ✓ | ✓ | ✓ |  | Confirmed behavior |
| Physical-base `999 + CALL` power off | ✓ |  |  |  | Manual behavior only |
| 9600/8N1/no-flow/RTS |  | ✓ | ✓ |  | Confirmed in project setup |
| Frame `FE 9A ... 69` |  | ✓ | project use |  | High confidence |
| Checksum formula |  | ✓ | project use |  | High confidence |
| Direct-call action `80` |  | ✓ | project use |  | High confidence; capture desirable |
| Pair/program action `20` | ✓ behavior | ✓ bytes | ✓ |  | High confidence |
| Call-all target `3FFF`, action `10` | ✓ UI function | ✓ |  |  | Black-box capture required |
| Cancel call-all action `40` | ✓ UI function | ✓ |  |  | Black-box capture required |
| Cancel pairing action `48` |  | ✓ |  |  | Black-box capture required |
| Settings `LEN=09` mapping | ✓ names | ✓ bytes |  |  | Black-box capture required |
| Reconstructed default settings frame |  | ✓ |  |  | Black-box capture required |
| `AA AA AA` accepted |  | ✓ | project tooling |  | High confidence |
| `EE EE EE` rejected |  | ✓ |  |  | Capture desirable |
| `55` / `F2` meaning |  | ✓ observed in analysis |  |  | Meaning unknown |
| USB power-off command | physical command only | not found |  |  | Do not claim |
| Countdown feature |  | not found as TD157A setting |  |  | Do not claim |
| Battery/slot telemetry |  | not found |  |  | Do not claim |

## Publication gate

Before changing an `RE`-only command to confirmed, add a black-box capture containing:

- date;
- hardware model/revision if known;
- vendor software SHA-256;
- exact UI action;
- serial settings;
- TX bytes;
- RX bytes;
- observed hardware effect.

Capture format: [../captures/README.md](../captures/README.md).
