# Conventions de code / Code Conventions

## 📝 Vue d'ensemble / Overview
Ce document établit les conventions de code pour maintenir un code propre, lisible et maintenable.

This document establishes coding conventions to maintain clean, readable, and maintainable code.

## 💻 C++

### Style de nommage / Naming Style
```cpp
// Classes: PascalCase
class CharacterController {
    // Variables privées: camelCase avec m_ prefix
    float m_health;
    int m_maxSpeed;
    
    // Variables publiques: camelCase
    bool isAlive;
    
    // Constantes: UPPER_SNAKE_CASE
    static const int MAX_HEALTH = 100;
    
public:
    // Fonctions publiques: PascalCase
    void TakeDamage(float amount);
    float GetHealth() const;
    
private:
    // Fonctions privées: camelCase
    void updateState();
    void calculateDamage();
};

// Namespaces: lowercase
namespace game {
namespace player {
    // ...
}
}

// Enums: PascalCase
enum class WeaponType {
    Sword,
    Bow,
    Staff
};
```

### Formatage / Formatting
```cpp
// Indentation: 4 espaces / 4 spaces
// Accolades: Nouvelle ligne pour fonctions et classes / New line for functions and classes
void MyFunction()
{
    if (condition) {
        // Code ici / Code here
    }
    
    for (int i = 0; i < 10; ++i) {
        // Loop body
    }
}

// Espaces autour des opérateurs / Spaces around operators
int result = a + b * c;
bool check = (x == y) && (z > 0);
```

### Commentaires / Comments
```cpp
// Commentaires de documentation: Doxygen style
/**
 * @brief Calcule les dégâts infligés au personnage
 * @param amount Montant des dégâts
 * @param damageType Type de dégâts
 * @return Dégâts réels appliqués après résistances
 */
float CalculateDamage(float amount, DamageType damageType);

// Commentaires de ligne: Expliquer le "pourquoi", pas le "quoi"
// On utilise une valeur aléatoire pour éviter les patterns prévisibles
float randomFactor = GetRandomValue();
```

## 🐍 Python

### Style de nommage / Naming Style (PEP 8)
```python
# Classes: PascalCase
class AssetManager:
    # Variables d'instance: snake_case
    def __init__(self):
        self.asset_count = 0
        self.max_assets = 1000
        
    # Constantes: UPPER_SNAKE_CASE
    MAX_FILE_SIZE = 1024 * 1024 * 100  # 100MB
    DEFAULT_PATH = "/assets"
    
    # Méthodes publiques: snake_case
    def load_asset(self, file_path):
        pass
    
    # Méthodes privées: _snake_case avec underscore
    def _validate_path(self, path):
        pass
    
    # Méthodes très privées: __snake_case avec double underscore
    def __internal_method(self):
        pass

# Fonctions: snake_case
def process_scene_file(file_path, options=None):
    pass

# Variables globales: snake_case (éviter si possible)
current_project_path = ""
```

### Formatage / Formatting
```python
# Indentation: 4 espaces / 4 spaces
# Lignes: Maximum 79 caractères (PEP 8) / Maximum 79 characters
def long_function_name(
        parameter_one, parameter_two,
        parameter_three, parameter_four):
    """Docstring explaining the function."""
    return parameter_one + parameter_two

# Espaces autour des opérateurs / Spaces around operators
result = value_a + value_b * value_c
is_valid = (x == y) and (z > 0)

# Imports: Groupés et triés / Grouped and sorted
# 1. Standard library
import os
import sys

# 2. Third-party
import maya.cmds as cmds
import numpy as np

# 3. Local
from . import utils
from .core import manager
```

### Commentaires et Docstrings / Comments and Docstrings
```python
def export_mesh(mesh_name, file_path, format_type="fbx"):
    """
    Exporte un mesh vers un fichier.
    
    Args:
        mesh_name (str): Nom du mesh à exporter
        file_path (str): Chemin de destination
        format_type (str): Format d'export (fbx, obj, etc.)
        
    Returns:
        bool: True si l'export a réussi
        
    Raises:
        ValueError: Si le mesh n'existe pas
        IOError: Si le chemin n'est pas accessible
    """
    # Validation du mesh
    if not mesh_exists(mesh_name):
        raise ValueError(f"Mesh '{mesh_name}' not found")
    
    # Export process
    return perform_export(mesh_name, file_path, format_type)
```

## 🎮 Blueprint (Unreal Engine)

### Organisation / Organization
- **Commentaires**: Utiliser des boîtes de commentaires pour grouper les nœuds
- **Variables**: Organiser en catégories
- **Fonctions**: Une fonction = une responsabilité
- **Noms**: Descriptifs et en PascalCase

### Variables Blueprint
```
PlayerHealth (float)
MaxSpeed (float)
IsGrounded (bool)
CurrentWeapon (WeaponType enum)
```

### Fonctions Blueprint
```
TakeDamage
ApplyHeal
CheckGroundBelow
UpdateHealthUI
```

## 🎯 C# (Unity)

### Style de nommage / Naming Style
```csharp
// Classes: PascalCase
public class PlayerController : MonoBehaviour
{
    // Variables publiques: PascalCase
    public float MoveSpeed = 5.0f;
    public int MaxHealth = 100;
    
    // Variables privées: camelCase avec underscore
    private float _currentHealth;
    private bool _isGrounded;
    
    // Variables serialized: camelCase
    [SerializeField]
    private GameObject _weaponPrefab;
    
    // Propriétés: PascalCase
    public bool IsAlive { get; private set; }
    
    // Méthodes: PascalCase
    public void TakeDamage(float amount)
    {
        _currentHealth -= amount;
        UpdateHealthUI();
    }
    
    private void UpdateHealthUI()
    {
        // Implementation
    }
}
```

## ✅ Bonnes pratiques générales / General Best Practices

### 1. Lisibilité / Readability
- Noms descriptifs et clairs
- Éviter les abréviations obscures
- Une seule responsabilité par fonction
- Fonctions courtes (< 50 lignes recommandé)

### 2. Commentaires / Comments
- Expliquer le "pourquoi", pas le "comment"
- Commenter les algorithmes complexes
- Documenter les APIs publiques
- Éviter les commentaires évidents

### 3. Structure / Structure
- Regrouper le code lié
- Séparer les préoccupations (separation of concerns)
- Utiliser des fonctions helper
- Éviter la duplication de code (DRY principle)

### 4. Performance
- Éviter les allocations inutiles
- Optimiser les boucles critiques
- Profiler avant d'optimiser
- Commenter les optimisations non-évidentes

### 5. Sécurité / Safety
- Valider les entrées
- Gérer les erreurs proprement
- Utiliser const/readonly quand approprié
- Éviter les valeurs magiques (utiliser des constantes)

## 📌 Outils / Tools

### Linters et Formatters
- **C++**: clang-format, cpplint
- **Python**: pylint, black, flake8
- **C#**: StyleCop, ReSharper

### Configuration recommandée / Recommended Configuration
Créer des fichiers de configuration dans le projet:
- `.clang-format` pour C++
- `.pylintrc` ou `setup.cfg` pour Python
- `.editorconfig` pour tous les langages

## 📚 Références / References
- [PEP 8 - Python Style Guide](https://www.python.org/dev/peps/pep-0008/)
- [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
- [Unreal Engine Coding Standard](https://docs.unrealengine.com/en-US/ProductionPipelines/DevelopmentSetup/CodingStandard/)
- [Unity C# Coding Conventions](https://unity.com/how-to/naming-and-code-style-tips-c-scripting-unity)
