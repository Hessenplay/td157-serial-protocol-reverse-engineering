# Publication and legal hygiene

This is a technical project policy, not legal advice.

The repository is intended to contain independently written interoperability documentation and original code only.

## Publish

- protocol byte layouts written in our own words;
- checksum algorithms;
- independently generated serial captures;
- original test scripts and integrations;
- factual compatibility notes;
- hashes identifying vendor software versions used for research;
- references to vendor manuals without reproducing them.

## Do not publish

- original vendor executables;
- vendor DLLs or extracted assemblies;
- decompiled/disassembled source listings;
- copied vendor implementation code;
- passwords, keys, certificates, or secrets;
- vendor graphics, icons, or screenshots unless clearly permitted;
- full copies or substantial verbatim portions of manuals.

## Evidence hygiene

For protocol facts discovered through vendor-software analysis, use the `RE` label.

Where practical, re-verify them using black-box serial captures and physical hardware. Once independently observed, add `CAPTURE` and/or `HW`.

## Trademark wording

Recommended notice:

> Retekess and TD157/TD157A are trademarks or product identifiers of their respective owners. They are used here solely to identify the product for interoperability purposes. This project is unofficial and is not affiliated with, endorsed by, sponsored by, or maintained by Retekess.

Avoid repository branding that could imply an official Retekess project.
