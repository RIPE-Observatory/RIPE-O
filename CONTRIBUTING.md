# Contributing

RIPE-O is intended to remain a lean, stable ontology for describing provenance traces of research integrity assessments. Contributions are welcome when they make the ontology clearer, more reusable, or better aligned with real assessment traces.

Before proposing a change, please open an issue describing the use case. For new classes or properties, include at least one example of the assessment trace that cannot be represented well with the current vocabulary.

Ontology changes should preserve the existing namespace:

```text
https://w3id.org/ripe/ripe-o#
```

When proposing a change, please check that:

* new terms have clear `rdfs:label` and `rdfs:comment` annotations;
* reused ontology terms are preferred over new RIPE-O terms where they already fit;
* examples and documentation are updated when the change affects users;
* changes that may break existing data are versioned rather than silently replacing earlier semantics.

For release changes, update the corresponding folder under `release/` and verify that the ontology serialisations parse correctly.
