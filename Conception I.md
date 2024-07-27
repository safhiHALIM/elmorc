### Conception du Site "ElMorc" avec UML et Cas d'Utilisation

#### Modélisation UML

Pour une conception plus détaillée, nous allons diviser les diagrammes en deux parties : côté site pour les utilisateurs et côté cPanel pour les administrateurs.

#### 1. Diagrammes de Cas d'Utilisation

**Côté Site (Utilisateur)**

```plaintext
          +-------------------+
          |      Client       |
          +-------------------+
             /       |       \
            /        |        \
           /         |         \
+----------------+   |   +------------------+
| Consulter     |<------>| Voir Détails     |
| Catalogue     |   |   | Produit          |
+----------------+   |   +------------------+
                     |
                +------------------+
                | Ajouter au Panier|
                +------------------+
                     |
                +------------------+
                | Passer Commande  |
                +------------------+
                     |
                +------------------+
                | Créer Compte     |
                +------------------+
                     |
                +------------------+
                | Se Connecter     |
                +------------------+
```

**Côté cPanel (Administrateur)**

```plaintext
          +-------------------+
          |    Administrateur |
          +-------------------+
               /     |    \
              /      |     \
             /       |      \
            /        |       \
+----------------+   |   +-----------------+
| Gérer Produits |<------>| Gérer Commandes|
+----------------+   |   +-----------------+
                     |
               +-----------------+
               | Gérer Utilisateurs|
               +-----------------+
```

#### 2. Diagrammes de Séquence

**Scénario 1 : Passer une Commande (Côté Site)**

```plaintext
Client         Site Web       Serveur      Base de Données
 |                |              |                |
 |   Consulter Catalogue         |                |
 |------------------------------>|                |
 |                |   getProduits()              |
 |                |----------------------------->|
 |                |              |  return Produits |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Voir Détails Produit        |                |
 |------------------------------>|                |
 |                |   getProduit(id)             |
 |                |----------------------------->|
 |                |              |  return Produit |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Ajouter au Panier           |                |
 |------------------------------>|                |
 |                |   addToCart(id, qty)         |
 |                |----------------------------->|
 |                |              |  update Panier |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Passer Commande             |                |
 |------------------------------>|                |
 |                |   createOrder()              |
 |                |----------------------------->|
 |                |              |  save Order    |
 |                |              |  update Stock  |
 |                |<-----------------------------|
 |<---------------|              |                |
 | Confirmation de Commande      |                |
 |------------------------------>|                |
 |                |              |                |
```

**Scénario 2 : Gérer les Produits (Côté cPanel)**

```plaintext
Administrateur  cPanel         Serveur       Base de Données
 |                |              |                |
 |   Ajouter Produit            |                |
 |------------------------------>|                |
 |                |   createProduct(data)        |
 |                |----------------------------->|
 |                |              |  save Product  |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Modifier Produit           |                |
 |------------------------------>|                |
 |                |   updateProduct(id, data)    |
 |                |----------------------------->|
 |                |              |  update Product|
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Supprimer Produit           |                |
 |------------------------------>|                |
 |                |   deleteProduct(id)          |
 |                |----------------------------->|
 |                |              |  remove Product|
 |                |<-----------------------------|
 |<---------------|              |                |
```

#### 3. Diagrammes de Classes

Pour une meilleure gestion des données, voici un diagramme de classes plus détaillé pour les deux côtés (Site et cPanel).

**Diagramme de Classes (Côté Site)**

```plaintext
+-----------------+
|     Produit     |
+-----------------+
| - id            |
| - nom           |
| - description   |
| - prix          |
| - stock         |
| - imageURL      |
+-----------------+
| + ajouterProduit()|
| + modifierProduit()|
| + supprimerProduit()|
+-----------------+

          ^
          |
          |
          |
+-----------------+
|    Utilisateur  |
+-----------------+
| - id            |
| - nom           |
| - email         |
| - motDePasse    |
| - adresse       |
+-----------------+
| + créerCompte()  |
| + seConnecter()  |
| + mettreAJourProfil()|
+-----------------+
        ^
        |
        |
        |
+-----------------+
|     Panier      |
+-----------------+
| - id            |
| - produits[]    |
| - total         |
+-----------------+
| + ajouterProduit()|
| + supprimerProduit()|
| + calculerTotal()  |
+-----------------+

         ^
         |
         |
         |
+-----------------+
|    Commande     |
+-----------------+
| - id            |
| - produits[]    |
| - total         |
| - statut        |
| - date          |
+-----------------+
| + passerCommande()|
| + annulerCommande()|
| + suivreCommande() |
+-----------------+
```

**Diagramme de Classes (Côté cPanel)**

```plaintext
+-----------------+
|     Produit     |
+-----------------+
| - id            |
| - nom           |
| - description   |
| - prix          |
| - stock         |
| - imageURL      |
+-----------------+
| + ajouterProduit()|
| + modifierProduit()|
| + supprimerProduit()|
+-----------------+

         ^
         |
         |
         |
+-----------------+
|   Administrateur|
+-----------------+
| - id            |
| - nom           |
| - email         |
| - motDePasse    |
+-----------------+
| + seConnecter()  |
| + gérerProduits()|
| + gérerCommandes()|
| + gérerUtilisateurs()|
+-----------------+
```

#### 4. Diagramme de Séquence (Côté Site et cPanel)

**Côté Site : Ajouter un Produit au Panier**

```plaintext
Client         Site Web       Serveur      Base de Données
 |                |              |                |
 |   Consulter Catalogue         |                |
 |------------------------------>|                |
 |                |   getProduits()              |
 |                |----------------------------->|
 |                |              |  return Produits |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Voir Détails Produit        |                |
 |------------------------------>|                |
 |                |   getProduit(id)             |
 |                |----------------------------->|
 |                |              |  return Produit |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Ajouter au Panier           |                |
 |------------------------------>|                |
 |                |   addToCart(id, qty)         |
 |                |----------------------------->|
 |                |              |  update Panier |
 |                |<-----------------------------|
 |<---------------|              |                |
```

**Côté cPanel : Gérer les Utilisateurs**

```plaintext
Administrateur  cPanel         Serveur       Base de Données
 |                |              |                |
 |   Ajouter Utilisateur        |                |
 |------------------------------>|                |
 |                |   createUser(data)           |
 |                |----------------------------->|
 |                |              |  save User     |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Modifier Utilisateur       |                |
 |------------------------------>|                |
 |                |   updateUser(id, data)       |
 |                |----------------------------->|
 |                |              |  update User   |
 |                |<-----------------------------|
 |<---------------|              |                |
 |   Supprimer Utilisateur       |                |
 |------------------------------>|                |
 |                |   deleteUser(id)             |
 |                |----------------------------->|
 |                |              |  remove User   |
 |                |<-----------------------------|
 |<---------------|              |                |
```

