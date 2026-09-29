# Retekess TD157 / TD157A Serial Protocol

Unofficial interoperability documentation for the **Retekess TD157A** pager system and related TD157 behavior.

This repository documents the serial protocol used between a PC and the TD157A transmitter/base. It is intended for interoperability, diagnostics, open-source integrations, and reproducible hardware research.

> **Status:** Work in progress. Core framing, direct calls, broadcast calls, pager programming, and the settings frame are documented. Some fields were reconstructed from the vendor PC application and are intentionally marked as needing independent black-box capture before they are considered fully confirmed.

## What is documented

- serial transport parameters;
- frame structure and checksum;
- direct pager/group calls;
- call-all and cancel-call-all;
- pager ID programming;
- pairing cancellation;
- device settings frame;
- known response/status tokens;
- vendor-default settings reconstructed during interoperability research;
- known unknowns and unverified assumptions.

## Quick start

```text
9600 baud
8 data bits
1 stop bit
no parity
no flow control
RTS asserted
```

Generic frame:

```text
FE 9A | LEN | DATA... | CHECKSUM | 69
```

Checksum:

```text
CHECKSUM = (LEN + sum(DATA bytes)) & 0xFF
```

IDs are unsigned 16-bit **big-endian** values.

Example: direct call to pager/group ID `146` using system ID `1`:

```text
FE 9A 05 00 01 00 92 80 18 69
```

See [docs/PROTOCOL.md](docs/PROTOCOL.md) for the protocol and [docs/STATUS.md](docs/STATUS.md) for the evidence level of each finding.

## Evidence labels

| Label | Meaning |
|---|---|
| `MANUAL` | Behavior is documented in a Retekess manual supplied with the device/software. |
| `RE` | Value/frame was reconstructed during interoperability analysis of the vendor PC application. |
| `HW` | Behavior was reproduced on physical TD157A hardware in this project. |
| `CAPTURE` | Exact bytes were independently observed on the serial link while using the vendor software. |
| `INFERRED` | Interpretation is plausible but not independently confirmed. |

For publication-quality claims, `CAPTURE` and/or `HW` are preferred. Items marked only `RE` remain explicitly identified until black-box verification is complete.

## Repository policy

This repository does **not** redistribute the Retekess PC application, vendor DLLs, extracted assemblies, decompiled source code, proprietary UI assets, or full vendor manuals.

Only independently written protocol documentation, project-generated captures, and original interoperability code should be committed.

## Trademark notice

Retekess and TD157/TD157A are trademarks or product identifiers of their respective owners. They are used solely to identify the product for interoperability purposes.

This project is unofficial and is **not affiliated with, endorsed by, sponsored by, or maintained by Retekess**.

## German documentation

See [README.de.md](README.de.md).

## License

This repository is licensed under the [MIT License](LICENSE).
