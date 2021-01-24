# 

a humanqueries.txt file containing the SPARQL queries with which the OWL ontology has been tested

Les binds sont là pour enjoliver les resultats
##	Query 1 : symmetric properties
### *Description*
Test sur les amis pour voir qui sont mutullements amis.

### *Query*
```{sparql}
select *
where {
	select ?x1 ?y1 where {
	?x h:hasFriend ?y
	?y h:hasFriend ?x
	BIND(REPLACE(?x,"http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#",
	"#") AS ?x1)
	BIND(REPLACE(?y,"http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#",
	"#") AS ?y1)
}
}
```

##	Query 2 : asymetric properties
### *Description*


### *Query*
```{sparql}
select *
where {
	?x h:hasChild ?y
	?y h:hasChild ?x
}
```

##	Query 3 : inverse properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 4 : transitive properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 5 : disjoint properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 6 : reflexive properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 7 : irrefelxive properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 8 : property chain properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 9 : functional properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 10 : inverse functional properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 11 : enumerated classes
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 12 : classs union
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 13 : class intersection
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 14 : class negation
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 15 : disjoint between two classes
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 16 : disjoint between several classes
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 17 : disjoint union
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 18 : equivalent classes
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 19 : equivalent properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 20 : same resources
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 21 : different resources
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 22 : identification by Keys
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 23 : restriction of property values
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 24 : restriction of some property values
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 25 : restriction to a single property value
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 26 : restriction of a property value to its subject
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 27 : cardinalityrestriction
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 28 : qualified cardinality restriction
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 29 : restriction to a single property value
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 30 : description of an ontology
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```

##	Query 31 : changes in classes or properties
### *Description*


### *Query*
```{sparql}
select *
where {
	
}
```