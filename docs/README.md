# Documentation

The public documentation is intentionally kept compact.

## Contents

### [Protocol](protocol.md)

Technical reference for:

- serial connection parameters;
- frame structure and checksum;
- direct calls;
- pager programming;
- call-all and cancel-call-all;
- pairing cancellation;
- TD157A device settings;
- known response/status tokens;
- known limitations and still-unverified behavior.

### [Verification](verification.md)

Research and verification reference for:

- evidence labels (`MANUAL`, `RE`, `HW`, `CAPTURE`, `INFERRED`);
- confidence/status matrix;
- black-box capture procedure;
- hardware test cases;
- research basis and vendor-software fingerprint.

## Machine-readable protocol

A machine-readable representation is available at:

[../protocol/td157a-protocol.json](../protocol/td157a-protocol.json)

## Serial captures

Project-generated black-box captures belong in:

[../captures/](../captures/)

Vendor binaries, decompiled code, extracted assemblies, full manuals, and proprietary vendor assets are intentionally not part of this repository.
