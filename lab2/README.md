# OWL Lab

## Homework on OWL : 

To be done alone or by pair ; To be uploaded: a .zip file containing :

1. a humanontology.ttl file containing the OWL ontology developed
2. a humanschema.ttl file containing the RDFS schema from which the OWL ontology has been created (possibly different from the original one)
3. a human.ttl file containing the RDF data with which the ontology has been tested (possibly different from the original one)
4. a humanqueries.txt file containing the SPARQL queries with which the OWL ontology has been tested
5. a owlsemantics.txt file containing SPARQL CONSTRUCT queries implementing the semantics of OWL
6. a report.pdf file containing a report (in English or in French) on the work done, explaining which SPARQL query in humansqueries.txt enables to test which OWL statement with which RDF statements, with which result; and which SPARQL query in owlsemantics.txt implements which entailment rule for which OWL construct.

***

## TD : 

### Exercice 1
Copy the ontology defined in human.rdfs in a new file humans.owl and complete it :
1. Declare that hasSpouse and hasFriend both are symmetric properties.
2. Declare that hasAncestor is transitive.
3. Declare that hasChild is the inverse property of hasParent.
4. Declare that classes Male and Female are disjoint.
5. Declare that class Professor is the intersection of class Lecturer and class Researcher.
6. Declare that class Academic is the union of class Lecturer and class Researcher.
7. Use two restrictions to declare that any person married with a man is a woman and that any person married with a woman is a man.
8. Use a restriction to declare that any person has a parent who is a woman.
9. For each of the above declarations, write a SPARQL query showing that Corese implements part of the OWL language (you will realize that the answers to the queries are different when you load the ontology in humans.rdfs or the ontology in humans.owl).

### Exercice 2
Load your file humans.owl in the OWL editor Protégé and visualize your ontology.

Discover the functionalities of Protégé to edit an ontology and the integrated reasoner to check consistencies, infer hierarchical relations between classes and types for instances.

### Exercice 3
Just like for RDFS, write a set of SPARQL CONSTRUCT queries that could implement (part of) the semantics of OWL (OWL entailment rules).

The documents in the W3C recommandation are much more complicated, but you may have a look at them:
- https://www.w3.org/TR/owl2-rdf-based-semantics/
- https://www.w3.org/TR/sparql11-entailment/

### Exercice 4
Choose a cultural domain and construct an OWL ontology to represent cultural events.

Create an RDF dataset based on it and test your model by writing SPARQL queries.
