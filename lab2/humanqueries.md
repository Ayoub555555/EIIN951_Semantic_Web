# 

a humanqueries.txt file containing the SPARQL queries with which the OWL ontology has been tested

Les binds sont là pour enjoliver les resultats

les prefix sont : 
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> 
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#> 
prefix owl: <http://www.w3.org/2002/07/owl#> 
prefix i: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
##	Query 1 : symmetric properties
### *Description*
Test sur les amis pour voir qui sont mutullements amis.

### *Query*
```{sparql}
select ?x1 ?y1 where {
	?x h:hasFriend ?y
	?y h:hasFriend ?x
	BIND(REPLACE(?x,"http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#",
	"#") AS ?x1)
	BIND(REPLACE(?y,"http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#",
	"#") AS ?y1)
}
```

##	Query 2 : asymetric properties
### *Description*
Test sur hasChild pour voir si l'asymétrie est respéctée

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
hasParent inverse de hasChild

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x h:hasChild ?y
	?y h:hasParent ?x
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```

##	Query 4 : transitive properties
### *Description*
on teste avec toute propriété transitive

### *Query*
```{sparql}
select ?x1 ?y1 ?z1
where {
	?x ?p ?y
	?y ?p ?z
	p a owl:TransitiveProperty
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
	BIND(REPLACE(?z,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?z1)
}
```

### *Query explanation :*
On trouve Gaston est ancestor de Harry en passant par Jack il nous reste plus qu'à tester : 
```{sparql}
select ?x1 ?y1  where {
	?x ?p ?y
	?x h:name "Harry"
	?y h:name "Gaston"
	?p a owl:TransitiveProperty
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```

##	Query 5 : disjoint properties
### *Description*
On teste avec deux propriétés qui sont disjointes et on demande les triplets pour lesquels les propriétés ont le même sujet et le même objet

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x ?v ?y
	?x ?z ?y
	?v owl:propertyDisjointWith ?z
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```

##	Query 6 : reflexive properties
### *Description*
On test les ressources qui ont elle même pour sujet
avec comme prédicat une propriété reflexive

### *Query*
```{sparql}
select ?x1 ?p
where {
	?x ?p ?x
	?p a owl:ReflexiveProperty
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
}
```

##	Query 7 : irrefelxive properties
### *Description*
On teste si hasParent apparait dans les resultats, ce qui n'est pas le cas

### *Query*
```{sparql}
select ?x1
where {
	?x ?p ?x
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
}
```

##	Query 8 : property chain properties
### *Description*
on teste si hasUncle et hasFather/hasBrother sont le meme chemin

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x h:hasUncle ?y
	?x h:hasFather/h:hasBrother ?y
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```

##	Query 9 : functional properties
### *Description*
On compte combien de valeur a John pour la propriété shirtsize

### *Query*
```{sparql}
select ?x1 ?y1
where {
	select ?x1 ?y1 (count(distinct ?y) as ?nb)
where {
	i:John h:hasSpouse ?y
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
}
```

##	Query 10 : inverse functional properties
### *Description*
On teste pour l'exemple que nous avons ajouté dans le fichier avec la ressource Unknow et John et on remarque qu'ils apparaissent
comme étant la meme ressource

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x h:name ?y
	?z h:name ?y
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
} order by ?y1
```

##	Query 11 : enumerated classes
### *Description*
On cree la classe dans notre fichier owl comme dans l'exemple et on la parcoure

### *Query*
```{sparql}
select ?x1 ?y1 ?element
where {
	?x a owl:Class
	?x owl:oneOf ?y
	?y rdf:rest*/rdf:first ?element
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
} 
```

##	Query 12 : classs union
### *Description*
On verifie dans les types d'une personne qui est Researcher et Lecturer s'il y a Academic (marche aussi avec une seule des deux classes)

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x a h:Lecturer
	?x a h:Researcher
	?x rdf:type ?y
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```

##	Query 13 : disjoint between two classes
### *Description*
Nous avons William qui est de type Male et Female on teste s'il ressort avec une query

### *Query*
```{sparql}
select ?x1 
where {
	?x a h:Male
	?x a h:Female
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
}
```

##	Query 14 : class intersection
### *Description*
On prend un sujet qui a le type researcher et lecturer et on verifie qu'il possède le type professor

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x a h:Lecturer
	?x a h:Researcher
	?x rdf:type ?y
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```


##	Query 15 : Restriction all Values from
### *Description*
on teste si une personne marriée à un homme est une femme et vice-versa

### *Query*
```{sparql}
select ?x1 ?y1
where {
	?x a h:Man
	?x h:hasSpouse ?y
	?y a h:Woman
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```

##	Query 16 : Restriction some Values
### *Description*
On regarde pour chaque personne si l'une d'entre elle a pour parent une femme dès qu'on en trouve une la régle est respectée

### *Query*
```{sparql}
select ?x1 ?y1 ?z
where {
	?x a h:Person
	?x h:hasParent+ ?y
	?y a ?z
	BIND(REPLACE(?x,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?x1)
	BIND(REPLACE(?y,
 "http://www\\.inria\\.fr/2007/09/11/humans\\.rdfs-instances#", "#") AS ?y1)
}
```
