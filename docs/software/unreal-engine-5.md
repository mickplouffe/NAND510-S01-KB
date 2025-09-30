# Unreal Engine 5 (UE5)

## 📝 Vue d'ensemble / Overview
Documentation et ressources pour Unreal Engine 5 dans le contexte du cours NAND510.

Documentation and resources for Unreal Engine 5 in the context of the NAND510 course.

## 🚀 Démarrage rapide / Quick Start

### Création de projet / Project Creation
```
Catégories recommandées:
- Games > Third Person (point de départ standard)
- Games > Blank (projet vide)

Settings:
- Blueprint ou C++ (selon besoins)
- Target Platform: Desktop
- Quality: Scalable 3D or 2D
- Starter Content: Oui (pour apprendre)
- Raytracing: Désactivé (sauf si GPU compatible)
```

### Structure de projet / Project Structure
```
ProjectName/
├── Content/          # Assets du jeu
├── Source/          # Code C++
├── Config/          # Configuration files
├── Saved/           # Sauvegardes et caches
├── Intermediate/    # Fichiers temporaires
└── Plugins/         # Plugins tiers
```

## 📐 Content Browser et Assets

### Organisation recommandée / Recommended Organization
```
Content/
├── Art/
│   ├── Characters/
│   ├── Environment/
│   ├── Props/
│   └── Effects/
├── Audio/
│   ├── Music/
│   ├── SFX/
│   └── Dialogue/
├── Blueprints/
│   ├── Characters/
│   ├── Gameplay/
│   └── UI/
├── Maps/
│   ├── Levels/
│   └── Test/
├── Materials/
│   ├── Master/
│   └── Instances/
└── Textures/
```

### Nomenclature des assets / Asset Naming
```
Meshes:
SM_PropName     - Static Mesh
SK_CharName     - Skeletal Mesh

Textures:
T_AssetName_D   - Diffuse/Albedo
T_AssetName_N   - Normal Map
T_AssetName_ORM - Packed (Occlusion/Roughness/Metallic)
T_AssetName_E   - Emissive

Materials:
M_MaterialName  - Material
MI_MaterialName - Material Instance
MF_FunctionName - Material Function

Blueprints:
BP_ClassName    - Blueprint Class
WBP_WidgetName  - Widget Blueprint
```

## 🎮 Blueprints

### Bonnes pratiques / Best Practices
```
1. Organisation:
   - Utiliser des commentaires (C key)
   - Grouper les nœuds logiquement
   - Reroute nodes pour clarté

2. Variables:
   - Noms descriptifs (PascalCase)
   - Catégories pour organisation
   - Tooltips pour documentation
   - Private par défaut

3. Fonctions:
   - Une responsabilité par fonction
   - Inputs/Outputs clairs
   - Pure functions quand possible
   - Documentation dans Description

4. Performance:
   - Éviter Tick quand possible (utiliser Timers)
   - Cacher résultats coûteux
   - Utiliser Event Dispatchers
```

### Raccourcis Blueprint / Blueprint Shortcuts
```
Navigation:
C: Add Comment
Alt + Left Click: Break Links
Double-click wire: Add Reroute Node
Ctrl + W: Duplicate node

Variables & Functions:
Ctrl + Shift + M: Promote to Variable
Ctrl + Shift + F: Promote to Function
Ctrl + K: Add Keyword (search)

Common Nodes (type to search):
B: Branch
D: Delay
E: Event
F: ForEach
G: Get
S: Sequence
T: Timeline
```

## 🎨 Materials et Textures / Materials and Textures

### Material Editor Shortcuts
```
1: Scalar Parameter
2: Vector2 Parameter
3: Vector3 Parameter
4: Vector4 Parameter
S: Scalar
T: Texture Sample
U: TexCoord
M: Multiply
A: Add
L: Lerp
P: Panner
```

### Master Material Pattern
```
Créer un Master Material avec paramètres:
- Base Color
- Metallic
- Roughness
- Normal
- Emissive

Créer des Material Instances pour variation:
- MI_Wood_Oak
- MI_Wood_Walnut
- MI_Metal_Brushed
```

### Texture Setup
```
Texture Import Settings:
- sRGB: ON pour Color maps
- sRGB: OFF pour Normal, ORM maps
- Compression: Default
- Mip Gen Settings: FromTextureGroup
```

## 🎬 Animation et Rigging / Animation and Rigging

### Import FBX Skeletal Mesh
```
Import Settings:
☑ Import Mesh
☑ Import Skeletal Mesh
☑ Import Animations
☑ Import Morph Targets
☑ Preserve Smoothing Groups
☑ Import Materials
☑ Import Textures

Transform:
Import Translation: (0, 0, 0)
Import Rotation: (0, 0, 0)
Import Uniform Scale: 1.0
```

### Animation Blueprint
```
Principaux nodes:
- State Machine: Gestion des états d'animation
- Blend Spaces: Blending directionnel
- Layered Blend Per Bone: Superposition d'animations
- Aim Offset: Look-at et aiming
```

### Retargeting
```
1. Créer un IK Rig pour source et target
2. Créer un IK Retargeter
3. Map les chains (spine, arm_l, arm_r, etc.)
4. Retarget animations
```

## 🌍 Level Design

### Workflow
```
1. Blocking:
   - Utiliser BSP ou cubes simples
   - Établir layout et flow
   - Tester gameplay

2. Meshes:
   - Remplacer par static meshes
   - Utiliser modular pieces
   - Optimiser draw calls

3. Lighting:
   - Directional Light (soleil)
   - Sky Light (ambient)
   - Point/Spot Lights (détails)
   - Light baking pour performance

4. Post-Process:
   - Exposure
   - Color Grading
   - Bloom
   - Ambient Occlusion
```

### World Settings
```
- World Composition: Pour grands mondes
- World Partition: Pour projets UE5
- Data Layers: Organisation et streaming
- Lighting Scenarios: Variantes d'éclairage
```

## 💻 C++ Programming

### Classe basique / Basic Class
```cpp
// MyActor.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class PROJECTNAME_API AMyActor : public AActor
{
    GENERATED_BODY()
    
public:
    AMyActor();
    
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Stats")
    float Health = 100.0f;
    
    UFUNCTION(BlueprintCallable, Category = "Actions")
    void TakeDamage(float DamageAmount);
    
protected:
    virtual void BeginPlay() override;
    
public:
    virtual void Tick(float DeltaTime) override;
};

// MyActor.cpp
#include "MyActor.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();
}

void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}

void AMyActor::TakeDamage(float DamageAmount)
{
    Health -= DamageAmount;
    if (Health <= 0)
    {
        // Handle death
    }
}
```

### Macros importants / Important Macros
```cpp
UCLASS()          // Expose class to UE
UPROPERTY()       // Expose property
UFUNCTION()       // Expose function
GENERATED_BODY()  // Required macro

UPROPERTY modifiers:
- EditAnywhere: Éditable partout
- BlueprintReadWrite: Accessible en Blueprint
- Category: Organisation dans l'éditeur
- VisibleAnywhere: Visible mais non-éditable

UFUNCTION modifiers:
- BlueprintCallable: Appelable depuis Blueprint
- BlueprintPure: Pure function (pas d'exec pin)
- CallInEditor: Bouton dans l'éditeur
```

## 🚀 Performance et Optimisation / Performance and Optimization

### Profiling Tools
```
Console Commands:
stat fps           - Affiche FPS
stat unit          - Temps frame détaillé
stat game          - Game thread stats
stat gpu           - GPU stats
profilegpu         - Détail GPU
dumpticks          - Liste tous les tick actors

Outils:
- Session Frontend: Profiling détaillé
- GPU Visualizer: Analyse rendu
- Unreal Insights: Profiling avancé (UE5)
```

### Optimisations communes / Common Optimizations
```
Rendering:
- LODs (Level of Detail)
- Instanced Static Meshes
- Merged Actors
- Culling Distance
- Occlusion Culling

Blueprints:
- Éviter Tick (utiliser Events/Timers)
- Cast économiquement
- Interfaces pour découplage
- Utiliser Object Pooling

Lighting:
- Static Lighting quand possible
- Lightmass pour baking
- Limiter dynamic lights
- Shadow distance optimization
```

## 🔌 Plugins utiles / Useful Plugins

### Inclus / Built-in
- **Enhanced Input**: Système input moderne
- **Niagara**: Système de particules
- **Chaos**: Système physique
- **Geometry Scripting**: Manipulation géométrie
- **Water**: Système d'eau

### Marketplace (gratuits)
- **Electronic Nodes**: Améliore lisibilité Blueprint
- **Victory Plugin**: Fonctions utilitaires
- **Rama Save System**: Système de sauvegarde
- **Advanced Sessions**: Multijoueur facilité

## 📦 Packaging et Distribution / Packaging and Distribution

### Packaging Settings
```
Edit > Project Settings > Packaging:

Build Configuration:
- Development: Pour tests
- Shipping: Pour release final

Maps to Package:
☑ Ajouter tous les maps nécessaires

Advanced:
☑ For Distribution
□ Include Debug Files (Shipping)
☑ Generate Chunks
```

### Platform Specific
```
Windows:
- Target Platform: Windows
- Architecture: 64-bit

Build Steps:
1. Package Project (File > Package Project)
2. Sélectionner plateforme cible
3. Choisir dossier de sortie
4. Attendre compilation
5. Tester le build!
```

## 📚 Ressources / Resources

### Documentation officielle / Official Documentation
- [UE5 Documentation](https://docs.unrealengine.com/5.0/)
- [API Reference](https://docs.unrealengine.com/5.0/en-US/API/)
- [C++ Programming Guide](https://docs.unrealengine.com/5.0/en-US/ProgrammingAndScripting/ProgrammingWithCPP/)

### Learning
- [Unreal Online Learning](https://www.unrealengine.com/learn)
- [Unreal Dev Community](https://dev.epicgames.com/community/)
- [Math For Game Developers (YouTube)](https://www.youtube.com/c/MathForGameDevelopers)

### Community
- [Unreal Forums](https://forums.unrealengine.com/)
- [r/unrealengine](https://www.reddit.com/r/unrealengine/)
- [Unreal Slackers Discord](https://unrealslackers.org/)
- [UE4 AnswerHub](https://answers.unrealengine.com/)

## 💡 Tips et Astuces / Tips and Tricks

### Editor
```
F11: Immersive Mode (fullscreen)
Alt + P: PIE (Play In Editor)
F8: Eject from PIE (free camera)
Shift + F1: Mouse cursor in PIE
End: Place actor on ground
```

### Content Browser
```
Ctrl + B: Find in Content Browser
Ctrl + Space: Quick search
Right-click > Size Map: Voir taille assets
Right-click > Reference Viewer: Voir dépendances
```

### Best Practices
- Commiter régulièrement avec Git/Perforce
- Documenter les systèmes complexes
- Utiliser Data Assets pour configurations
- Tester sur hardware cible tôt et souvent
- Suivre les coding standards Unreal

## 📌 Notes importantes / Important Notes

### Version Control
```
Fichiers à ignorer (.gitignore):
/Saved/
/Intermediate/
/Build/
*.sln
*.suo
*.user
*.opensdf

Fichiers à versionner:
/Content/
/Config/
/Source/
/Plugins/
*.uproject
```

### Migration entre projets / Migration Between Projects
```
1. Sélectionner assets dans Content Browser
2. Right-click > Migrate
3. Choisir Content folder du projet cible
4. Vérifier dépendances listées
5. OK pour copier
```

### Updates UE5
- Toujours sauvegarder avant update
- Tester dans une branche séparée
- Vérifier changelog pour breaking changes
- Recompiler les plugins C++
