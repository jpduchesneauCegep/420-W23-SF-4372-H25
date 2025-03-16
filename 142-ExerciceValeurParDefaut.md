# Exercice 14-3: Valeurs par défaut dans MySQL 8.0

## Objectif

Cet exercice vise à renforcer votre compréhension des valeurs par défaut dans MySQL 8.0. Vous allez créer une table avec des valeurs par défaut, insérer des données et observer comment les valeurs par défaut sont utilisées.

## Instructions

1.  **Création de la table `produits` :**
    Exécutez la requête SQL suivante pour créer une table nommée `produits` avec les colonnes suivantes :
    * `id` : entier, clé primaire, auto-incrémenté.
    * `nom` : chaîne de caractères (VARCHAR(255)), non nulle.
    * `prix` : nombre décimal (DECIMAL(10, 2)), valeur par défaut de 0.00.
    * `stock` : entier, valeur par défaut de 10.
    * `date_creation` : date et heure (TIMESTAMP), valeur par défaut de la date et heure courantes.

    ```sql
    CREATE TABLE produits (
        id INT AUTO_INCREMENT PRIMARY KEY,
        nom VARCHAR(255) NOT NULL,
        prix DECIMAL(10, 2) DEFAULT 0.00,
        stock INT DEFAULT 10,
        date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    ```

2.  **Insertion de données :**
    * Insérez un produit en spécifiant uniquement le nom.
    * Insérez un produit en spécifiant le nom et le prix.
    * Insérez un produit en spécifiant le nom, le prix et le stock.
    * Insérez un produit en spécifiant toutes les colonnes.

    ```sql
    INSERT INTO produits (nom) VALUES ('Ordinateur portable');
    INSERT INTO produits (nom, prix) VALUES ('Souris', 15.99);
    INSERT INTO produits (nom, prix, stock) VALUES ('Clavier', 29.99, 5);
    INSERT INTO produits (nom, prix, stock, date_creation) VALUES ('Écran', 199.99, 20, '2023-10-27 10:00:00');
    ```

3.  **Affichage des données :**
    Exécutez la requête SQL suivante pour afficher toutes les données de la table `produits`.

    ```sql
    SELECT * FROM produits;
    ```

4.  **Observation et analyse :**
    * Quelles valeurs ont été attribuées aux colonnes `prix`, `stock` et `date_creation` pour le premier produit inséré ? Pourquoi ?
    * Comment les valeurs par défaut ont-elles été utilisées dans les autres insertions ?
    * Comment la colonne `date_creation` est-elle affectée lorsque vous insérez une nouvelle ligne sans spécifier de valeur pour cette colonne ?

5.  **Modification de la valeur par défaut :**
    Modifiez la valeur par défaut de la colonne `stock` à 20.

    ```sql
    ALTER TABLE produits ALTER stock SET DEFAULT 20;
    ```

6.  **Insertion de nouvelles données et vérification :**
    Insérez un nouveau produit en spécifiant uniquement le nom et vérifiez que la valeur de la colonne `stock` est bien 20.

    ```sql
    INSERT INTO produits (nom) VALUES ('Casque audio');
    SELECT * FROM produits;
    ```

## Questions supplémentaires (facultatif)

* Comment supprimer une valeur par défaut d'une colonne ?
* Quelles sont les autres types de valeurs par défaut que MySQL 8.0 supporte ?
* Dans quels cas l'utilisation de valeurs par défaut est-elle particulièrement utile ?

## Conseils

* Utilisez un client MySQL comme MySQL Workbench pour exécuter les requêtes SQL.
* Prenez des captures d'écran des résultats pour les inclure dans votre études.
* Expliquez vos observations et vos réponses en détail.
* N'hésitez pas à consulter la documentation de MySQL 8.0 pour plus d'informations.