# Construct OWL rules

Consignes :

This file containins SPARQL CONSTRUCT queries implementing the semantics of OWL.


## symmetric properties

```{sparql}
construct { ?y ?p ?x }
where {
	?p a owl:SymmetricProperty .
	?x ?p ?y .
}
```

## inverse properties
Comme de base la règle inverse n'est pas symétrique, on réalise deux construct.

```{sparql}
construct { ?y ?q ?x }
where {
	?p owl:inverseOf ?q .
	?x ?p ?y .
}
```
```{sparql}
construct { ?y ?q ?x }
where {
	?p owl:inverseOf ?q .
	?x ?q ?y .
}
```

## transitive properties
```{sparql}
construct { ?x ?p ?z }                    	 	
where {
	?p a owl:TransitiveProperty  .
	?x ?p ?y .
	?y ?p ?z .
}
```

## reflexive properties
```{sparql}
construct { ?x ?p ?x }                    	 	
where {
	?p a owl:ReflexiveProperty .
	?x ?p ?y .
}
```

## 
```{sparql}

```

## properties faites :
- symmetric properties
- inverse properties
- transitive properties
- reflexive properties

## properties ignorées :
- asymmetric properties
- disjoint properties
- irreflexive properties
- property chains
- functional properties
- inverse functionalproperties