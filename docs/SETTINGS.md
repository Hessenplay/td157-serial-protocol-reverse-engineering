# TD157A settings frame

## Status

The vendor PC application exposes these setting categories:

- system ID;
- extension/pager buzzer;
- extension response time;
- extension boundary alarm duration;
- extension/pager vibration;
- host/base vibration;
- extension/pager lighting;
- host/base buzzer;
- restore factory settings.

The category names are documented in the vendor PC-software manual. The byte mapping below was reconstructed during interoperability analysis and should be independently confirmed by black-box serial capture.

**Evidence:** setting categories `MANUAL`; byte mapping primarily `RE`.

## Frame layout

All settings are written together using a `LEN = 09` frame:

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

| DATA offset | Name | Description |
|---:|---|---|
| 0 | `SYS_H` | System ID high byte |
| 1 | `SYS_L` | System ID low byte |
| 2 | `BUZZER` | Pager buzzer mode |
| 3 | `RESPONSE` | Pager response/alarm time |
| 4 | `VIB` | Pager vibration |
| 5 | `LIGHT` | Pager light effect |
| 6 | `BOUNDARY` | Boundary/out-of-range alarm duration |
| 7 | `HOST_VIB` | Base/host vibration |
| 8 | `HOST_BUZZ` | Base/host buzzer |

## Values

### System ID

Reconstructed range: `1..999`.

Vendor documentation warns that changing the system/base ID requires re-pairing/re-coding pagers.

### Pager buzzer

| Value | Meaning |
|---:|---|
| `00` | Off |
| `01` | Slow |
| `02` | Medium |
| `03` | Fast |

Reconstructed vendor default: `02`.

### Pager response time

`0..99` seconds. Reconstructed vendor default: `60` seconds (`0x3C`).

The older physical-base manual separately documents a call-duration setting of `001..099` seconds. The exact relationship should be verified on hardware.

### Pager vibration

| Value | Meaning |
|---|---|
| `46` | Off |
| `4E` | On |

Reconstructed vendor default: `4E`.

### Pager lighting

| Value | Meaning |
|---:|---|
| `00` | Off |
| `01` | Slow flash |
| `02` | Medium flash |
| `03` | Fast flash |
| `04` | Breathing light |

Reconstructed vendor default: `02`.

### Boundary alarm duration

Reconstructed range: `0..30` minutes. Reconstructed vendor default: `0`.

No separate enable/disable byte has been identified. The semantics of zero should therefore be hardware-verified.

### Host/base vibration

`46` = off, `4E` = on. Reconstructed vendor default: `4E`.

### Host/base buzzer

`46` = off, `4E` = on. Reconstructed vendor default: `4E`.

## Reconstructed vendor defaults

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

The vendor application's restore-defaults path appears to preserve the current system ID and apply default values through the settings command. This remains `RE` until captured.

## Not established as TD157A USB settings

Do not currently claim:

- a fixed table of “39 reminder modes”;
- a separate countdown/overdue timer;
- battery or charging-slot telemetry;
- software control of the pager's physical mute button;
- advertising-inlay management.
