# Retekess TD157 / TD157A – serielle Protokolldokumentation

Inoffizielle Interoperabilitäts-Dokumentation für das **Retekess TD157A** Pager-System und verwandtes TD157-Verhalten.

Das Repository beschreibt die serielle Kommunikation zwischen PC und TD157A-Basis. Ziel sind nachvollziehbare Integrationen, Diagnosewerkzeuge und offene Implementierungen.

> **Stand:** Entwurf. Frame-Aufbau, Ruf, Globalruf, Pager-Programmierung und der Einstellungsframe sind dokumentiert. Einige Werte stammen derzeit noch aus der Analyse der Hersteller-PC-Software und sind deshalb ausdrücklich als noch durch Black-Box-Mitschnitte zu bestätigen gekennzeichnet.

## Kurzüberblick

```text
9600 Baud
8 Datenbits
1 Stopbit
keine Parität
kein Flow-Control
RTS aktiv
```

Frame:

```text
FE 9A | LEN | DATA... | CHECKSUM | 69
```

Prüfsumme:

```text
CHECKSUM = (LEN + Summe(DATA)) & 0xFF
```

IDs werden als unsigned 16-Bit-Wert **Big Endian** übertragen.

Beispiel: Ruf der Pager-/Gruppen-ID `146` bei System-ID `1`:

```text
FE 9A 05 00 01 00 92 80 18 69
```

Die Dokumentation ist unter [docs/README.md](docs/README.md) zusammengefasst. Dort sind die vollständige [Protokollreferenz](docs/protocol.md) und der [Nachweis-/Teststand](docs/verification.md) verlinkt.

## Nachweis-Kategorien

| Kennzeichnung | Bedeutung |
|---|---|
| `MANUAL` | Verhalten ist in einer Retekess-Anleitung beschrieben. |
| `RE` | Wert/Frame wurde bei der Interoperabilitätsanalyse der Hersteller-PC-Software rekonstruiert. |
| `HW` | Verhalten wurde in diesem Projekt an echter TD157A-Hardware reproduziert. |
| `CAPTURE` | Die Bytes wurden unabhängig auf der seriellen Verbindung mitgeschnitten. |
| `INFERRED` | Interpretation ist plausibel, aber noch nicht unabhängig bestätigt. |

## Was nicht veröffentlicht wird

Nicht in dieses Repository gehören:

- Original-EXE des Herstellers;
- Hersteller-DLLs oder extrahierte Assemblies;
- dekompilierter Programmcode;
- Hersteller-Grafiken und proprietäre UI-Ressourcen;
- vollständige Kopien der Herstellerhandbücher.

Veröffentlicht werden selbst erstellte Dokumentation, eigene Testmitschnitte und eigener Integrationscode.

## Markenhinweis

Retekess und TD157/TD157A werden ausschließlich zur Bezeichnung des kompatiblen Produkts verwendet.

Dieses Projekt ist inoffiziell und steht **in keiner Verbindung zu Retekess und wird von Retekess weder unterstützt noch betreut**.

## Lizenz

Dieses Repository verwendet zwei Lizenzen, abhängig vom Inhalt:

- Dokumentation, Protokollspezifikation, Protokolldaten und projekterzeugte Mitschnitte: **CC BY 4.0**;
- Software-Quellcode, Skripte, Beispiele und Tools: **MIT**.

Die genaue Zuordnung steht in [LICENSE](LICENSE), die einzelnen Lizenzhinweise liegen unter [LICENSES/](LICENSES/).
