# TD157 / TD157A serial protocol

This document is the technical protocol reference for the project.

For the evidence level of individual findings, see [verification.md](verification.md).

## 1. Serial transport

| Parameter | Value | Evidence |
|---|---:|---|
| Baud rate | `9600` | `RE`, `HW` |
| Data bits | `8` | `RE`, `HW` |
| Stop bits | `1` | `RE`, `HW` |
| Parity | none | `RE`, `HW` |
| Flow control | none | `RE`, `HW` |
| RTS | asserted/on | `RE`, `HW` |

A project hardware test confirmed successful operation at `9600 8N1` with RTS enabled.

## 2. Frame format

```text
FE 9A | LEN | DATA... | CHECKSUM | 69
```

Byte layout:

```text
FE 9A LEN DATA[0] ... DATA[LEN-1] CHECKSUM 69
```

### Checksum

```text
CHECKSUM = (LEN + DATA[0] + ... + DATA[LEN-1]) & 0xFF
```

The header bytes `FE 9A` and trailing byte `69` are not part of the checksum.

### ID encoding

System IDs and pager/target IDs are unsigned 16-bit values in big-endian order:

```text
ID_H = (ID >> 8) & 0xFF
ID_L = ID & 0xFF
```

## 3. Call and programming commands

The common command family uses `LEN = 05`:

```text
SYS_H SYS_L TARGET_H TARGET_L ACTION
```

| Function | Action | Target | Evidence |
|---|---:|---|---|
| Direct pager/group call | `80` | pager/group ID | `RE`, project hardware use |
| Program/pair pager ID | `20` | new pager ID | `MANUAL`, `RE`, `HW` |
| Call all pagers | `10` | `3FFF` | vendor manual describes function; bytes `RE` |
| Cancel call-all | `40` | `3FFF` | vendor manual describes function; bytes `RE` |
| Cancel pairing | `48` | `0000` | `RE` |

### 3.1 Direct call

The vendor PC software documents normal pager-number input as `1..997`.

```text
DATA = SYS_H SYS_L TARGET_H TARGET_L 80
```

Example: system ID `1`, pager/group ID `146` (`0x0092`):

```text
FE 9A 05 00 01 00 92 80 18 69
```

Checksum:

```text
05 + 00 + 01 + 00 + 92 + 80 = 0x118
0x118 & 0xFF = 0x18
```

### 3.2 Program/pair pager ID

The physical TD157-family documentation describes pairing IDs `1..998`.

```text
DATA = SYS_H SYS_L TARGET_H TARGET_L 20
```

Example: system ID `1`, new pager ID `146`:

```text
FE 9A 05 00 01 00 92 20 B8 69
```

On the TD157A hardware tested in this project, one programming command was sufficient to program all pagers inserted in the charging/base slots at that time. This should not yet be generalized to every hardware revision without additional captures.

### 3.3 Call all pagers

```text
DATA = SYS_H SYS_L 3F FF 10
```

System ID `1`:

```text
FE 9A 05 00 01 3F FF 10 54 69
```

### 3.4 Cancel call-all

```text
DATA = SYS_H SYS_L 3F FF 40
```

System ID `1`:

```text
FE 9A 05 00 01 3F FF 40 84 69
```

### 3.5 Cancel pairing

```text
DATA = SYS_H SYS_L 00 00 48
```

System ID `1`:

```text
FE 9A 05 00 01 00 00 48 4E 69
```

This command still needs an independent serial capture.

## 4. Device settings

The vendor PC application exposes the following setting categories:

- system ID;
- pager/extension buzzer;
- pager/extension response time;
- boundary/out-of-range alarm duration;
- pager/extension vibration;
- base/host vibration;
- pager/extension lighting;
- base/host buzzer;
- restore factory settings.

The category names are documented by the vendor. The byte mapping below was reconstructed during interoperability analysis and is still awaiting full black-box capture verification.

### 4.1 Settings frame

The settings family uses `LEN = 09`:

```text
FE 9A 09
SYS_H SYS_L
BUZZER
RESPONSE
VIB
LIGHT
BOUNDARY
HOST_VIB
HOST_BUZZ
CHECKSUM
69
```

| DATA offset | Field | Meaning |
|---:|---|---|
| 0 | `SYS_H` | system ID high byte |
| 1 | `SYS_L` | system ID low byte |
| 2 | `BUZZER` | pager buzzer mode |
| 3 | `RESPONSE` | pager response/alarm time |
| 4 | `VIB` | pager vibration |
| 5 | `LIGHT` | pager light effect |
| 6 | `BOUNDARY` | boundary/out-of-range alarm duration |
| 7 | `HOST_VIB` | base/host vibration |
| 8 | `HOST_BUZZ` | base/host buzzer |

### 4.2 System ID

Reconstructed range:

```text
1..999
```

Vendor documentation warns that changing the system/base ID requires re-pairing/re-coding the pagers.

### 4.3 Pager buzzer

| Value | Meaning |
|---:|---|
| `00` | Off |
| `01` | Slow |
| `02` | Medium |
| `03` | Fast |

Reconstructed vendor default: `02`.

### 4.4 Pager response time

Reconstructed range:

```text
0..99 seconds
```

Reconstructed vendor default: `60` seconds (`0x3C`).

The older physical-base TD157 documentation separately describes a call-duration value of `001..099` seconds. The exact relationship between that setting and this TD157A PC field should be verified on hardware.

### 4.5 Pager vibration

| Value | Meaning |
|---|---|
| `46` | Off |
| `4E` | On |

Reconstructed vendor default: `4E`.

### 4.6 Pager lighting

| Value | Meaning |
|---:|---|
| `00` | Off |
| `01` | Slow flash |
| `02` | Medium flash |
| `03` | Fast flash |
| `04` | Breathing light |

Reconstructed vendor default: `02`.

### 4.7 Boundary alarm duration

Reconstructed range:

```text
0..30 minutes
```

Reconstructed vendor default: `0`.

No separate enable/disable byte has been identified. The exact semantics of `0` should therefore be verified on hardware.

### 4.8 Base/host vibration

| Value | Meaning |
|---|---|
| `46` | Off |
| `4E` | On |

Reconstructed vendor default: `4E`.

### 4.9 Base/host buzzer

| Value | Meaning |
|---|---|
| `46` | Off |
| `4E` | On |

Reconstructed vendor default: `4E`.

### 4.10 Reconstructed default settings

```text
Pager buzzer:       medium (02)
Response time:      60 s (3C)
Pager vibration:    on (4E)
Pager light:        medium flash (02)
Boundary duration:  0 min (00)
Host vibration:     on (4E)
Host buzzer:        on (4E)
```

With system ID `1`, the candidate settings frame is:

```text
FE 9A 09 00 01 02 3C 4E 02 00 4E 4E 34 69
```

The vendor application's restore-defaults path appears to preserve the current system ID and apply the defaults through the same settings command. This remains `RE` until independently captured.

## 5. Responses and status tokens

Known/reconstructed tokens:

| Token | Working interpretation | Evidence |
|---|---|---|
| `AA AA AA` | command accepted by base | `RE`, project tooling |
| `EE EE EE` | command rejected by base | `RE` |
| `55 55 55` | status/heartbeat-like | `RE`; exact meaning unknown |
| `F2 F2 F2` | status/heartbeat-like | `RE`; exact meaning unknown |

Vendor-application analysis indicates a possible framed status-response family using `LEN = 05`, with the first two DATA bytes holding the system ID:

```text
FE 9A 05 SYS_H SYS_L ST ST ST ... 69
```

This framing should be independently captured before being considered final for every firmware revision.

### Response semantics

`AA AA AA` must not be interpreted as proof of RF delivery to a pager.

Recommended application states:

- `serial_written`: bytes were written to the serial port;
- `base_accepted`: the base returned `AA AA AA`;
- `base_rejected`: the base returned `EE EE EE`;
- `unknown_status`: `55 55 55`, `F2 F2 F2`, or another unclassified response.

No per-pager end-to-end delivery acknowledgement has been identified.

### Reader behavior

A robust implementation should not close the serial port merely because no response arrives after a command.

The tested setup can remain silent after a valid write. Recommended behavior:

1. keep one long-lived serial reader;
2. write one command at a time;
3. optionally wait briefly for AA/EE;
4. treat no response as “written without confirmation” rather than a disconnect;
5. close only on an actual serial error or explicit user disconnect.

## 6. Power-off behavior

The physical hardware documentation describes `999 + CALL` while paired pagers are in the charging slots.

A dedicated USB/serial power-off command has not been confirmed in the TD157A PC application analysis.

Do not assume that a normal direct-call frame to target `999` is equivalent.

## 7. Not currently confirmed

The project does not currently claim the following as confirmed TD157A USB functions:

- a fixed table of “39 reminder modes”;
- a separate countdown/overdue timer;
- battery or charging-slot telemetry;
- software control of the pager's physical mute button;
- advertising-inlay management.
