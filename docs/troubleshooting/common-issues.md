# Problèmes courants et solutions / Common Issues and Solutions

## 📝 Vue d'ensemble / Overview
Ce document répertorie les problèmes courants rencontrés dans le cours NAND510 et leurs solutions.

This document lists common issues encountered in the NAND510 course and their solutions.

## 🔧 Problèmes généraux / General Issues

### Problème: Fichiers corrompus / Corrupted Files
**Symptômes / Symptoms:**
- Fichier ne s'ouvre pas
- Crash à l'ouverture
- Erreurs de lecture

**Solutions:**
1. Vérifier les fichiers de backup automatiques (.autosave, .bak)
2. Essayer d'ouvrir dans une version plus récente du logiciel
3. Utiliser un outil de réparation si disponible
4. Récupérer depuis version control (Git)

**Prévention / Prevention:**
- Sauvegardes régulières et incrémentales
- Utiliser version control (Git)
- Ne pas forcer la fermeture des logiciels
- Vérifier l'espace disque disponible

### Problème: Performance lente / Slow Performance
**Symptômes / Symptoms:**
- Lag dans l'interface
- Temps de chargement long
- Crash ou freeze

**Solutions:**
```
1. Réduire la complexité:
   - Diminuer les détails du viewport
   - Utiliser des proxies pour assets lourds
   - Désactiver les features non nécessaires

2. Optimiser la scène:
   - Supprimer historique/cache
   - Nettoyer les objets inutilisés
   - Limiter les polygones visibles

3. Hardware:
   - Fermer les autres applications
   - Vérifier l'utilisation RAM/CPU
   - Mettre à jour les drivers GPU
```

### Problème: Import/Export échoue / Import/Export Fails
**Symptômes / Symptoms:**
- Erreur lors de l'import
- Fichier exporté vide ou corrompu
- Textures ou animations manquantes

**Solutions:**
1. Vérifier la compatibilité des formats
2. Confirmer que tous les assets référencés existent
3. Utiliser le format intermédiaire (FBX) pour transferts
4. Vérifier les paramètres d'export (axes, scale, units)
5. Essayer d'exporter en mode ASCII si binaire échoue

**Checklist d'export / Export Checklist:**
```
☑ Tous les objets nécessaires sélectionnés
☑ Transformations appliquées (freeze transforms)
☑ Historique supprimé
☑ Échelle correcte (1 unité Maya = 100 unités UE5)
☑ Axes corrects (Y-up vs Z-up)
☑ Textures dans le chemin d'export
☑ Permissions d'écriture sur le dossier
```

## 🗂️ Problèmes de fichiers / File Issues

### Problème: "File Not Found" / Fichier introuvable
**Solutions:**
1. Vérifier le chemin (pas d'espaces, caractères spéciaux)
2. Utiliser chemins absolus pour tester
3. Vérifier les permissions du dossier
4. Confirmer que le fichier existe réellement
5. Sur Windows, vérifier les backslashes: `C:\path\file.txt`

### Problème: Textures manquantes / Missing Textures
**Solutions:**
```python
# Script pour relinker textures (Maya)
import maya.cmds as cmds
import os

def relink_textures(new_texture_path):
    """Relie toutes les textures vers un nouveau chemin"""
    file_nodes = cmds.ls(type='file')
    
    for node in file_nodes:
        old_path = cmds.getAttr(f"{node}.fileTextureName")
        filename = os.path.basename(old_path)
        new_path = os.path.join(new_texture_path, filename)
        
        if os.path.exists(new_path):
            cmds.setAttr(f"{node}.fileTextureName", 
                        new_path, type="string")
            print(f"Relinked: {filename}")
        else:
            print(f"Not found: {filename}")

# Usage:
relink_textures("C:/Projects/Textures")
```

### Problème: Conflits de versions / Version Conflicts
**Symptômes / Symptoms:**
- "File was saved with newer version"
- Fonctionnalités manquantes après ouverture

**Solutions:**
1. Utiliser la version correcte du logiciel
2. Downgrade en exportant vers format compatible
3. Demander à collègue de sauver en version antérieure
4. Mettre à jour le logiciel si possible

## 💻 Problèmes de code / Code Issues

### Problème: "NullReferenceException" / Référence nulle
**Code problématique / Problematic Code:**
```csharp
// Unity C#
GameObject player = GameObject.Find("Player");
player.transform.position = new Vector3(0, 0, 0);
// Crash si "Player" n'existe pas!
```

**Solution:**
```csharp
GameObject player = GameObject.Find("Player");
if (player != null)
{
    player.transform.position = new Vector3(0, 0, 0);
}
else
{
    Debug.LogError("Player not found!");
}

// Ou mieux, utiliser SerializeField:
[SerializeField]
private GameObject playerPrefab;

void Start()
{
    if (playerPrefab == null)
    {
        Debug.LogError("Player prefab not assigned!");
        return;
    }
    // Use playerPrefab...
}
```

### Problème: Boucle infinie / Infinite Loop
**Symptômes / Symptoms:**
- Application freeze
- CPU 100%
- Doit forcer la fermeture

**Solutions:**
```cpp
// MAUVAIS / BAD
while (true)
{
    // Pas de condition de sortie!
}

// BON / GOOD
int maxIterations = 1000;
int iterations = 0;
while (condition && iterations < maxIterations)
{
    // Do work
    iterations++;
}

if (iterations >= maxIterations)
{
    // Log warning
    Debug.LogWarning("Max iterations reached!");
}
```

### Problème: Memory Leak / Fuite mémoire
**Symptômes / Symptoms:**
- Utilisation RAM augmente constamment
- Performance dégrade avec le temps
- Crash après utilisation prolongée

**Solutions C++:**
```cpp
// MAUVAIS / BAD
void CreateObjects()
{
    for (int i = 0; i < 1000; i++)
    {
        MyObject* obj = new MyObject();
        // Jamais delete!
    }
}

// BON / GOOD
void CreateObjects()
{
    std::vector<std::unique_ptr<MyObject>> objects;
    for (int i = 0; i < 1000; i++)
    {
        objects.push_back(std::make_unique<MyObject>());
    }
    // Automatiquement nettoyé à la fin
}
```

**Solutions Unity C#:**
```csharp
// MAUVAIS / BAD
void Update()
{
    // Crée un nouveau GameObject chaque frame!
    GameObject temp = new GameObject();
}

// BON / GOOD
// Utiliser Object Pooling
public class ObjectPool
{
    private Queue<GameObject> pool = new Queue<GameObject>();
    private GameObject prefab;
    
    public GameObject Get()
    {
        if (pool.Count > 0)
        {
            GameObject obj = pool.Dequeue();
            obj.SetActive(true);
            return obj;
        }
        return Instantiate(prefab);
    }
    
    public void Return(GameObject obj)
    {
        obj.SetActive(false);
        pool.Enqueue(obj);
    }
}
```

## 🎮 Problèmes de build / Build Issues

### Problème: Build échoue / Build Fails
**Solutions générales / General Solutions:**
1. Lire attentivement le message d'erreur
2. Vérifier que toutes les dépendances sont présentes
3. Nettoyer et rebuilder (`Clean Build`)
4. Vérifier les paramètres du build (plateforme, architecture)
5. S'assurer que tous les fichiers sont sauvegardés

### Problème: "Missing Reference" dans le build
**Symptômes / Symptoms:**
- Build réussit mais crash au lancement
- Objets manquants dans le jeu

**Solutions Unity:**
```
1. Vérifier que toutes les scènes sont ajoutées dans Build Settings
2. S'assurer que tous les assets utilisés sont dans le projet
3. Vérifier les références dans Resources folder
4. Tester avec Development Build pour voir les logs
```

**Solutions Unreal:**
```
1. Vérifier que tous les Content est inclus dans le package
2. Cook content avant packaging
3. Vérifier les References dans Reference Viewer
4. Packager en Development pour debugging
```

## 🔍 Outils de debugging / Debugging Tools

### Logs et Console
```csharp
// Unity
Debug.Log("Info message");
Debug.LogWarning("Warning message");
Debug.LogError("Error message");
Debug.LogException(exception);

// Conditional logging
#if UNITY_EDITOR
    Debug.Log("Editor only message");
#endif
```

```cpp
// Unreal Engine
UE_LOG(LogTemp, Log, TEXT("Info message"));
UE_LOG(LogTemp, Warning, TEXT("Warning: %s"), *VariableName);
UE_LOG(LogTemp, Error, TEXT("Error occurred!"));

// Screen messages
GEngine->AddOnScreenDebugMessage(-1, 5.0f, FColor::Red, TEXT("Message"));
```

### Profiling
**Unity:**
- Window > Analysis > Profiler
- Deep Profile pour voir tous les appels
- Frame Debugger pour rendu

**Unreal:**
- `stat fps` - Affiche FPS
- `stat unit` - Temps frame
- `stat game` - Game thread
- `profilegpu` - GPU profiling

### Breakpoints et Step Debugging
```
Visual Studio:
- F9: Toggle breakpoint
- F5: Start debugging
- F10: Step over
- F11: Step into
- Shift+F11: Step out

Visual Studio Code:
- Click à gauche du numéro de ligne pour breakpoint
- F5: Start/Continue
- F10: Step over
- F11: Step into
```

## 📋 Checklist de résolution / Troubleshooting Checklist

Avant de demander de l'aide / Before asking for help:

```
☑ J'ai lu le message d'erreur complet
☑ J'ai cherché l'erreur sur Google/Stack Overflow
☑ J'ai vérifié la documentation officielle
☑ J'ai essayé de redémarrer le logiciel
☑ J'ai vérifié que mes fichiers sont sauvegardés
☑ J'ai essayé sur un projet de test simple
☑ Je peux reproduire le problème de manière consistante
☑ J'ai noté les étapes pour reproduire le problème
☑ J'ai vérifié les logs/console pour plus d'infos
```

## 🆘 Obtenir de l'aide / Getting Help

### Informations à fournir / Information to Provide
```
1. Description claire du problème
2. Étapes pour reproduire
3. Messages d'erreur complets (screenshots)
4. Version du logiciel
5. Système d'exploitation
6. Ce que vous avez déjà essayé
7. Code minimal qui reproduit le problème (si applicable)
```

### Ressources d'aide / Help Resources
- Documentation officielle du logiciel
- Forums officiels (Unity Answers, Unreal Forums)
- Stack Overflow (tag approprié)
- Discord communautaire
- Documentation de ce wiki
- Professeur / TAs du cours

## 💡 Bonnes pratiques pour éviter les problèmes / Best Practices

### 1. Sauvegardes régulières / Regular Backups
```
- Sauvegardes automatiques activées
- Incremental saves (v01, v02, etc.)
- Version control (Git)
- Backup sur cloud/externe
```

### 2. Organisation / Organization
```
- Structure de dossiers claire
- Nommage cohérent
- Documentation du code
- Commentaires pour parties complexes
```

### 3. Tests fréquents / Frequent Testing
```
- Tester après chaque changement majeur
- Build régulièrement
- Test sur hardware cible
- Validation des assets
```

### 4. Version Control
```bash
# Git workflow basique
git status
git add .
git commit -m "Description des changements"
git push

# Avant de commencer nouveau feature
git pull
git checkout -b feature/nom-feature

# Merger après completion
git checkout main
git merge feature/nom-feature
```

## 📚 Ressources supplémentaires / Additional Resources

### Outils de diagnostic / Diagnostic Tools
- **Process Explorer** (Windows) - Moniteur processus avancé
- **Activity Monitor** (macOS) - Utilisation ressources
- **GPU-Z** - Info GPU
- **Dependency Walker** - Dépendances DLL (Windows)

### Documentation
- Stack Overflow
- Official Software Documentation
- GitHub Issues des projets open source
- Reddit communities (r/gamedev, r/unrealengine, r/Unity3D)

## 📌 Notes
Ce document est un work in progress. Ajoutez vos propres solutions et problèmes découverts!

This document is a work in progress. Add your own solutions and discovered issues!
