# Exercice 14-1 : Valeurs par défaut dans MySQL 8.0

## Objectif

Cet exercice vise à vous familiariser avec l'utilisation des valeurs par défaut dans MySQL 8.0. Vous allez créer des tables, insérer des données et observer comment les valeurs par défaut sont appliquées.

## Prérequis

* Avoir une installation de MySQL 8.0 fonctionnelle.
* Comprendre les concepts de base des tables et des types de données dans SQL.
* Avoir étudié la théorie sur les valeurs par défaut dans MySQL 8.0.

## Instructions

1.  **Création de la base de données et de la table `t1`**

    * Exécutez les commandes suivantes pour créer une base de données `defaulttest` et une table `t1` avec des valeurs par défaut.
    * Observez la structure de la table `t1` en utilisant `DESCRIBE t1;`.
    * Insérez des données dans `t1` en omettant certaines colonnes et observez les valeurs par défaut appliquées.
    * Affichez le contenu de la table `t1`.

    ```sql
    CREATE DATABASE if not exists defaulttest ;
    use defaulttest;

    CREATE TABLE t1 (
        id SERIAL ,
        i INT DEFAULT -1,
        c VARCHAR(10 ) DEFAULT '' ,
        price DOUBLE(16,2) DEFAULT 0.00
    )ENGINE=InnoDB;

    describe t1;
    insert into t1  (i,c) values(2,'A');
    insert into t1  (price) values(20.5);
    select * from t1;
    ```

2.  **Création et manipulation de la table `t0`**

    * Créez une table `t0` similaire à `t1`.
    * Insérez différentes combinaisons de données, en incluant et en omettant des valeurs.
    * Observez comment les valeurs par défaut sont utilisées.
    * Affichez le contenu de `t0`.

    ```sql
    CREATE TABLE t0 (
        id SERIAL ,
        i INT DEFAULT -1,
        c VARCHAR(10) DEFAULT '' ,
        price DOUBLE(16,2) DEFAULT 0.00
    ) ENGINE=InnoDB;

    desc t0;
    INSERT INTO t0 (i,c,price) values (10,'Allo',12.5);
    INSERT INTO t0 (id,c,price) values (10,'A4',0.5);
    INSERT INTO t0 (price) values (099.5);
    INSERT INTO t0 values ();
    SELECT * from t0;
    ```

3.  **Valeurs par défaut avec expressions**

    * Créez une table `t2` avec des valeurs par défaut définies par des expressions (par exemple, `RAND()`, `UUID()`, `CURRENT_DATE + INTERVAL 1 YEAR`).
    * Insérez des données et observez comment ces expressions sont évaluées.

    ```sql
    DROP TABLE IF EXISTS t2;
    CREATE TABLE t2 (
        -- literal defaults
        i INT DEFAULT 0 ,
        c VARCHAR(10 ) DEFAULT '' ,
        -- expression defaults
        f FLOAT DEFAULT (RAND()*RAND()),
        b BINARY(16) DEFAULT (UUID_TO_BIN(UUID())),
        d DATE DEFAULT (CURRENT_DATE + INTERVAL 1 YEAR),
        p POINT DEFAULT ( Point(0,0)),
        j JSON DEFAULT (JSON_ARRAY())
    )ENGINE=InnoDB;

    describe t2;
    insert into t2  (i,c) values(2,'A');
    select * from t2;
    ```

4.  **Valeurs par défaut pour les types `TIMESTAMP` et `DATETIME`**

    * Créez une table `t3` pour tester les valeurs par défaut de `TIMESTAMP` et `DATETIME`.
    * Insérez des données et observez comment `CURRENT_TIMESTAMP` est utilisé.
    * Testez le comportement de `ON UPDATE CURRENT_TIMESTAMP`.
    * Testez les valeurs par défaut à 0.

    ```sql
    CREATE TABLE t3 (
        ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE 
        CURRENT_TIMESTAMP,
        dt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE
        CURRENT_TIMESTAMP
    )ENGINE=InnoDB;

    insert into t3 values();
    select * from t3;

    CREATE TABLE t1 (
        ts TIMESTAMP DEFAULT 0 , 
        dt DATETIME DEFAULT 0
    )ENGINE=InnoDB;
    insert into t3 values();
    select * from t3;

    DROP table t1;
    CREATE TABLE t1 (
        id Serial,
        val varchar(3),
        ts TIMESTAMP DEFAULT 0 ON UPDATE CURRENT_TIMESTAMP, 
        dt DATETIME DEFAULT 0 ON UPDATE CURRENT_TIMESTAMP
    )ENGINE=InnoDB;
    insert into t1 values();
    insert into t1 (val) values('A'),('B'),('C');
    select * from t1;
    update t1 set val='W' where id=1;
    update t1 set dt=now() where id=2;
    ```

5.  **Comportement de `ON UPDATE CURRENT_TIMESTAMP` et `NULL`**

    * Créez une table `t1` pour observer les différences entre `TIMESTAMP ON UPDATE CURRENT_TIMESTAMP` et `TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP`.
    * Créez une table `t1` pour observer les différences entre `DATETIME ON UPDATE CURRENT_TIMESTAMP` et `DATETIME NOT NULL ON UPDATE CURRENT_TIMESTAMP`.

    ```sql
    CREATE TABLE t1 (
        ts1 TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, -- default 0
        ts2 TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP -- default NULL
    )ENGINE=InnoDB;

    CREATE TABLE t1 (
        dt1 DATETIME ON UPDATE CURRENT_TIMESTAMP, -- default NULL
        dt2 DATETIME NOT NULL ON UPDATE CURRENT_TIMESTAMP -- default 0
    )ENGINE=InnoDB;
    ```

6.  **COMMIT et ROLLBACK**

    * Utilisez la base de données `MODULE06_elevage`.
    * Effectuez une transaction avec `COMMIT` et `ROLLBACK` pour comprendre comment les changements sont enregistrés ou annulés.
    * Testez le comportement de `ROLLBACK` après un `COMMIT`.

    ```sql
    USE MODULE06_elevage;
    COMMIT;
    SELECT * FROM race WHERE id = 5;
    UPDATE race SET prix = 145 WHERE id = 5;
    SELECT *FROM race WHERE id = 5;
    ROLLBACK;
    SELECT * FROM race WHERE id = 5;
    -- ROLLBACK after COMMIT
    SELECT * FROM race WHERE id = 5;
    UPDATE race SET prix = 14 WHERE id = 5;
    COMMIT;
    SELECT * FROM race WHERE id = 5;
    ROLLBACK;
    SELECT * FROM race WHERE id = 5;
    ```

## Questions

* Quelles sont les valeurs par défaut appliquées lorsque vous omettez des colonnes lors de l'insertion de données?
* Comment les expressions sont-elles évaluées dans les valeurs par défaut?
* Quelle est la différence entre `TIMESTAMP` et `DATETIME` avec `ON UPDATE CURRENT_TIMESTAMP`?
* Comment `COMMIT` et `ROLLBACK` affectent-ils les modifications apportées aux données?

