# Serial captures

This directory is for **project-generated black-box captures** only.

Do not add vendor binaries, DLLs, decompiled code, extracted assemblies, or proprietary assets.

## File naming

```text
YYYY-MM-DD_<hardware>_<action>_<system-id>_<target>.txt
```

Example:

```text
2026-09-29_td157a_call_sys1_id146.txt
```

## Capture format

```text
# Test
Action: direct call
Date: 2026-09-29
Hardware: Retekess TD157A
System ID: 1
Target ID: 146
Vendor executable SHA-256: <hash>
Serial: 9600 8N1, no flow control, RTS on

TX:
FE 9A 05 00 01 00 92 80 18 69

RX:
<raw bytes or "none observed">

Observed result:
Pager 146 alarmed.

Notes:
...
```

A capture describes what was actually observed. Do not “correct” raw bytes to match the current protocol hypothesis.
