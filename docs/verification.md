# Verification and research status

This document tracks how strongly each protocol finding is supported and defines the black-box verification procedure.

## 1. Evidence labels

| Label | Meaning |
|---|---|
| `MANUAL` | Behavior is documented in a Retekess manual supplied with the device/software. |
| `RE` | Value/frame was reconstructed during interoperability analysis of the vendor PC application. |
| `HW` | Behavior was reproduced on physical TD157A hardware in this project. |
| `CAPTURE` | Exact bytes were independently observed on the serial link while using the vendor software. |
| `INFERRED` | Interpretation is plausible but not independently confirmed. |

Unknown is better than guessed. Findings based only on software analysis remain marked `RE` until independently verified.

## 2. Current confidence matrix

| Item | MANUAL | RE | HW | CAPTURE | Status |
|---|:---:|:---:|:---:|:---:|---|
| PC pager input range 1–997 | ✓ |  |  |  | documented |
| Pairing/programming supported | ✓ | ✓ | ✓ |  | high confidence |
| Pairing range up to 998 | ✓ | ✓ | ✓ |  | high confidence |
| Physical-base `999 + CALL` power off | ✓ |  |  |  | manual behavior only |
| 9600/8N1/no-flow/RTS |  | ✓ | ✓ |  | verified in project setup |
| Frame `FE 9A ... 69` |  | ✓ | project use |  | high confidence |
| Checksum formula |  | ✓ | project use |  | high confidence |
| Direct-call action `80` |  | ✓ | project use |  | capture desirable |
| Pair/program action `20` | ✓ behavior | ✓ bytes | ✓ |  | high confidence |
| Call-all target `3FFF`, action `10` | ✓ UI function | ✓ |  |  | capture required |
| Cancel call-all action `40` | ✓ UI function | ✓ |  |  | capture required |
| Cancel pairing action `48` |  | ✓ |  |  | capture required |
| Settings `LEN=09` mapping | ✓ names | ✓ bytes |  |  | capture required |
| Reconstructed default settings frame |  | ✓ |  |  | capture required |
| `AA AA AA` accepted |  | ✓ | project tooling |  | high confidence |
| `EE EE EE` rejected |  | ✓ |  |  | capture desirable |
| `55` / `F2` meaning |  | ✓ |  |  | meaning unknown |
| USB power-off command | physical command only | not found |  |  | do not claim |
| Countdown feature |  | not found as TD157A setting |  |  | do not claim |
| Battery/slot telemetry |  | not found |  |  | do not claim |

## 3. Research basis

The project used Retekess documentation for the TD157/TD157A family and a vendor TD157A PC application supplied for interoperability testing.

The public repository does not redistribute those proprietary materials.

Vendor PC application fingerprint used during research:

```text
SHA-256
a9ee41b48b9336476483346b5a47000d4a454f48c1fd725eb9388f8ff2e11cd7
```

A project hardware log records successful TD157A serial opening as:

```text
9600 8N1
flow control: none
RTS: on
```

## 4. Capture requirements

Before changing an `RE`-only command to confirmed, record:

```text
Date:
TD157A hardware revision/label:
Pager revision/label:
Vendor application SHA-256:
COM port:
Serial parameters:
System ID:
Pager ID(s):
Vendor UI action:
TX bytes:
RX bytes:
Observed hardware result:
Notes:
```

Project-generated captures belong in [../captures/](../captures/).

## 5. Verification tests

### T01 — Direct call

Use system ID `1`, target ID `146`, and trigger one call in the vendor software.

Candidate:

```text
FE 9A 05 00 01 00 92 80 18 69
```

Target evidence: `CAPTURE` + `HW`.

### T02 — Program pager ID

Program ID `146`, capture TX/RX, then verify by calling the new ID.

Candidate:

```text
FE 9A 05 00 01 00 92 20 B8 69
```

### T03 — Call all

Trigger the vendor software's group-call function.

Candidate:

```text
FE 9A 05 00 01 3F FF 10 54 69
```

### T04 — Cancel call-all

Candidate:

```text
FE 9A 05 00 01 3F FF 40 84 69
```

### T05 — Settings frame

Change one setting at a time in the vendor application and capture the complete transmitted frame.

Suggested sequence:

1. defaults;
2. pager buzzer only;
3. response time only;
4. pager vibration only;
5. light effect only;
6. boundary duration only;
7. host vibration only;
8. host buzzer only.

Compare the changed byte with [protocol.md](protocol.md#4-device-settings).

### T06 — Restore defaults

Use the vendor application's restore-defaults function and capture the transmitted bytes. Verify whether the system ID is preserved.

### T07 — Status tokens

Capture all RX traffic during:

- a successful call;
- idle connection;
- connection/startup;
- any safely reproducible rejected command.

Do not assign semantics to `55` or `F2` until repeatable behavior correlates them with a specific state.

## 6. Testing precautions

Avoid repeatedly changing the system ID unless required. Vendor documentation warns that changing the base/system ID requires re-pairing pagers.

Record known-good settings before tests.
