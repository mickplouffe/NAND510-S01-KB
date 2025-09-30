# Structure de dossiers / Folder Structure

## 📁 Vue d'ensemble / Overview
Une structure de dossiers organisée est essentielle pour maintenir un projet propre et efficace.

An organized folder structure is essential to maintain a clean and efficient project.

## 🎮 Unreal Engine 5 (UE5)

```
ProjectName/
├── Content/
│   ├── Art/
│   │   ├── Characters/
│   │   │   ├── Hero/
│   │   │   │   ├── Meshes/
│   │   │   │   ├── Textures/
│   │   │   │   ├── Materials/
│   │   │   │   └── Animations/
│   │   ├── Environment/
│   │   │   ├── Props/
│   │   │   ├── Landscape/
│   │   │   └── Foliage/
│   │   └── Effects/
│   ├── Audio/
│   │   ├── Music/
│   │   ├── SFX/
│   │   └── Voice/
│   ├── Blueprints/
│   │   ├── Characters/
│   │   ├── Gameplay/
│   │   └── UI/
│   ├── Maps/
│   │   ├── Levels/
│   │   └── Test/
│   └── UI/
├── Source/
│   └── [ProjectName]/
│       ├── Private/
│       └── Public/
└── Config/
```

## 🎯 Unity

```
ProjectName/
├── Assets/
│   ├── Art/
│   │   ├── Models/
│   │   │   ├── Characters/
│   │   │   └── Environment/
│   │   ├── Textures/
│   │   ├── Materials/
│   │   └── Animations/
│   ├── Audio/
│   │   ├── Music/
│   │   ├── SFX/
│   │   └── Ambience/
│   ├── Scripts/
│   │   ├── Core/
│   │   ├── Gameplay/
│   │   ├── UI/
│   │   └── Utilities/
│   ├── Scenes/
│   │   ├── Levels/
│   │   └── Test/
│   ├── Prefabs/
│   │   ├── Characters/
│   │   ├── Props/
│   │   └── UI/
│   └── Resources/
├── ProjectSettings/
└── Packages/
```

## 🎨 Maya

```
ProjectName/
├── scenes/
│   ├── assets/
│   │   ├── characters/
│   │   ├── props/
│   │   └── environment/
│   ├── animation/
│   ├── rigging/
│   └── archive/
├── sourceimages/
│   ├── textures/
│   ├── references/
│   └── hdri/
├── exports/
│   ├── fbx/
│   ├── obj/
│   └── alembic/
├── scripts/
│   ├── mel/
│   └── python/
└── cache/
```

## 🔷 Blender

```
ProjectName/
├── blend_files/
│   ├── assets/
│   │   ├── characters/
│   │   ├── props/
│   │   └── environment/
│   ├── animation/
│   ├── rigging/
│   └── archive/
├── textures/
│   ├── source/
│   ├── baked/
│   └── references/
├── exports/
│   ├── fbx/
│   ├── obj/
│   ├── gltf/
│   └── alembic/
├── scripts/
│   └── addons/
└── renders/
    ├── previews/
    └── final/
```

## 💻 Projet C++

```
ProjectName/
├── src/
│   ├── core/
│   ├── engine/
│   ├── game/
│   └── utils/
├── include/
│   └── [ProjectName]/
├── lib/
│   ├── external/
│   └── internal/
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
├── build/
├── bin/
└── assets/
```

## 🐍 Projet Python

```
ProjectName/
├── src/
│   └── project_name/
│       ├── __init__.py
│       ├── core/
│       ├── utils/
│       └── tests/
├── docs/
├── tests/
│   ├── unit/
│   └── integration/
├── scripts/
├── data/
│   ├── input/
│   └── output/
├── requirements.txt
├── setup.py
└── README.md
```

## ✅ Bonnes pratiques / Best Practices

### 1. Organisation / Organization
- **Séparation logique**: Grouper les fichiers par type et fonction
- **Hiérarchie claire**: Maximum 3-4 niveaux de profondeur recommandé
- **Dossiers de travail**: Créer un dossier `WIP` ou `Work` pour les fichiers en cours

### 2. Nommage / Naming
- **Cohérence**: Utiliser les mêmes noms de dossiers dans tous les projets
- **Clarté**: Les noms doivent être explicites et compréhensibles
- **Langue**: Privilégier l'anglais pour la compatibilité

### 3. Maintenance
- **Archive**: Créer un dossier `archive/` pour les anciennes versions
- **Backup**: Maintenir des sauvegardes régulières dans un dossier séparé
- **Documentation**: Inclure un README.md dans les dossiers principaux

### 4. Version Control
- **Ignorer**: Utiliser `.gitignore` pour exclure:
  - Fichiers temporaires
  - Fichiers de cache
  - Fichiers de build
  - Assets binaires volumineux (considérer Git LFS)

### 5. Collaboration
- **Standards**: Documenter la structure dans le README du projet
- **Templates**: Créer des templates de structure pour nouveaux projets
- **Révision**: Revoir la structure régulièrement et l'adapter si nécessaire

## 📌 Notes
La structure peut être adaptée selon:
- La taille du projet
- Le nombre de collaborateurs
- Les exigences spécifiques du pipeline
- Les contraintes techniques

The structure can be adapted according to:
- Project size
- Number of collaborators
- Specific pipeline requirements
- Technical constraints
