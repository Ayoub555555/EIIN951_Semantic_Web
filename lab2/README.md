OWL Lab
# Exercice 1
Copy the ontology defined in human.rdfs in a new file humans.owl and complete it :
- Declare that hasSpouse and hasFriend both are symmetric properties.
- Declare that hasAncestor is transitive.
- Declare that hasChild is the inverse property of hasParent.
- Declare that classes Male and Female are disjoint.
- Declare that class Professor is the intersection of class Lecturer and class Researcher.
- Declare that class Academic is the union of class Lecturer and class Researcher.
- Use two restrictions to declare that any person married with a man is a woman and that any person married with a woman is a man.
- Use a restriction to declare that any person has a parent who is a woman.
- For each of the above declarations, write a SPARQL query showing that Corese implements part of the OWL language (you will realize that the answers to the queries are different when you load the ontology in humans.rdfs or the ontology in humans.owl).

# Exercice 2
Load your file humans.owl in the OWL editor Protégé and visualize your ontology.

Discover the functionalities of Protégé to edit an ontology and the integrated reasoner to check consistencies, infer hierarchical relations between classes and types for instances.

# Exercice 3
Just like for RDFS, write a set of SPARQL CONSTRUCT queries that could implement (part of) the semantics of OWL (OWL entailment rules).

The documents in the W3C recommandation are much more complicated, but you may have a look at them:
- https://www.w3.org/TR/owl2-rdf-based-semantics/
- https://www.w3.org/TR/sparql11-entailment/

# Exercice 4
Choose a cultural domain and construct an OWL ontology to represent cultural events.

Create an RDF dataset based on it and test your model by writing SPARQL queries.
