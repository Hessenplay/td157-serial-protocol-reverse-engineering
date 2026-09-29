# TD157A serial protocol

## 1. Transport

| Parameter | Value | Evidence |
|---|---:|---|
| Baud rate | `9600` | `RE`, `HW` |
| Data bits | `8` | `RE`, `HW` |
| Stop bits | `1` | `RE`, `HW` |
| Parity | none | `RE`, `HW` |
| Flow control | none | `RE`, `HW` |
| RTS | asserted/on | `RE`, `HW` |

A project hardware log confirmed successful opening at `9600 8N1` with RTS enabled.

## 2. Frame structure

```text
+------+-----+----------+----------+------+
| FE9A | LEN | DATA ... | CHECKSUM |  69  |
+------+-----+----------+----------+------+
 2 byte 1 B    LEN bytes    1 B      1 B
```

Byte sequence:

```text
FE 9A LEN DATA[0] ... DATA[LEN-1] CHECKSUM 69
```

### Checksum

```text
CHECKSUM = (LEN + DATA[0] + ... + DATA[LEN-1]) & 0xFF
```

The header bytes `FE 9A` and final byte `69` are not included in the checksum.

### ID encoding

System IDs and pager/target IDs are 16-bit unsigned values, high byte first:

```text
ID_H = (ID >> 8) & 0xFF
ID_L = ID & 0xFF
```

## 3. Command family: LEN = 05

Common call/programming payload:

```text
SYS_H SYS_L TARGET_H TARGET_L ACTION
```

| Action | Byte | Target | Evidence |
|---|---:|---|---|
| Direct pager/group call | `80` | Pager/group ID | `RE`, project hardware use |
| Program/pair pager ID | `20` | New pager ID | `MANUAL`, `RE`, `HW` |
| Call all pagers | `10` | `3FFF` | vendor manual describes group call; `RE` for bytes |
| Cancel call-all | `40` | `3FFF` | vendor manual describes cancellation; `RE` for bytes |
| Cancel pairing | `48` | `0000` | `RE` |

### Direct call

```text
DATA = SYS_H SYS_L TARGET_H TARGET_L 80
```

The vendor PC software documents pager-number input `1..997`.

Example: system ID `1`, pager/group ID `146` (`0x0092`):

```text
FE 9A 05 00 01 00 92 80 18 69
```

Checksum:

```text
05 + 00 + 01 + 00 + 92 + 80 = 0x118
0x118 & 0xFF = 0x18
```

### Program/pair pager ID

```text
DATA = SYS_H SYS_L TARGET_H TARGET_L 20
```

The physical TD157-family manual documents pairing IDs `1..998`.

Example: system ID `1`, new ID `146`:

```text
FE 9A 05 00 01 00 92 20 B8 69
```

On the tested TD157A setup, one programming command was sufficient to program all pagers currently inserted in the charging/base slots. This project observation should be re-captured before being generalized to every hardware revision.

### Call all pagers

```text
DATA = SYS_H SYS_L 3F FF 10
```

System ID `1`:

```text
FE 9A 05 00 01 3F FF 10 54 69
```

### Cancel call-all

```text
DATA = SYS_H SYS_L 3F FF 40
```

System ID `1`:

```text
FE 9A 05 00 01 3F FF 40 84 69
```

### Cancel pairing

```text
DATA = SYS_H SYS_L 00 00 48
```

System ID `1`:

```text
FE 9A 05 00 01 00 00 48 4E 69
```

This command still needs an independent serial capture.

## 4. Settings family: LEN = 09

See [SETTINGS.md](SETTINGS.md).

## 5. Responses/status

See [RESPONSES.md](RESPONSES.md).

## 6. Write vs. delivery

A successful serial write only means that bytes were accepted by the local serial stack.

Even an `AA AA AA` response should be interpreted as a response from the base/transmitter, not as proof that an individual pager received the RF call.

No end-to-end delivery acknowledgement from individual pagers has been identified.

## 7. Power-off command

The physical hardware manual documents `999 + CALL` while paired pagers are in charging slots.

A dedicated USB/serial power-off command has **not** been confirmed in the TD157A vendor PC application analysis. Do not assume that a normal direct-call frame to target `999` is equivalent.
