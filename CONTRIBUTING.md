# Contributing

Contributions that improve reproducibility are welcome.

## Preferred contributions

- black-box serial captures;
- verification on different TD157A hardware revisions;
- corrections to byte-field interpretations;
- original implementations in additional languages;
- documentation clarifications.

## Evidence requirement

When changing a protocol value, include:

1. exact hardware/software context;
2. raw TX/RX bytes;
3. observed hardware behavior;
4. whether the value is `MANUAL`, `RE`, `HW`, `CAPTURE`, or `INFERRED`.

Do not submit vendor binaries, decompiled code, extracted assemblies, or copyrighted vendor assets.

## Unknown values

Unknown is better than guessed.

If a status byte or setting cannot be explained confidently, document the raw observation and mark the interpretation `INFERRED` or `unknown`.

## Licensing of contributions

By contributing material to this repository, you agree that your contribution is made available under the license that applies to that type of material:

- documentation, specifications, protocol data and project-generated captures: **CC BY 4.0**;
- software source code, scripts, examples and tools: **MIT**.

If a contribution intentionally needs a different license, discuss it before submitting it and mark the affected files explicitly.
