# SELECT

Dans ce document, on teste et compare les différences qu'on peut observer dans le résultat de requêtes SPARQL si les règle d'inférences RDFS (RDFS entailment rules) sont actives ou non.
Pour chaque règle, on décrit sa fonction et on donne un exemple. La requête de l'exemple et en SPARQL et le résultat et le copier coller de la Table résultante dans corese.

Dans l'explication des exemples, on utilise les préfixes suivant :
- prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
- prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
- prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#> .
- prefix i: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#> .

***

## RDFS1
### *Description*
Cette règle stipule que tout Iri(c'est à dire valeurs literrales/Literal <br>
Values definition stipulée dans la recommandation) <br>
dans nos données doit avoir le type rdfs:Datatype. <br>
Les Iri peuvent seulement être des objets.

### *Exemple*

SPARCL test query :
```{SPARCL}
select * where {
	?x ?p ?z
	?z a rdfs:Datatype
}
```
Résultat avec inférence RDFS :
```{csv}
Rien car les données ne contiennent pas de litteral values
```
Résultat sans inférence RDFS :
```{csv}
Rien car les données ne contiennent pas de litteral values
```

***

## RDFS2
### *Description*
Cette règle stipule que si une propriété aaa a un domaine xxx défini (aaa rdfs:domain xxx), alors tous les sujets yyy de triplets de la forme (yyy aaa zzz) doivent posséder le rdf:type du domaine (yyy rdf:type xxx)

### *Exemple*
i:Lucas possède la propriété h:shirtsize dont le rdfs:domain est h:Person. <br>
On devrait donc avoir (i:Lucas rdf:type h:Person) <br>
Ici on demande la liste des h:Person avec une h:shirtsize inférieur à 9.

SPARCL test query :
```{SPARCL}
prefix h:<http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
  ?x a h:Person .
  ?x h:shirtsize ?s .
  filter(?s < 9)
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Mark>	9
2	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Pierre>	9
3	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Lucas>	8
4	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Karl>	9
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Mark>	9
2	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Karl>	9
```
***

## RDFS3
### *Description*
Cette règle stipule que si une propriété aaa a un range xxx défini (aaa rdfs:range xxx), alors tous les objets zzz de triplets de la forme (yyy aaa zzz) doivent posséder le rdf:type du range (zzz rdf:type xxx)

### *Exemple*
On souhaite avoir la liste de toutes les femmes qui sont des mères. <br>
La propriété h:hasMother a pour range h:Woman mais et i:Laura mère de i:Catherine est définie comme une h:Person mais pas une h:Woman. 

SPARCL test query :
```{SPARCL}
prefix h:<http://www.inria.fr/2007/09/11/humans.rdfs#>
select ?w  where {
  ?w a h:Woman .
  ?c h:hasMother ?w .
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Catherine>
2	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Laura>
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Catherine>
```

***

## RDFS4
### *Description*
Cette règle stipule que tous les sujets xxx et les objet yyy des triplets (xxx aaa yyy) doivent avoir le rdf:type rdfs:Ressource.

### *Exemple*
Dans notre exemple on liste tous les rdf:type d'une personne, i:John,  pour voir si rdfs:Ressource apparait dedans.

SPARCL test query :
```{SPARCL} 
prefix i: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
select ?t {
 i:John rdf:type ?t
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Animal>
3	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
4	rdfs:Ressource
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Animal>
3	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
```

***
## RDFS5
### *Description*
Cette règle stipule la transitivité de la propriété rdfs:SubPropertyOf : si on a 3 propriétés xxx, yyy et zzz en relations suivantes : (xxx rdfs:SubPropertyOf yyy) et (yyy rdfs:SuPropertyOf zzz) alors xxx en tant que sous-propriété de la propriété yyy elle même sous propriété de zzz, est une sous propriété de zzz : (xxx rdfs:SubPropertyOf zzz) .

### *Exemple*
Pour notre exemple on regarde la liste des sous propriétés de h:hasAncestor dans laquelle on devrait trouver h:hasParent, h:hasFather et h:hasMother puisque h:hasFather et h:Mother sont sous-propriétés de h:hasParent et h:hasParent est une sous propriété de h:hasAncestor.

SPARCL test query :
```{SPARCL}
prefix h:<http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
  ?x rdfs:subPropertyOf h:hasAncestor
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#hasParent>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#hasFather>
3	<http://www.inria.fr/2007/09/11/humans.rdfs#hasMother>
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#hasParent>
```

***

## RDFS6
### *Description*
Cette règle stipule que toute rdf:Property doit être sous propriété d'elle même.

### *Exemple*
On prend la liste des sous propriété de h:hasParent.

SPARCL test query :
```{SPARCL}
prefix h:<http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
  ?x rdfs:subPropertyOf h:hasFather
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#hasFather>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#hasMother>
3	<http://www.inria.fr/2007/09/11/humans.rdfs#hasParent>
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#hasFather>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#hasMother>
```

***

## RDFS7
### *Description*
Cette règle stipule que pour toute propriété qui est sous propriété d’une autre ces deux propriétés relient les même couples sujets objets. 

### *Exemple*
Pour l'exemple, on regarde les relations qui existent entre i:Mark et i:John. i:John est le père (h:hasFather) de i:Mark et par inférence, c'est donc aussi un de ses parents (h:hasParent) et un de ses ancêtres (h:hasAncestor).

SPARCL test query :
```{SPARCL}
prefix i: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
select * where {
  i:Mark ?p i:John .
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#hasAncestor>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#hasFather>
3	<http://www.inria.fr/2007/09/11/humans.rdfs#hasParent>
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#hasFather>
```

***

## RDFS8
### *Description*
Cette règle stipule que toutes les classes doivent être une sous classe de rdfs:Ressource.

### *Exemple*
Pour vérifier, on regarde la liste des sous-classes de la classe h:Man.

SPARCL test query :
```{SPARCL}
prefix h:<http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
  h:Man rdfs:subClassOf ?o .
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
3   rdfs:Ressource
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
```

***

## RDFS9
### *Description*
Cette règle stipule que si une ressource est de type une certaine classe qui elle même est une sous classe alors lui attribuer la sous classe et la classe
c'est une transitivité mais seulement à deux "étages".
### *Exemple*
On regarde les rdf:type de i:Mark. i:Mark est définie comme une h:Person or h:Person hérite de h:Animal donc i:Mark doit aussi être un animal.

SPARCL test query :
```{SPARCL}
prefix i: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
select * where {
  i:Mark rdf:type ?t  .
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Animal>
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
```

***

## RDFS10
### *Description*
Cette règle stipule que toute classe est sous classe d'elle même. C'est la même règle que RDFS6 mais pour les rdfs:Class au lieu des rdfs:Property.

### *Exemple*
Pour vérifier, on regarde la liste des sous-classes de la classe h:Man.

SPARCL test query :
```{SPARCL}
prefix h:<http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
  h:Man rdfs:subClassOf ?o .
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Man>
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
```

***

## RDFS11
### *Description*
Cette règle stipule la transitivité de la propriété rdfs:subClassOf : si on a 3 classes xxx, yyy et zzz en relations suivantes : yyy est une sous classe de xxx et zzz une sous classe d'y alors zzz est une sous classe de x
C'est la même règle que RDFS5 mais pour les rdfs:Class au lieu des rdfs:Property.

### *Exemple*
Pour notre exemple on vérifie les "sur"-classes (contraire de sous classes) de h:Man dans lesquels on doit retrouver h:Male, h:Person et h:Animal.

SPARCL test query :
```{SPARCL}
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
  h:Man rdfs:subClassOf ?s  .
}
```
Résultat avec inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Animal
```
Résultat sans inférence RDFS :
```{csv}
1	<http://www.inria.fr/2007/09/11/humans.rdfs#Person>
2	<http://www.inria.fr/2007/09/11/humans.rdfs#Male>
```

***

## RDFS12
### *Description*
Toute propriété xxx possédant le rdf:type rdfs:ContainerMembershipProperty doit aussi être une sous propriété de rdfs:subPropertyOf rdfs:member et donc le triplet suivant doit exister : (xxx rdfs:subPropertyOf rdfs:member) .

### *Exemple*
On prend la liste des amants de i:Catherine et on regarde les triplets vérifiés par cette liste (c'est un objet anonyme). On y retrouve le rdf:type rdf:List ainsi que la liste de tous les amants avec comme propriété rdf:_1 rdf:_2 et rdf:_3. Si la règle est respectée pour chaque rdf:_n (n un nombre ici entre 1 et 3) on a une propriété rdf:member.

SPARCL test query :
```{SPARCL}
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
prefix i: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
select ?property ?object where {
  i:Catherine h:bestLovers ?list
  ?list ?property ?object
}
```
Résultat avec inférence RDFS :
```{csv}
1	rdf:_1	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Harry>
2	rdf:_2	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#John>
3	rdf:_3	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Mark>
4	rdf:type	rdf:List
5	rdfs:member	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Harry>
6	rdfs:member	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#John>
7	rdfs:member	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Mark>
```
Résultat sans inférence RDFS :
```{csv}
1	rdf:_1	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Harry>
2	rdf:_2	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#John>
3	rdf:_3	<http://www.inria.fr/2007/09/11/humans.rdfs-instances#Mark>
4	rdf:type	rdf:List
```

***

## RDFS13
### *Description*
La treizième et dernière loi stipule que tout ce qui a comme classe rdfs:Datatype sont des litéraux.

### *Exemple*
On test alors pour tout les triplets xxx <br>
qui ont comme classe rdfs:Datatype <br>
s'il y a des triplets qui sont de type rdfs:Literal<br>
 mais qui sont aussi des sous classes de xxx 
SPARCL test query :
```{SPARCL}
select * where{
?x a rdfs:Datatype
?y rdfs:subClassOf ?x
?y a rdfs:Literal 
}
```
Résultat avec inférence RDFS :
```{csv}
Rien car les données ne contiennent pas de rdfs:Literal (verifiable avec select * where {?x a rdfs:Literal})
```
Résultat sans inférence RDFS :
```{csv}
Rien car les données ne contiennent pas de rdfs:Literal (verifiable avec select * where {?x a rdfs:Literal})
```
