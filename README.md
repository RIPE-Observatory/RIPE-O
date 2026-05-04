# RIPE-O - Research Integrity Provenance and Evidence Ontology

Research integrity assessments bring together evidence, reviewer judgement, and automated outputs. RIPE-O provides a vocabulary for documenting the provenance of those assessments: the work being assessed, the questions asked, the evidence considered, the hypotheses or answers produced, the activities that generated them, and the human or automated agents involved.

The ontology is motivated by research integrity assessment of randomised clinical trial publications, but its core terms are intentionally generic. It can be used to describe provenance traces for research integrity assessments across the literature.

## Getting Started

The ontology namespace is:

[https://w3id.org/ripe/ripe-o#](https://w3id.org/ripe/ripe-o#)

The ontology document is published at:

[https://w3id.org/ripe/ripe-o](https://w3id.org/ripe/ripe-o)

Version 1.0.0 is available at:

[https://w3id.org/ripe/ripe-o/1.0.0](https://w3id.org/ripe/ripe-o/1.0.0)

Opening the ontology URI in a browser displays the ontology documentation. The release also supports RDF serialisations in Turtle, RDF/XML, JSON-LD, and N-Triples.

For example, to retrieve the ontology as Turtle:

```sh
curl -sH "Accept: text/turtle" -L https://w3id.org/ripe/ripe-o
```

To retrieve version 1.0.0 directly:

```sh
curl -sH "Accept: text/turtle" -L https://w3id.org/ripe/ripe-o/1.0.0
```

## Releases

RIPE-O uses semantic versioning. Each ontology release has its own URI and release folder.

The current release is:

[release/1.0.0](release/1.0.0)

Competency questions and example SPARQL queries are available in [cqs/readme.md](cqs/readme.md).

The latest ontology document URI, [https://w3id.org/ripe/ripe-o](https://w3id.org/ripe/ripe-o), redirects to the latest available release.

## Authors

* Milan Markovic, [University of Aberdeen](https://www.abdn.ac.uk)
* Goutham Indukuri, [University of Aberdeen](https://www.abdn.ac.uk)

## Related Resources

* [RIPE Observatory](https://w3id.org/ripe):
  Semantic resources for capturing and publishing provenance traces of research integrity assessments.
* [RIPE-KG](https://w3id.org/ripe/ripe-kg):
  Knowledge graph of research integrity assessment traces described using RIPE-O.
* [INSPECT-AI](https://w3id.org/ripe/inspect-ai):
  Tooling for supporting research integrity assessments and generating provenance traces.

## License

This ontology is licensed under the Creative Commons Attribution 4.0 International License. See [LICENSE.md](LICENSE.md) for details.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before proposing ontology changes.
