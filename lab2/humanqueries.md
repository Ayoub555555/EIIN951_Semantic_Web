# 

a humanqueries.txt file containing the SPARQL queries with which the OWL ontology has been tested

Test des amis a sens unique.
Il ne devrait pas y en avoir quand RDFS est activé.

```{sparql}
select *
where {
	?x h:hasFriend ?y .
	filter not exists { ?y h:hasFriend ?x }
}
```

