

# Constructor

Dans ce fichier on rédige l'ensemble des commandes SPARQL qui permettent (une fois donnée à un RDF engine) de faire toutes les inférences RDFS décrite dans ce <a href="https://www.w3.org/TR/rdf11-mt/#rdfs-entailment">tableau</a>.
La description des 13 règles est déjà précisée dans select.md .

***

## RDFS1
```{rdf}

``` 	
***

## RDFS2
```{rdf}
construct { ?y rdf:type ?x }
where {
	?a rdfs:domain ?x .
	?y ?a ?z
}
``` 
***

## RDFS3
```{rdf}
construct { ?z rdf:type ?x }
where {
	?a rdfs:range ?x .
	?y ?a ?z .
}
``` 	
***

## RDFS4
```{rdf}
construct {
	?x rdf:type rdfs:Resource . 
	?y rdf:type rdfs:Resource .
}
where { ?x ?a ?y }
``` 	
***

## RDFS5
```{rdf}
construct { ?x rdfs:subPropertyOf ?z }
where {
	?x rdfs:subPropertyOf ?y .
	?y rdfs:subPropertyOf ?z .
}
``` 	
***

## RDFS6
```{rdf}
construct { ?x rdfs:subPropertyOf ?x }
where { ?x rdf:type rdf:Property }
``` 	
***

## RDFS7
```{rdf}
construct { ?x ?b ?y }
where {
	?a rdfs:subPropertyOf ?b .
	?x ?a ?y .
}
``` 	
***

## RDFS8
```{rdf}
construct { ?x rdfs:subClassOf rdfs:Resource }
where { ?x rdf:type rdfs:Class }
``` 	
***

## RDFS9
```{rdf}
construct { ?z rdf:type ?y }
where {
	?x rdfs:subClassOf ?y .
	?z rdf:type ?x .
}
``` 	
***

## RDFS10
```{rdf}
construct { ?x rdfs:subClassOf ?x }
where { ?x rdf:type rdfs:Class }
``` 	
***

## RDFS11
```{rdf}
construct { ?x rdfs:subClassOf ?z }
where {
	?x rdfs:subClassOf ?y .
	?y rdfs:subClassOf ?z .
}
```
***

## RDFS12
```{rdf}
construct { ?x rdfs:subPropertyOf rdfs:member }
where { ?x rdf:type rdfs:ContainerMembershipProperty }
``` 	
***

## RDFS13
```{rdf}
construct { ?x rdfs:subClassOf rdfs:Literal }
where { ?x rdf:type rdfs:Datatype }
``` 	
