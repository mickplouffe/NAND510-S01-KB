# Conventions de nommage / Naming Conventions

## 📝 Vue d'ensemble / Overview
Ce document établit les conventions de nommage standardisées pour tous les assets, fichiers, et éléments du projet.

This document establishes standardized naming conventions for all assets, files, and project elements.

## 🎨 Assets 3D / 3D Assets

### Modèles / Models
- **Format**: `[Type]_[Nom]_[Variation]_[LOD]`
- **Exemple / Example**: `SM_Chair_Wood_LOD0`, `SK_Character_Hero_LOD1`
- **Préfixes / Prefixes**:
  - `SM_` - Static Mesh
  - `SK_` - Skeletal Mesh
  - `SKM_` - Skeletal Mesh (alternative)

### Textures / Textures
- **Format**: `T_[NomAsset]_[Type]_[Resolution]`
- **Exemple / Example**: `T_Wall_Diffuse_2K`, `T_Metal_Normal_1K`
- **Suffixes de type / Type Suffixes**:
  - `_D` ou `_Diffuse` - Diffuse/Albedo
  - `_N` ou `_Normal` - Normal Map
  - `_R` ou `_Roughness` - Roughness
  - `_M` ou `_Metallic` - Metallic
  - `_AO` - Ambient Occlusion
  - `_E` ou `_Emissive` - Emissive
  - `_H` ou `_Height` - Height Map

### Matériaux / Materials
- **Format**: `M_[Nom]_[Variation]`
- **Exemple / Example**: `M_Stone_Granite`, `M_Metal_Brushed`
- **Préfixes / Prefixes**:
  - `M_` - Material
  - `MI_` - Material Instance
  - `MF_` - Material Function

## 🎬 Animation

### Fichiers d'animation / Animation Files
- **Format**: `A_[Personnage]_[Action]_[Variation]`
- **Exemple / Example**: `A_Hero_Walk_Forward`, `A_Enemy_Attack_Sword`

### Rigging
- **Format**: `RIG_[Nom]_[Version]`
- **Exemple / Example**: `RIG_Character_v01`, `RIG_Vehicle_v02`

## 💻 Code

### Fichiers C++
- **Classes**: PascalCase - `CharacterController.h`, `GameManager.cpp`
- **Variables**: camelCase - `playerHealth`, `maxSpeed`
- **Constantes**: UPPER_SNAKE_CASE - `MAX_PLAYERS`, `DEFAULT_SPEED`
- **Fonctions**: PascalCase pour publiques, camelCase pour privées

### Scripts Python
- **Fichiers**: snake_case - `import_textures.py`, `batch_export.py`
- **Classes**: PascalCase - `AssetManager`, `SceneParser`
- **Fonctions**: snake_case - `process_file()`, `export_meshes()`
- **Constantes**: UPPER_SNAKE_CASE - `MAX_ITERATIONS`, `DEFAULT_PATH`

## 🗂️ Fichiers de projet / Project Files

### Scènes / Scenes
- **Format**: `[Projet]_[Nom]_[Version]`
- **Exemple / Example**: `NAND510_Level01_v03.ma`, `NAND510_Character_v01.blend`

### Fichiers de sauvegarde / Backup Files
- **Format**: `[NomOriginal]_backup_[YYYYMMDD]`
- **Exemple / Example**: `Level01_backup_20240115.ma`

## ✅ Règles générales / General Rules

1. **Pas d'espaces / No Spaces**: Utiliser underscore `_` ou tiret `-`
2. **Pas de caractères spéciaux / No Special Characters**: Éviter les accents et symboles
3. **Langue / Language**: Préférer l'anglais pour la compatibilité internationale
4. **Longueur / Length**: Noms descriptifs mais concis (max 50 caractères recommandé)
5. **Cohérence / Consistency**: Suivre les conventions établies dans le projet

## 📌 Notes
- Ces conventions peuvent être adaptées selon les besoins spécifiques du projet
- Documenter toute exception ou variation dans les notes du projet
- Maintenir la cohérence au sein d'un même projet

These conventions can be adapted according to specific project needs. Document any exceptions or variations in project notes. Maintain consistency within the same project.
