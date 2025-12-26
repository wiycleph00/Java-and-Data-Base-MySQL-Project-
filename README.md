# Java-and-Data-Base-MySQL-Project-
Répertoire GitHub d’une application Java console utilisant JDBC et MySQL pour gérer artistes, œuvres et expositions.

# Galerie BelArt - Application de Gestion POO

## Description
Application Java console professionnelle pour gérer une galerie d'art avec base de données MySQL.
**Architecture POO complète** avec pattern DAO (Data Access Object).

Développé dans le cadre d'un projet pédagogique de Programmation Orientée Objet.

---

## Architecture du projet

### Structure des packages
```
src/
├── models/              # Classes métier (modèle)
│   ├── Artiste.java
│   ├── Oeuvre.java
│   └── Exposition.java
├── dao/                 # Accès aux données (Data Access Object)
│   ├── ArtisteDAO.java
│   ├── OeuvreDAO.java
│   └── ExpositionDAO.java
├── database/            # Gestion de la connexion
│   └── DatabaseManager.java
├── utils/               # Utilitaires
│   └── InputValidator.java
└── Main.java            # Point d'entrée de l'application
```

---

## Principes POO utilisés

### 1. **Encapsulation**
- Attributs privés avec getters/setters
- Classes bien structurées avec responsabilités claires

### 2. **Séparation des responsabilités**
- **Models** : Représentent les entités métier
- **DAO** : Gèrent l'accès aux données
- **Utils** : Fournissent des fonctions utilitaires
- **Main** : Gère l'interface utilisateur

### 3. **Pattern DAO (Data Access Object)**
- Abstraction de l'accès aux données
- Séparation logique métier / accès base de données
- Facilite les tests et la maintenance

### 4. **Pattern Singleton**
- `DatabaseManager` : Une seule connexion pour toute l'application

### 5. **Configuration externalisée**
- Fichier `config.properties` pour les identifiants
- Pas de mots de passe en dur dans le code

---

## Prérequis

- **Java JDK 11 ou supérieur**
- **MySQL Server 8.0+**
- **MySQL Connector/J** (Driver JDBC)
- **VS Code** avec Extension Pack for Java (ou autre IDE)

---

## Installation

### 1. Cloner ou télécharger le projet

```bash
git clone https://github.com/votre-repo/BelArtApp.git
cd BelArtApp
```

### 2. Structure du projet

```
BelArtApp/
├── config.properties              # Configuration (à créer)
├── config.properties.example      # Template
├── .gitignore                     # Protection des fichiers sensibles
├── src/
│   ├── models/
│   ├── dao/
│   ├── database/
│   ├── utils/
│   └── Main.java
├── lib/
│   └── mysql-connector-j-9.5.0.jar
├── .vscode/
│   └── settings.json
└── README.md
```

### 3. Créer la base de données

Ouvrez **MySQL Workbench** et exécutez :

```sql
DROP DATABASE IF EXISTS BelartDB;
CREATE DATABASE BelartDB;
USE BelartDB;

CREATE TABLE Artiste (
    id_artiste INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    prenom VARCHAR(100) NOT NULL,
    nationalite VARCHAR(100)
);

CREATE TABLE Oeuvre (
    id_oeuvre INT PRIMARY KEY AUTO_INCREMENT,
    titre VARCHAR(200) NOT NULL,
    type VARCHAR(50) NOT NULL,
    annee INT,
    id_artiste INT NOT NULL,
    FOREIGN KEY (id_artiste) REFERENCES Artiste(id_artiste) ON DELETE CASCADE
);

CREATE TABLE Exposition (
    id_exposition INT PRIMARY KEY AUTO_INCREMENT,
    titre VARCHAR(200) NOT NULL,
    date_debut DATE NOT NULL,
    date_fin DATE NOT NULL,
    CHECK (date_fin >= date_debut)
);

CREATE TABLE Oeuvre_Exposition (
    id_oeuvre INT,
    id_exposition INT,
    PRIMARY KEY (id_oeuvre, id_exposition),
    FOREIGN KEY (id_oeuvre) REFERENCES Oeuvre(id_oeuvre) ON DELETE CASCADE,
    FOREIGN KEY (id_exposition) REFERENCES Exposition(id_exposition) ON DELETE CASCADE
);

-- Données de test
INSERT INTO Artiste (nom, prenom, nationalite) VALUES
('Picasso', 'Pablo', 'Espagnole'),
('Monet', 'Claude', 'Française'),
('Van Gogh', 'Vincent', 'Néerlandaise');

INSERT INTO Oeuvre (titre, type, annee, id_artiste) VALUES
('Guernica', 'Tableau', 1937, 1),
('Les Demoiselles d''Avignon', 'Tableau', 1907, 1),
('Impression, soleil levant', 'Tableau', 1872, 2),
('Les Nymphéas', 'Tableau', 1916, 2),
('La Nuit étoilée', 'Tableau', 1889, 3);

INSERT INTO Exposition (titre, date_debut, date_fin) VALUES
('Exposition Cubisme', '2024-01-15', '2024-03-15'),
('Impressionnisme Moderne', '2024-04-01', '2024-06-30'),
('Chefs-d''œuvre du XIXe siècle', '2024-07-01', '2024-09-30');

INSERT INTO Oeuvre_Exposition (id_oeuvre, id_exposition) VALUES
(1, 1), (2, 1), (3, 2), (4, 2), (5, 3), (3, 3);
```

### 4. Configuration

**A. Fichier `.vscode/settings.json` :**
```json
{
    "java.project.sourcePaths": ["src"],
    "java.project.outputPath": "bin",
    "java.project.referencedLibraries": [
        "lib/**/*.jar"
    ]
}
```

**B. Fichier `config.properties` (à la racine) :**

Copiez `config.properties.example` en `config.properties` et modifiez :

```properties
db.url=jdbc:mysql://localhost:3306/BelartDB
db.user=root
db.password=VOTRE_MOT_DE_PASSE    # ← Modifiez ici !
db.serverTimezone=UTC
db.characterEncoding=UTF-8
db.useSSL=false
db.autoReconnect=true
```

**Important** : Le fichier `config.properties` est ignoré par Git pour protéger vos identifiants.

### 5. Télécharger le driver JDBC

1. Téléchargez MySQL Connector/J : https://dev.mysql.com/downloads/connector/j/
2. Choisissez "Platform Independent" (ZIP)
3. Extrayez le fichier `.jar`
4. Placez-le dans le dossier `lib/`

---

##  Lancement

### Avec VS Code :
1. Ouvrez le dossier du projet
2. Ouvrez `Main.java`
3. Cliquez sur "Run" au-dessus de `public static void main`

### Avec le terminal :
```bash
# Compilation
javac -d bin -cp "lib/*" src/**/*.java src/*.java

# Exécution (Windows)
java -cp "bin;lib/*" Main

# Exécution (Mac/Linux)
java -cp "bin:lib/*" Main
```

---

## Fonctionnalités

### Gestion des Artistes
-  Ajouter un artiste
-  Afficher tous les artistes (tableau formaté)
-  Rechercher par nom
-  Modifier un artiste
-  Supprimer un artiste (avec confirmation)

###  Gestion des Œuvres
-  Ajouter une œuvre
-  Afficher toutes les œuvres (tableau formaté)
-  Filtrer par type (Tableau, Sculpture, etc.)
-  Modifier une œuvre
-  Supprimer une œuvre (avec confirmation)

###  Gestion des Expositions
-  Créer une exposition (avec validation des dates)
-  Afficher toutes les expositions (tableau formaté avec statut)
-  Ajouter une œuvre à une exposition
-  Retirer une œuvre d'une exposition
-  Supprimer une exposition (avec confirmation)

###  Consultation
-  Voir les œuvres d'un artiste
-  Voir les œuvres d'une exposition
-  Voir les expositions d'une œuvre
-  Voir les expositions en cours

###  Statistiques
-  Nombre total d'artistes, œuvres, expositions
-  Artiste le plus prolifique
-  Exposition la plus fournie
-  Répartition par type d'œuvre
-  Répartition par période/décennie

---

##  Améliorations par rapport à la version de base

### Interface utilisateur
-  Tableaux formatés avec bordures Unicode
-  Colonnes alignées pour une meilleure lisibilité
-  Pause après chaque action
-  Messages clairs si liste vide
-  Validation robuste des entrées
-  Annulation possible partout ('r' ou 0)
-  Confirmations pour les suppressions
-  Indicateur de statut pour les expositions (EN COURS / Terminée)

### Fonctionnalités avancées
-  Recherche par nom (artiste)
-  Filtrage par type (œuvres)
-  Voir les expositions d'une œuvre
-  Retirer une œuvre d'une exposition
-  Expositions en cours
-  Statistiques complètes
-  Configuration externalisée (config.properties)

### Architecture
-  **POO complète** avec séparation des responsabilités
-  **Pattern DAO** pour l'accès aux données
-  **Pattern Singleton** pour la connexion
-  **Classes métier** bien structurées
-  **Validation centralisée** des entrées
-  **Gestion d'erreurs** robuste
-  **Code maintenable** et extensible
-  **Sécurité** : mot de passe hors du code

---

##  Structure de la base de données

### Tables
- **Artiste** : id_artiste, nom, prenom, nationalite
- **Oeuvre** : id_oeuvre, titre, type, annee, id_artiste (FK)
- **Exposition** : id_exposition, titre, date_debut, date_fin
- **Oeuvre_Exposition** : id_oeuvre (FK), id_exposition (FK)

### Relations
- Un artiste peut créer plusieurs œuvres (1:N)
- Une œuvre appartient à un artiste (N:1)
- Une exposition peut contenir plusieurs œuvres (N:M)
- Une œuvre peut être dans plusieurs expositions (N:M)

---

## Concepts POO démontrés

### Encapsulation
- Attributs privés avec getters/setters publics
- Méthodes publiques pour l'interface, logique privée

### Abstraction
- Classes DAO abstraient l'accès à la base de données
- L'utilisateur ne voit que les méthodes métier

### Modularité
- Chaque classe a une responsabilité unique (SRP)
- Facile d'ajouter de nouvelles fonctionnalités

### Réutilisabilité
- Classes modèles réutilisables
- Méthodes utilitaires centralisées
- Pattern DAO réutilisable pour d'autres projets

---

##  Résolution de problèmes

### Erreur "Access denied for user"
- Vérifiez vos identifiants dans `config.properties`
- Assurez-vous que MySQL est démarré

### Erreur "ClassNotFoundException"
- Le driver MySQL n'est pas trouvé
- Vérifiez que le `.jar` est dans `lib/`
- Vérifiez `settings.json`

### Erreur "Unknown database"
- La base `BelartDB` n'existe pas
- Exécutez le script SQL de création

### Erreur "Fichier config.properties introuvable"
- Le fichier doit être **à la racine** du projet (même niveau que `src/`)
- Vérifiez l'orthographe exacte : `config.properties`

### Le bouton Run n'apparaît pas
- Fermez et rouvrez VS Code
- Attendez que l'analyse du projet se termine
- Installez "Extension Pack for Java"

---

##  Sécurité

-  Configuration externalisée dans `config.properties`
-  `.gitignore` empêche le partage des identifiants
-  PreparedStatement pour éviter les injections SQL
-  Validation de toutes les entrées utilisateur

---

##  Exemple d'affichage

### Menu principal
```
╔════════════════════════════════════════╗
║   GALERIE D'ART BELART - MENU          ║
╚════════════════════════════════════════╝
1. Gestion des Artistes
2. Gestion des Œuvres
3. Gestion des Expositions
4. Consultation
5. Statistiques
0. Quitter
```

### Tableau des artistes
```
┌──────┬─────────────────────┬─────────────────────┬─────────────────────┬──────────┐
│  ID  │        NOM          │       PRÉNOM        │     NATIONALITÉ     │ ŒUVRES   │
├──────┼─────────────────────┼─────────────────────┼─────────────────────┼──────────┤
│ 1    │ Picasso             │ Pablo               │ Espagnole           │ 2        │
│ 2    │ Monet               │ Claude              │ Française           │ 2        │
│ 3    │ Van Gogh            │ Vincent             │ Néerlandaise        │ 1        │
└──────┴─────────────────────┴─────────────────────┴─────────────────────┴──────────┘
```

---

##  Auteur

Projet pédagogique POO - Gestion de galerie d'art

---

##  Licence

Ce projet est développé dans un cadre éducatif.

---

##  Notes pour l'évaluation

### Points forts du projet :

1. **Architecture POO complète**
   - Séparation claire des responsabilités
   - Pattern DAO professionnel
   - Classes métier bien encapsulées

2. **Bonnes pratiques**
   - Configuration externalisée
   - Validation des entrées
   - Gestion d'erreurs robuste
   - Code commenté et documenté

3. **Interface utilisateur soignée**
   - Tableaux formatés professionnels
   - Messages clairs et informatifs
   - Navigation intuitive

4. **Fonctionnalités complètes**
   - CRUD complet pour toutes les entités
   - Fonctionnalités avancées (recherche, filtres, stats)
   - Relations N:M gérées correctement

5. **Sécurité**
   - Pas de mot de passe en dur
   - Protection contre injections SQL
   - Validation de toutes les entrées

---

##  Support

Pour toute question ou problème :
1. Consultez la section "Résolution de problèmes"
2. Vérifiez la configuration dans `config.properties`
3. Assurez-vous que MySQL est démarré
