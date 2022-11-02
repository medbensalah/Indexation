<style>
t1 { color: #aa0000; font-style: italic; }
t2 { color: #2dc104; font-style: italic; }
t3 { color: #0450c1; font-style: italic; }
r { color: #a11c1c; font-weight: 600; font-style: italic; }
g { color: #12bf12; font-weight: 600; font-style: italic; }
b { color: #060697; font-weight: 600; font-style: italic; }
</style>

# Indexation

### <t1>- Definition</t1><brr>
Processus permettant de construire un ensemble d'éléments
<b>clés</b> permettant de caractériser le contenu d’un 
document / retrouver ce document en réponse à une requête

----------------------------------------------------------------

### <t1>- Indexation Libre / Controlée</t1>

#### <t2>- Indexation Libre</t2>
- Mots, termes des documents

#### <t2>- Indexation Contrôlée</t2>
- Listes de termes prédéfinies 
- Vocabulaire contrôlé 
- Thésaurus

#### <t2>- Construction d'index</t2><br>
![](/assets/Construction.png)

#### <t2>- Tokenisation</t2>
+ Identification des élements élementaires
  + phonèmes
  + morphèmes
  + mots, ...
+ Complexe en certains langues

#### <t2>- Filtrae des mots vides "StopWords"
+ Determinants
+ pronons
+ prépositions, ...

<r>!!! parfois ces mots portent un sens</r>

#### <t2>- Normalisation</t2>
+ <t3>Normalisation textuelle</t3>
  + Unifier les termes
  + Enlever la ponctuation
  + La casse : réduction des lettres en miniscules
  + Enlever les accents
  + Unifier les dates et les valeurs monétraires
+ <t3>Normalisation linguistique</t3> 
  + Lemmatisation
    + verbe => infinitif
    + nom, adj, article => masculin singulier
  + Racinisation (Stemming)
  + Etiquetage
    + Associer aux mots leur catégorie morphosyntaxique (treeTagger)

+ <t3>- Recherche de groupes de mots</t3>
  + n-grammes ; n > 1 / un n-gramme sera considéré comme un terme

#### <t2>Construction de l'index</t3>
+ <t3>Algorithme naïf</t3>

  ```
  input requête q;
  
  for every document d in collection
        if d matches q
            add its docid to list L;

  output list L
  ```

+ <t3>Index inversé</t3>
  + Construire une matrice d'incidence
  
|        | documents |
|--------|:---------:|
| termes |   0 / 1   |

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0: Le terme n'appartient pas au document<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1 : le terme apparait dans le document<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pmatrice éparse => représenter que les 1<br>
+ 
  + Composantes<br>
![](/assets/IndexInverse.png)<br><br>
<b>Dictionnaire : </b>en mémoire centrale permettant un accès rapide
aux termes et leurs informations<br>
<b>Postings : </b>au niveau du disque et sauvegardant les 
informations des termes et les identifiants des documents<br>
Ce qu'on stocke dépend du modèle de la recherche :
    + <g>Recherche booléenne : </g>L'identifiant du document est suffisant<br>
    ![](/assets/Screenshot_93.png)
    + <g>Recherche Rankée : </g>Fréquence ou score du terme<br>
    ![](/assets/Screenshot_94.png)
    + <g>Recherche de proximité / de séquence : </g>Fréquence ou score du terme<br>
    ![](/assets/Screenshot_95.png)
    <br>
    
    Les liste de posting peuvent être triées selon:
    + <g>Document ID : </g>![](/assets/Screenshot_97.png)
    + <g>Term Frequency : </g>![](/assets/Screenshot_96.png)
    
+ <t3>Construction d'un fichier inverse</t3>
  + Extraire les termes de chque document
    
| Termes | Document |
|--------|:--------:|
| terme  |   doc#   |    
+ 
  + Trier les termes par ordre alphabétique<br>
![](/assets/Screenshot_99.png)<br>
  + Indexation<br>
  + ![](/assets/Screenshot_100.png) => ![](/assets/Screenshot_101.png)
  + Compression<br>
  Minimiser le nombre de bits transférés

------------------------------------------------------------------

### <t1>- Variable-Byte Encoding</t1>

Le bit du poids le plus fort est un <b>stop bit</b>
+ Exemple:<br>
13   = 00000000 00000000 00000000 00001101<br>
VBE  : 10001101
+ Exemple:<br>
131  = 00000000 00000000 00000000 10000011<br>
VBE  : 00000001 10000011
+ Exemple:<br>
1337 = 00000000 00000000 00000101 00111001<br>
VBE  : 00001010 10111001

------------------------------------------------------------------

+ Exercice<br>
00000111 10011100 10000101 01111010 01011000 10111101<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;924&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2010173