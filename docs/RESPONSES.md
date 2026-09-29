# TD157A responses and status tokens

## Known tokens

| Token | Working interpretation | Evidence |
|---|---|---|
| `AA AA AA` | Command accepted by base | `RE`, project tooling |
| `EE EE EE` | Command rejected by base | `RE` |
| `55 55 55` | Status/heartbeat-like token | `RE`; exact semantics unknown |
| `F2 F2 F2` | Status/heartbeat-like token | `RE`; exact semantics unknown |

## Possible framed form

Vendor-application analysis indicates a status-response family with `LEN = 05` where the first two DATA bytes are the system ID and the remaining bytes contain a repeated status value:

```text
FE 9A 05 SYS_H SYS_L ST ST ST ... 69
```

This framing should be independently captured before being treated as final for every firmware revision.

Diagnostic tooling may also encounter the naked three-byte repeated tokens.

## Semantics

Do not call `AA AA AA` “pager delivered”.

Recommended application statuses:

- `serial_written`: bytes were written to the serial port;
- `base_accepted`: base/transmitter returned `AA AA AA`;
- `base_rejected`: base/transmitter returned `EE EE EE`;
- `unknown_status`: `55 55 55`, `F2 F2 F2`, or other data with no confirmed meaning.

No per-pager RF delivery acknowledgement is currently documented.

## Reader behavior

Do not close the serial port simply because no response arrives after a command.

The tested setup can remain silent after a valid write. A robust implementation should:

1. keep one long-lived serial reader;
2. write one command at a time;
3. optionally wait briefly for AA/EE;
4. treat no response as “written without confirmation”, not a disconnect;
5. close only on a real serial error or explicit user disconnect.
