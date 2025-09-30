# Blender

## 📝 Vue d'ensemble / Overview
Documentation et ressources pour Blender dans le contexte du cours NAND510.

Documentation and resources for Blender in the context of the NAND510 course.

## 🚀 Démarrage rapide / Quick Start

### Configuration initiale / Initial Setup
- **Unités**: Metric, Scale 1.0 (1 unit = 1 meter)
- **Up Axis**: Z-up (défaut Blender)
- **Frame Rate**: 30 fps (jeux) ou 24 fps (cinématique)
- **Color Management**: Filmic ou AgX

### Paramètres recommandés / Recommended Settings
```
Edit > Preferences:
- Input > Emulate 3 Button Mouse (pour trackpads)
- System > GPU pour Cycles
- Save & Load > Auto Save (activer)
- Add-ons > Enable: Node Wrangler, FBX, glTF
```

## 📐 Modélisation / Modeling

### Workflow de base / Basic Workflow
1. **Subdivision Modeling**: Partir d'un cube avec modifier Subdivision Surface
2. **Edge Loops**: Utiliser Ctrl+R pour ajouter des edge loops
3. **Topology**: Maintenir des quads pour les surfaces déformables
4. **Modifiers**: Utiliser les modifiers de manière non-destructive

### Raccourcis essentiels / Essential Shortcuts
```
Navigation:
Middle Mouse: Rotate view
Shift + Middle Mouse: Pan view
Scroll: Zoom
Numpad .: Frame selected

Modeling:
Tab: Edit/Object mode
1, 2, 3: Vertex/Edge/Face mode
E: Extrude
I: Inset
Ctrl + R: Loop Cut
G: Move (Grab)
R: Rotate
S: Scale
X: Delete menu
Ctrl + B: Bevel

Selection:
A: Select All/Deselect
Alt + A: Deselect All
B: Box Select
C: Circle Select
```

### Bonnes pratiques / Best Practices
- **Échelle**: Appliquer les scales (Ctrl+A) avant export
- **Origin**: Placer l'origin correctement (Set Origin > Geometry to Origin)
- **Shading**: Auto Smooth pour un bon rendu
- **Nommage**: Collections et objets bien nommés
- **Non-destructive**: Utiliser les modifiers quand possible

## 🎨 Texturing et Materials / Texturing and Materials

### UV Unwrapping
```
Mode Edit (Tab):
U: Unwrap menu
  - Smart UV Project (rapide)
  - Unwrap (avec seams)
  - Cube/Sphere/Cylinder Projection

Ctrl + E: Edge menu
  - Mark Seam
  - Clear Seam
```

### Shader Editor
```
Shift + A: Add node
Ctrl + Shift + Left Click: Preview node output
Ctrl + T: Texture Setup (Node Wrangler addon)
```

### Export de textures / Texture Export
```python
import bpy

# Bake texture
bpy.ops.object.bake(
    type='COMBINED',
    save_mode='EXTERNAL',
    filepath='//textures/baked_texture.png',
    width=2048,
    height=2048
)
```

## 🎭 Animation et Rigging / Animation and Rigging

### Rigging basique / Basic Rigging
```
Armature (Shift + A > Armature):
- Add > Single Bone

Edit Mode (Tab):
E: Extrude bone
Alt + P: Clear Parent
Ctrl + P: Make Parent

Object Mode:
Ctrl + P > Armature Deform > Automatic Weights
```

### Animation
```
I: Insert Keyframe
  - Location
  - Rotation
  - Scale
  - LocRotScale

Timeline:
Space: Play/Pause
Left/Right Arrow: Previous/Next frame
Shift + Left/Right: Jump to keyframe
```

### Actions et NLA
- Créer des actions séparées pour chaque animation
- Utiliser le NLA Editor pour combiner les animations
- Nommer les actions clairement: `CharacterName.ActionName`

## 📤 Export / Exporting

### FBX Export Settings
```
File > Export > FBX (.fbx)

Include:
☑ Selected Objects (ou All si nécessaire)
☑ Custom Properties
☑ Apply Modifiers

Transform:
Scale: 1.0
☑ Apply Unit
☑ Apply Transform
Forward: -Z Forward
Up: Y Up

Geometry:
☑ Apply Modifiers
☑ Smoothing: Face
☑ Loose Edges
☑ Tangent Space

Armature:
☑ Add Leaf Bones
Primary Bone Axis: Y
Secondary Bone Axis: X

Animation:
☑ Baked Animation (si animation)
Simplify: 0.0
```

### glTF Export (Alternative moderne)
```
File > Export > glTF 2.0 (.glb/.gltf)

Include:
☑ Selected Objects
☑ Custom Properties
☑ Cameras/Punctual Lights (si besoin)

Transform:
+Y Up

Geometry:
☑ Apply Modifiers
☑ UVs
☑ Normals
☑ Tangents
Materials: Export

Animation:
☑ Animation
☑ Shape Keys
```

### Python Script d'export / Export Python Script
```python
import bpy
import os

def export_selected_fbx(filepath, scale=1.0):
    """Exporte les objets sélectionnés en FBX"""
    
    # Vérifier la sélection
    if not bpy.context.selected_objects:
        print("Aucun objet sélectionné")
        return False
    
    # Settings FBX
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True,
        global_scale=scale,
        apply_unit_scale=True,
        apply_scale_options='FBX_SCALE_ALL',
        axis_forward='-Z',
        axis_up='Y',
        bake_space_transform=False,
        object_types={'MESH', 'ARMATURE'},
        use_mesh_modifiers=True,
        mesh_smooth_type='FACE',
        add_leaf_bones=True,
        primary_bone_axis='Y',
        secondary_bone_axis='X',
        armature_nodetype='NULL',
        bake_anim=True,
        bake_anim_use_all_actions=False,
        bake_anim_simplify_factor=0.0
    )
    
    print(f"Exporté vers: {filepath}")
    return True

# Utilisation:
# export_selected_fbx("C:/Export/model.fbx", scale=1.0)
```

## 🔌 Add-ons utiles / Useful Add-ons

### Inclus avec Blender / Included with Blender
- **Node Wrangler**: Améliore le workflow des nodes
- **Bool Tool**: Opérations booléennes rapides
- **LoopTools**: Outils pour edge loops
- **F2**: Création rapide de faces

### Externes / External
- **Hard Ops / Boxcutter**: Hard surface modeling
- **Substance 3D for Blender**: Intégration Substance
- **Rigify**: Auto-rigging avancé
- **Auto-Rig Pro**: Rigging automatique professionnel
- **MACHIN3tools**: Collection d'outils de modélisation

### Installation d'add-ons / Add-on Installation
```
Edit > Preferences > Add-ons
- Install (bouton en haut)
- Sélectionner le fichier .zip
- Activer l'add-on dans la liste
```

## 💡 Tips et Astuces / Tips and Tricks

### Performance
- Utiliser Viewport Display > Bounds pour objets complexes
- Simplifier la géométrie avec Decimate modifier
- Limiter les Subdivision Surface levels dans le viewport
- Utiliser les Collections pour organiser et cacher des objets

### Workflow
```
Numpad shortcuts:
1: Front view
3: Right view
7: Top view
0: Camera view
Ctrl + Numpad: Opposite views

Organisation:
M: Move to collection
H: Hide selected
Alt + H: Unhide all
Shift + H: Hide unselected (isolate)
```

### Scripting Python
```python
# Accéder aux objets sélectionnés
selected = bpy.context.selected_objects

# Accéder à l'objet actif
active = bpy.context.active_object

# Créer un nouveau mesh
bpy.ops.mesh.primitive_cube_add(location=(0, 0, 0))

# Modifier les propriétés
obj = bpy.data.objects['Cube']
obj.location = (1, 2, 3)
obj.scale = (2, 2, 2)

# Appliquer les transformations
bpy.ops.object.transform_apply(
    location=False,
    rotation=False,
    scale=True
)
```

## 🎯 Pipeline Unreal Engine

### Préparation pour UE5
```
1. Échelle: 1 unité Blender = 100 unités UE5
   - Export Scale: 100 dans FBX settings
   
2. Axes: Blender (Z-up) → UE5 (Z-up)
   - Forward: -Z Forward
   - Up: Y Up
   
3. Materials: Utiliser des noms standards
   - M_MaterialName
   
4. Textures: Exporter séparément
   - BaseColor, Normal, ORM (Occlusion/Roughness/Metallic)
```

### Script export pour UE5 / UE5 Export Script
```python
def export_for_unreal(filepath):
    """Export optimisé pour Unreal Engine"""
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True,
        global_scale=100.0,  # Conversion UE5
        apply_unit_scale=True,
        apply_scale_options='FBX_SCALE_ALL',
        axis_forward='-Z',
        axis_up='Y',
        use_mesh_modifiers=True,
        mesh_smooth_type='FACE',
        use_tspace=True,  # Tangent space
        add_leaf_bones=False,
        primary_bone_axis='Y',
        secondary_bone_axis='X',
        bake_anim=True
    )
```

## 🎯 Pipeline Unity

### Préparation pour Unity
```
1. Échelle: Garder 1:1
   - Export Scale: 1.0
   
2. Axes: Blender (Z-up) → Unity (Y-up)
   - Forward: -Z Forward
   - Up: Y Up
   
3. FBX version: 2020 ou plus récent
```

## 📚 Ressources / Resources

### Documentation officielle / Official Documentation
- [Blender Manual](https://docs.blender.org/manual/en/latest/)
- [Blender Python API](https://docs.blender.org/api/current/)
- [Blender Cloud Tutorials](https://cloud.blender.org/welcome)

### Tutoriels / Tutorials
- [Blender Guru](https://www.youtube.com/user/AndrewPPrice) - YouTube
- [CG Cookie](https://cgcookie.com/) - Comprehensive courses
- [Grant Abbitt](https://www.youtube.com/user/mediagabbitt) - Beginner friendly
- [Polygon Runway](https://www.youtube.com/c/PolygonRunway) - Game asset focused

### Communauté / Community
- [Blender Artists](https://blenderartists.org/)
- [r/blender](https://www.reddit.com/r/blender/)
- [Blender Stack Exchange](https://blender.stackexchange.com/)
- [BlenderNation](https://www.blendernation.com/)

## 📌 Notes importantes / Important Notes

### Différences Maya vs Blender / Maya vs Blender Differences
- **Axes**: Maya (Y-up) vs Blender (Z-up)
- **Échelle**: Maya (cm) vs Blender (m)
- **Sélection**: Maya (click) vs Blender (left-click)
- **Navigation**: Similaire avec Middle Mouse

### Compatibilité / Compatibility
- Blender → UE5: Excellent support FBX/glTF
- Blender → Unity: Excellent support FBX, aussi .blend direct
- Toujours tester l'import dans le moteur cible
- Garder une version de travail .blend séparée des exports

### Version Control
- Fichiers .blend: Binaires, difficiles à merger
- Sauvegarder régulièrement des incréments
- Exporter les assets en formats texte (FBX ASCII) si possible
- Documenter les changements importants
