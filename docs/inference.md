# Inference and Expansion of Graphs

## Introduction

"Inference" is the logical process of drawing a conclusion based on available evidence and prior knowledge. In the world
of RDF, inference is used to generate new triples from existing triples and either reasoning axioms or rules.

In RDFLib, the [`Graph`][rdflib.graph.Graph] object has a function [`expand()`][rdflib.graph.Graph.expand] which will 
add triples to the graph instance it is called on, or create a new graph (see below), using a given reasoning regime or
rules.

The function requires a parameter of `expansion_logic`, default of "RDFS", which specifies which reasoning regime or rule
system is to be used in expansion.

Currently only two reasoning regimes are implements - "RDFS" & "OWLRL" - and one rule system is indicated - "SPARQLRULES" 
- but not yet implemented (expected in later 2026!).

## Reasoning

Two reasoning regimes are supplied with RDFLib at the moment:

* [RDFS Entailment](https://www.w3.org/TR/rdf11-mt/#rdfs-entailment)
    * simple reasoning about subclasses and subproperties, domains and ranges
* [OWL-RL](https://www.w3.org/TR/owl2-profiles/#Reasoning_in_OWL_2_RL_and_RDF_Graphs_using_Rules)
    * a "partial axiomatization of the OWL 2 RDF-Based Semantics", basically some of the OWL rules in the form of first-order logic
    * OWL-RL does all the expansion that RDFS does, and a few more things too

## Rules

There are many rule systems that can be used to express specialised reasoning, for example, [SPARQL Update](http://www.w3.org/TR/sparql11-update/)'s
`INSERT` statement adds new data into a graph like this:

```sparql
# A special SPARQL Update to add an additional name of "Bob" to every person called "Robert"
PREFIX schema: <https://schema.org/>

INSERT {
    ?p schema:additionalName "Bob" .
}
WHERE {
    ?p 
        a schema:Person ;
        schema:givenName "Robert" ;
    .
}
```

When implemented, the "SPARQLRULES" rule set will allow rules written in SPARQL Rules / SRL Rule form to be used, as
per the experimental [SRL Engine](https://github.com/simonstey/py-srl).

## Future Development

The general steps to allow [`expand()`][rdflib.graph.Graph.expand] to work with other reasoning regimes or rule systems
are:

* implement the engine within RDFLib
* create a token for the regime/system and add it to the allowed `expansion_logic` values
    * ensure that when calling [`expand()`][rdflib.graph.Graph.expand] with that new token, any other parameters that
      regime / system needs are specified
