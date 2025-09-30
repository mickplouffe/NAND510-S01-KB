# UE5 - Résolution de problèmes / UE5 Troubleshooting

## 🚀 Problèmes de démarrage / Startup Issues

### UE5 ne démarre pas / UE5 Won't Start
**Solutions:**
1. Vérifier que GPU supporte DirectX 12 / Vulkan
2. Mettre à jour drivers GPU
3. Lancer Epic Games Launcher > Library > Verify
4. Vérifier les Visual C++ Redistributables installés
5. Réinitialiser config: Supprimer `Saved/Config/` folder

### Projet ne s'ouvre pas / Project Won't Open
**Solutions:**
1. Vérifier version UE5 compatible avec projet
2. Rebuild project: Right-click .uproject > Generate Visual Studio project files
3. Ouvrir en safe mode sans plugins
4. Vérifier logs: `Saved/Logs/`

### Compilation C++ échoue / C++ Compilation Fails
**Solutions:**
```bash
# Clean solution
1. Fermer UE5
2. Supprimer folders: Binaries/, Intermediate/, Saved/
3. Supprimer fichiers: *.sln, *.VC.db
4. Right-click .uproject > Generate Visual Studio project files
5. Build solution dans Visual Studio
```

## 🎨 Problèmes de viewport / Viewport Issues

### Viewport noir ou corrompu / Black Viewport
**Solutions:**
1. Editor Preferences > Scalability > Engine Scalability Settings > Low
2. Project Settings > Engine > Rendering > Forward Renderer (désactiver)
3. Changer RHI: Add `-dx11` ou `-dx12` au launcher shortcut
4. Désactiver Raytracing si activé
5. Console: `r.SetRes 1280x720w`

### Performance viewport lente / Slow Viewport Performance
**Console Commands:**
```
stat fps              // Show FPS
stat unit            // Show frame time
stat gpu             // GPU stats
r.ScreenPercentage 70  // Reduce resolution temporarily
r.ViewDistanceScale 0.5 // Reduce view distance
```

**Settings:**
- Editor Preferences > Performance > Use Less CPU in Background
- Viewport Options > Realtime (désactiver si pas nécessaire)
- Show > Volumes (cacher si pas nécessaire)

## 📦 Problèmes d'assets / Asset Issues

### Asset import échoue / Asset Import Fails
**FBX Import:**
```
☑ Vérifier que FBX est version 2020 ou compatible
☑ Pas de caractères spéciaux dans noms
☑ Tous les nodes ont des noms uniques
☑ Transformations appliquées dans DCC
☑ History supprimé (Maya/Blender)
```

**Solutions si échec:**
1. Essayer d'importer avec "Convert Scene" désactivé
2. Tester avec un mesh simple du même fichier
3. Vérifier console Output Log pour erreurs spécifiques
4. Ré-exporter depuis DCC avec settings différents

### Textures ne s'importent pas / Textures Don't Import
**Solutions:**
1. S'assurer que textures sont dans même dossier que FBX
2. Importer textures manuellement
3. Vérifier format supporté (PNG, TGA, JPG, EXR)
4. Taille texture doit être puissance de 2 (recommandé)
5. Path ne doit pas contenir caractères spéciaux

### Material looks wrong / Material incorrect
**Solutions:**
```
1. Vérifier que sRGB est ON pour Color maps
2. sRGB OFF pour Normal, Roughness, Metallic
3. Recompiler shaders: Build > Build Lighting / Compile Shaders
4. Vérifier dans Material Editor les connections
```

## 🎮 Problèmes de Blueprint / Blueprint Issues

### Blueprint compile errors
**Common Errors:**

**"Accessed None":**
```cpp
// MAUVAIS - pas de null check
PlayerRef->TakeDamage(10);

// BON - avec validation
if (IsValid(PlayerRef))
{
    PlayerRef->TakeDamage(10);
}
```

**Solutions générales:**
1. Valider tous les pointeurs avant utilisation
2. Print String nodes pour debug
3. Vérifier que variables sont initialisées
4. Check Construction Script vs BeginPlay initialization

### Blueprint performance lente / Slow Blueprint Performance
**Optimisations:**
```
☑ Éviter Tick - utiliser Events/Timers
☑ Utiliser Event Dispatcher au lieu de Cast dans Tick
☑ Cacher résultats coûteux (GetAllActors, etc.)
☑ Utiliser Interfaces au lieu de Cast quand possible
☑ Branch vs Sequence - choisir approprié
```

**Profiling:**
```
Console: stat game
Session Frontend: Window > Developer Tools > Session Frontend
```

## 💻 Problèmes C++ / C++ Issues

### Compilation errors common
**Error: "Unresolved external symbol":**
```cpp
// Solution: Ajouter module dans Build.cs
PublicDependencyModuleNames.AddRange(new string[] { 
    "Core", 
    "CoreUObject", 
    "Engine",
    "AIModule"  // Module manquant
});
```

**Error: "Cannot open include file":**
```cpp
// Vérifier chemin include
#include "GameFramework/Actor.h"  // Correct
// #include "Actor.h"             // Peut ne pas fonctionner
```

### Hot Reload ne fonctionne pas / Hot Reload Doesn't Work
**Solutions:**
1. Ne PAS utiliser Hot Reload (souvent problématique)
2. Fermer éditeur, compiler, rouvrir
3. Utiliser Live Coding (Editor Preferences > Live Coding)
4. Ou compiler depuis IDE puis fermer/rouvrir éditeur

### Header vs Forward Declaration
```cpp
// MyActor.h - Utiliser forward declaration quand possible
class ACharacter;  // Forward declaration

class MYGAME_API AMyActor : public AActor
{
    UPROPERTY()
    ACharacter* CharacterRef;  // Pointer = forward declaration OK
};

// MyActor.cpp - Include complet seulement dans .cpp
#include "MyActor.h"
#include "GameFramework/Character.h"
```

## 🎬 Problèmes de lighting / Lighting Issues

### Lighting looks dark / Éclairage sombre
**Solutions:**
1. Build Lighting (Build > Build Lighting Only)
2. Vérifier que Lightmass Importance Volume entoure scène
3. Augmenter Directional Light intensity
4. Ajouter Sky Light
5. Post Process Volume > Exposure: Auto Exposure désactivé

### Lightmap errors / Erreurs lightmap
**"Object has overlapping UVs":**
```
Solutions:
1. Mesh Import > Generate Lightmap UVs (cocher)
2. Ou créer UV channel 1 manuellement dans DCC
3. Static Mesh Editor > Generate Lightmap UVs
```

**"Lightmap resolution too low":**
```
Static Mesh Editor > LOD0 > Build Settings
- Override Lightmap Resolution: 256 ou plus
```

## 📦 Problèmes de packaging / Packaging Issues

### Package build fails
**Solutions:**
```
1. Vérifier Output Log pour erreurs spécifiques
2. Package en Development Build d'abord (plus de logging)
3. Vérifier que tous les maps sont dans Maps to Package
4. Delete Intermediate/ et Saved/ folders
5. Build > Clean Project puis Package
```

### Packaged game crashes / Jeu crash après package
**Debugging:**
```
1. Package en Development avec debug symbols
2. Run packaged .exe depuis command line pour voir logs
3. Vérifier Saved/Logs/ dans package folder
4. Test que tous les assets sont inclus
```

**Common causes:**
- Assets référencés pas inclus dans package
- Plugin manquant dans packaged build
- Path hardcodé qui n'existe pas
- Content pas cooked correctement

## 🔧 Problèmes de performance / Performance Issues

### Low FPS in game
**Profiling:**
```
Console Commands:
stat fps
stat unit    // Shows CPU/GPU time
stat scenerendering
stat memory
profilegpu   // Detailed GPU breakdown
```

**Optimisations:**
```
Rendering:
- r.ScreenPercentage 70
- r.ShadowQuality 2
- r.PostProcessQuality 2
- Enable Hierarchical LOD

Optimization:
- Use LODs on all meshes
- Reduce draw calls (merge actors)
- Cull distance on objects
- Optimize materials (reduce instructions)
```

### Memory issues / Problèmes mémoire
```
Console:
stat memory
memreport full

Solutions:
- Reduce texture sizes
- Use texture streaming
- Optimize mesh complexity
- Clear unused assets
```

## 🐛 Debugging tools / Outils de debugging

### Visual Logging
```cpp
// C++
#include "VisualLogger/VisualLogger.h"

UE_VLOG(this, LogTemp, Log, TEXT("Message"));
UE_VLOG_LOCATION(this, LogTemp, Log, GetActorLocation(), 10, FColor::Red, TEXT("Location"));

// Activer: Console > vislog
// Voir: Window > Developer Tools > Visual Logger
```

### Print debugging
```cpp
// C++
UE_LOG(LogTemp, Warning, TEXT("Health: %f"), Health);
GEngine->AddOnScreenDebugMessage(-1, 5.f, FColor::Red, TEXT("Message"));

// Blueprint: Print String node
```

### Crash debugging
```
1. Crash Reporter envoie rapport automatiquement
2. Vérifier logs: Saved/Logs/ProjectName.log
3. Callstack dans Output Log
4. Attach Visual Studio debugger pour C++ crashes
```

## 📋 Maintenance / Maintenance

### Project cleanup
```
Safe à supprimer:
- Saved/ folder (configs seront régénérées)
- Intermediate/ folder
- DerivedDataCache/
- Build/ folder

NE PAS supprimer:
- Content/ folder
- Config/ folder (sauvegarder si custom settings)
- Source/ folder (code C++)
```

### Shader compilation cache
```
If shaders keep recompiling:
1. Delete DerivedDataCache folder
2. Editor Preferences > General > Shader Compiler > Clear cache
3. Rebuild shaders
```

## 📚 Ressources / Resources

### Documentation
- [UE5 Documentation](https://docs.unrealengine.com/5.0/)
- [AnswerHub](https://answers.unrealengine.com/)
- [Forums](https://forums.unrealengine.com/)

### Logs locations
- `ProjectName/Saved/Logs/`
- Launch log: Most recent file
- Crash reports: `ProjectName/Saved/Crashes/`

### Console commands reference
```
?         // Liste toutes les commandes disponibles
stat      // Liste toutes les stat commands
r.        // Tab complete pour rendering commands
```

### Support
- [UE Forums](https://forums.unrealengine.com/)
- [UE AnswerHub](https://answers.unrealengine.com/)
- [Discord - Unreal Slackers](https://unrealslackers.org/)
- [r/unrealengine](https://www.reddit.com/r/unrealengine/)
