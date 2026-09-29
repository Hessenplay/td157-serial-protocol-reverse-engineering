# Hardware verification procedure

The purpose of verification is to independently reproduce protocol behavior from observable serial traffic without publishing vendor implementation code.

## Recommended setup

Use a serial interception method suitable for the TD157A USB serial interface, for example a virtual COM bridge/monitor or hardware analyzer where appropriate.

Record raw bytes in hexadecimal.

## Capture metadata

For every test record:

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

## T01 – Direct call

Use system ID `1`, target ID `146`, and trigger one call in the vendor software.

Candidate:

```text
FE 9A 05 00 01 00 92 80 18 69
```

Target evidence: `CAPTURE` + `HW`.

## T02 – Program pager ID

Program ID `146`, capture TX/RX, then verify by calling the programmed ID.

Candidate:

```text
FE 9A 05 00 01 00 92 20 B8 69
```

## T03 – Call all

Trigger the vendor software's group-call function.

Candidate:

```text
FE 9A 05 00 01 3F FF 10 54 69
```

## T04 – Cancel call all

Candidate:

```text
FE 9A 05 00 01 3F FF 40 84 69
```

## T05 – Settings frame

Change **one setting at a time** in the vendor application and capture the complete transmitted frame.

Suggested order:

1. defaults;
2. pager buzzer only;
3. response time only;
4. pager vibration only;
5. light effect only;
6. boundary duration only;
7. host vibration only;
8. host buzzer only.

Compare the changed byte with [SETTINGS.md](SETTINGS.md).

## T06 – Restore defaults

Use the vendor application's restore-defaults function and capture the transmitted bytes. Verify whether system ID is preserved.

## T07 – Status tokens

Capture all RX traffic during a successful call, idle connection, startup, and any safely reproducible rejected command.

Do not assign semantics to `55` or `F2` until repeatable behavior correlates them with a specific state.

## Safety

Avoid repeatedly changing system ID unless necessary. Vendor documentation warns that changing the base/system ID requires re-pairing pagers.

Record known-good settings before tests.
