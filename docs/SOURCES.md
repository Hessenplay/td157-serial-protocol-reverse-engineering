# Research sources

This repository publishes references and hashes, not proprietary vendor software.

## Vendor documentation used

### TD157 pager system operating instructions

Research copy filename:

```text
TD157-EN-DE-FR-IT-ES-RU.pdf
```

Relevant documented behavior includes system/base ID, pager ID pairing, pager number `999` for physical-base power-off, buzzer configuration, call duration, and re-pairing after certain changes.

### TD157A/P upper computer programme user manual

Research copy filenames:

```text
TD157A&P-EN-DE-IT-FR-ES-RU-PT-JP.pdf
TD157A&P-EN-DE-IT-FR-ES-RU-PT-JP-CN.pdf
```

Relevant documented PC-software functionality includes:

- COM-port connection;
- direct pager call with pager input `1–997`;
- group call and cancel group call;
- extension/pager ID setting;
- call-history search/export;
- system ID;
- extension buzzer;
- extension response time;
- extension boundary alarm duration;
- extension vibration;
- host vibration;
- extension lighting;
- host buzzer;
- restore factory settings.

The manuals themselves are not redistributed here.

## Vendor PC application

A vendor TD157A PC application supplied for interoperability testing was analyzed.

Research fingerprint:

```text
SHA-256:
a9ee41b48b9336476483346b5a47000d4a454f48c1fd725eb9388f8ff2e11cd7
```

The binary is not included in this repository.

Protocol facts obtained only from software analysis remain tagged `RE` until independently confirmed by serial capture and/or hardware testing.

## Project hardware log

A project log records successful TD157A serial opening as:

```text
9600 8N1
flow control: none
RTS: on
```

Future public captures should be project-generated and non-proprietary.
