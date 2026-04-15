# 1.1 Compréhension de la Problématique

### Le problème métier concret

Dans un site e-commerce basé sur SAP Hybris, chaque produit est représenté par une **fiche produit** contenant des attributs : titre, description, couleur, matière, catégorie, taille, saisonnalité, etc. Ces attributs servent à deux choses critiques :

- **L'expérience utilisateur** : un client qui filtre par "couleur bleue" ou "matière coton" ne trouvera rien si ces attributs sont absents ou mal renseignés
- **Le moteur de recherche Apache Solr** : Solr indexe ces attributs pour alimenter les facettes (filtres latéraux) et améliorer le ranking des résultats

Le problème chez DECADE est que ces attributs sont **incomplets, incohérents ou absents** sur une grande partie du catalogue. Pourquoi ? Parce qu'ils sont renseignés **manuellement** par des équipes humaines qui :

- Font des erreurs de saisie (couleur "bleu" vs "bleu marine" vs "navy")
- Omettent des attributs jugés secondaires mais pourtant utiles
- N'ont pas le temps de traiter des catalogues contenant parfois des **milliers de références**
- Ne sont pas toujours experts du domaine produit (ex : distinguer "lin" de "coton" sur une image)

#### **Questions à poser**

<aside>
⁉️

**Sur les données** :

- Quel est le volume du catalogue produit ? (100 produits ? 10 000 ? 100 000 ?)=⇒ 100 milles / produits de base et variantes(couleur ⇒ taille)
- Quel pourcentage d'attributs est actuellement manquant ? (pour avoir un KPI de départ)
- Les images produit sont-elles systématiquement disponibles, ou parfois absentes ?⇒oui
- Les descriptions textuelles existantes sont-elles fiables ou aussi problématiques ?⇒ non

**Sur l'environnement technique** :

- Quelle version de SAP Hybris / SAP Commerce Cloud est utilisée ? (2211.4)
- Quels attributs sont déjà indexés ? ⇒(que la variante taille)
- Le LLM sera-t-il une API externe (OpenAI, Google, Anthropic) ou un modèle hébergé en interne ? (question de confidentialité des données produit) ⇒ apres de l’etude d’existant
- Y a-t-il des contraintes de performance ? (enrichissement en temps réel à la création produit, ou en batch la nuit ?) ⇒ batch (..)

**Sur le périmètre** :

- Quels attributs sont prioritaires à enrichir ? (couleur ? description ? catégorie ?)⇒ à decider par LLM
- Y a-t-il des marchés multilingues à couvrir dès le départ, ou c'est une fonctionnalité secondaire ?⇒ multilingue
- Qui sont les utilisateurs du dashboard de validation ? (équipe technique, équipe métier, les deux ?) ⇒agents de commerce (jacadi)
</aside>

#### **exemples concrets**

> fournir **5 à 10 fiches produit réelles** (exportées de Hybris), avec différents niveaux de complétude :
> 
> 
> > Une fiche bien renseignée → pour comprendre à quoi ressemble l'idéal
> > 
> 
> > Des fiches partiellement renseignées → le cas le plus courant
> > 
> 
> > Des fiches quasi vides → le cas extrême à traiter
> > 

Cela permettra dès maintenant de :

1. Comprendre le modèle de données Hybris utilisé (quels attributs existent dans le système)
2. Identifier visuellement les lacunes les plus fréquentes
3. Avoir des données réelles pour tester ton LLM lors du POC