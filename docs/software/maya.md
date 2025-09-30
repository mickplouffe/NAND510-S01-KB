# Maya

## 📝 Vue d'ensemble / Overview
Documentation et ressources pour Autodesk Maya dans le contexte du cours NAND510.

Documentation and resources for Autodesk Maya in the context of the NAND510 course.

## 🚀 Démarrage rapide / Quick Start

### Configuration de projet / Project Setup
```mel
// Créer un nouveau projet / Create new project
file -new -force;
workspace -create "C:/Projects/MyProject";
workspace -openWorkspace "C:/Projects/MyProject";
```

### Paramètres recommandés / Recommended Settings
- **Unités**: Centimètres (préféré pour game engines)
- **Frame Rate**: 30 fps (standard pour jeux) ou 24 fps (cinématique)
- **Up Axis**: Y-up (standard pour Unreal/Unity)
- **Linear Working Space**: sRGB or ACEScg

## 📐 Modélisation / Modeling

### Workflow de base / Basic Workflow
1. **Box Modeling**: Partir d'une primitive et subdiviser
2. **Edge Flow**: Maintenir un bon flow de topology
3. **Quad Topology**: Privilégier les quads pour les surfaces déformables
4. **Triangles**: OK pour les objets statiques/rigides

### Commandes utiles / Useful Commands
```mel
// Combiner des objets / Combine objects
polyUnite -n "CombinedMesh";

// Séparer par composants / Separate by components
polySeparate -n "Separated";

// Cleanup de mesh / Mesh cleanup
polyCleanupArgList 4 {
    "0","1","1","0","1","0","0","0","0","1e-05",
    "0","1e-05","0","1e-05","0","1","0","1"
};

// Export sélection / Export selection
file -force -options "groups=1;ptgroups=1;materials=1;smoothing=1;normals=1" 
     -type "FBX export" -pr -ea "C:/Export/model.fbx";
```

### Bonnes pratiques / Best Practices
- **Échelle**: Travailler à l'échelle réelle (1 unité = 1 cm)
- **Pivot**: Placer le pivot au sol et centré pour les props
- **Histoire**: Supprimer l'historique avant l'export (`Edit > Delete by Type > History`)
- **Freeze Transforms**: Geler les transformations (`Modify > Freeze Transformations`)
- **Nommage**: Utiliser des noms descriptifs sans espaces

## 🎨 Texturing et Shading / Texturing and Shading

### UV Mapping
```mel
// Auto UV unwrap
polyAutoProjection -lm 0 -pb 0 -ibd 1 -cm 0 -l 2 -sc 1 -o 1 -p 6;

// Layout UVs
polyMultiLayoutUV -lm 1 -sc 1 -rbf 0 -fr 1 -ps 0.2 -l 2;
```

### Export de textures / Texture Export
- **Format**: PNG (sans perte) ou TGA
- **Résolution**: Puissances de 2 (512, 1024, 2048, 4096)
- **Canaux**: RGB + Alpha si nécessaire
- **Naming**: `T_AssetName_TextureType_Resolution.png`

## 🎭 Animation et Rigging / Animation and Rigging

### Rigging basique / Basic Rigging
```mel
// Créer un joint / Create joint
joint -p 0 0 0;

// Orient les joints / Orient joints
joint -e -oj "xyz" -secondaryAxisOrient "yup" -ch -zso;

// Bind skin
skinCluster -tsb -mi 4 -dr 4;
```

### Animation
```mel
// Set keyframe
setKeyframe -v 10 -at "translateX" -t 1;

// Bake animation
bakeResults -simulation true -t "1:100" -sampleBy 1 
           -oversamplingRate 1 -disableImplicitControl true 
           -preserveOutsideKeys true -sparseAnimCurveBake false;
```

### Bonnes pratiques / Best Practices
- **Frame Range**: Définir clairement le range d'animation
- **Nomenclature**: `A_CharacterName_ActionName`
- **Root Motion**: Décider si root motion ou in-place
- **Export**: Exporter avec le skeleton pour les animations

## 📤 Export / Exporting

### FBX Export Settings
```
Geometry:
✓ Smoothing Groups
✓ Smooth Mesh
✓ Referenced Assets Content
□ Triangulate (laisser le moteur le faire)

Animation:
✓ Animation
✓ Bake Animation
□ Deformed Models (si pas de rig dans le jeu)
✓ Skins
✓ Blend Shapes
□ Curve Filters

Advanced Options:
✓ Convert Units Automatically
Axis Conversion: Y-up
FBX File Format: Binary
FBX Version: FBX 2020 (ou version compatible)
```

### Python Script d'export / Export Python Script
```python
import maya.cmds as cmds

def export_selected_fbx(file_path):
    """Exporte la sélection en FBX avec paramètres optimaux"""
    
    # Sélectionner l'objet
    selection = cmds.ls(selection=True)
    if not selection:
        cmds.warning("Aucun objet sélectionné")
        return False
    
    # Load FBX plugin
    cmds.loadPlugin('fbxmaya', quiet=True)
    
    # FBX export options
    mel.eval('FBXExportSmoothingGroups -v true')
    mel.eval('FBXExportHardEdges -v false')
    mel.eval('FBXExportTangents -v true')
    mel.eval('FBXExportSmoothMesh -v true')
    mel.eval('FBXExportInstances -v false')
    mel.eval('FBXExportInAscii -v false')
    
    # Export
    cmds.file(file_path, 
              force=True,
              options="v=0",
              type="FBX export",
              exportSelected=True)
    
    print(f"Exported to: {file_path}")
    return True
```

## 🔌 Scripts et Plugins / Scripts and Plugins

### Scripts utiles / Useful Scripts
- **Clean Mesh**: Supprime l'historique et freeze les transforms
- **Batch Export**: Exporte plusieurs objets en batch
- **Auto Rig**: Setup de rig basique automatisé
- **UV Checker**: Vérifie la qualité des UVs

### Plugins recommandés / Recommended Plugins
- **RizomUV Bridge**: Pour UV unwrapping avancé
- **Substance 3D for Maya**: Intégration Substance
- **Advanced Skeleton**: Rigging avancé
- **Turtle**: Baking de textures

## ⚡ Raccourcis clavier / Keyboard Shortcuts

### Navigation
- `Alt + Left Mouse`: Rotate view
- `Alt + Middle Mouse`: Pan view
- `Alt + Right Mouse` ou `Alt + Scroll`: Zoom
- `F`: Frame selected
- `A`: Frame all

### Sélection / Selection
- `F8`: Toggle object/component mode
- `F9`: Vertex mode
- `F10`: Edge mode
- `F11`: Face mode
- `F12`: UV mode

### Outils / Tools
- `Q`: Select tool
- `W`: Move tool
- `E`: Rotate tool
- `R`: Scale tool
- `G`: Repeat last command
- `Shift + D`: Duplicate
- `Ctrl + D`: Duplicate with transform

### Affichage / Display
- `4`: Wireframe
- `5`: Shaded
- `6`: Shaded with textures
- `7`: Lighting (all lights)

## 💡 Tips et Astuces / Tips and Tricks

### Performance
- Désactiver viewport 2.0 pour des scènes complexes
- Utiliser le mode "Bounding Box" pour les objets lourds
- Isoler la sélection (`Ctrl + 1`) pour travailler sur des objets spécifiques

### Workflow
- Utiliser les "Quick Selection Sets" pour grouper des objets
- Créer des "Display Layers" pour organiser la scène
- Utiliser l'Outliner avec "Show DAG Objects Only" activé
- Sauvegarder des incremental saves (`File > Save Scene As` + increment)

### Scripting
- Utiliser le "Script Editor" pour capturer les commandes MEL
- Echo All Commands pour voir les commandes exécutées
- Créer des shelf buttons pour les scripts fréquents

## 📚 Ressources / Resources

### Documentation officielle / Official Documentation
- [Maya User Guide](https://help.autodesk.com/view/MAYAUL/ENU/)
- [Maya Python API](https://help.autodesk.com/view/MAYAUL/ENU/?guid=__CommandsPython_index_html)
- [Maya MEL Reference](https://help.autodesk.com/view/MAYAUL/ENU/?guid=__Commands_index_html)

### Tutoriels / Tutorials
- Autodesk Maya Learning Channel (YouTube)
- Pluralsight Maya courses
- Digital Tutors Maya fundamentals
- CGCookie Maya tutorials

### Communauté / Community
- [Creative Crash](https://www.creativecrash.com/maya)
- [CGSociety Maya Forums](https://forums.cgsociety.org/c/autodesk/maya/)
- [Polycount Forums](https://polycount.com/categories/technical-talk)

## 📌 Notes
Toujours vérifier la compatibilité des versions Maya avec les game engines utilisés.
Unreal et Unity peuvent avoir des exigences spécifiques pour l'import FBX.

Always check Maya version compatibility with the game engines used.
Unreal and Unity may have specific requirements for FBX imports.
