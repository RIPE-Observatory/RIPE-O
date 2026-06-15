# How to contribute

Thank you for investing your time in contributing to RIPE-O.

To get an overview of the ontology, please read the [README](README.md).

## Issues

If you spot a problem or want to suggest a new term or use case, please search the [issue tracker](https://github.com/RIPE-Observatory/RIPE-O/issues) first. If a related issue does not exist, open a new issue on GitHub.

For new classes or properties, include at least one example of the assessment trace that cannot be represented well with the current vocabulary.

## Proposing ontology changes

RIPE-O is intended to remain a lean, stable ontology for describing provenance traces of research integrity assessments. Contributions are welcome when they make the ontology clearer, more reusable, or better aligned with real assessment traces.

Ontology changes should preserve the existing namespace:

```text
https://w3id.org/ripe/ripe-o#
```

When proposing a change, please check that:

- new terms have clear `rdfs:label` and `rdfs:comment` annotations;
- reused ontology terms are preferred over new RIPE-O terms where they already fit;
- examples and documentation are updated when the change affects users;
- changes that may break existing data are versioned rather than silently replacing earlier semantics.

For release changes, update the corresponding folder under `release/` and verify that the ontology serialisations parse correctly.

## Make changes locally

1. Fork the repository.
2. Create a working branch.
3. Make the focused change.
4. Check the affected ontology files, documentation, and examples.

## Commit your update

Commit the changes once you are happy with them.

Always write a clear log message for your commits. One-line messages are fine for small changes.

## Pull request

When you are finished, create a pull request.

- Link the pull request to the issue it addresses.
- Describe the motivation for the change.
- Mention any release folders, examples, or documentation files that were updated.
- Target the `main` branch.
