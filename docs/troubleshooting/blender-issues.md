# Blender - Résolution de problèmes / Blender Troubleshooting

## 🔧 Problèmes d'ouverture / Startup Issues

### Blender ne démarre pas / Blender Won't Start
**Solutions:**
1. Supprimer le dossier config: `%APPDATA%\Blender Foundation\Blender\<version>\config`
2. Lancer avec factory settings: `blender --factory-startup`
3. Vérifier compatibilité GPU et drivers
4. Essayer version portable

### Fichier .blend corrompu / Corrupted .blend File
**Solutions:**
```python
# Essayer de récupérer avec script
import bpy
bpy.ops.wm.recover_auto_save()

# Ou depuis command line
blender --recover-last

# Chercher les backups auto
# Windows: %TEMP%\
# Linux: /tmp/
# Mac: /tmp/
```

## 🎨 Problèmes de viewport / Viewport Issues

### Viewport noir / Black Viewport
**Solutions:**
1. Edit > Preferences > System > GPU Backend: Change to OpenGL/Vulkan/Metal
2. Vérifier que Cycles/Eevee est bien configuré
3. Reset viewport shading (Z key)
4. Désactiver Overlay qui pourrait causer problème

### Performance viewport lente / Slow Viewport
**Solutions:**
```python
import bpy

# Réduire samples Eevee
bpy.context.scene.eevee.taa_render_samples = 64
bpy.context.scene.eevee.taa_samples = 16

# Désactiver features coûteuses
bpy.context.scene.eevee.use_gtao = False
bpy.context.scene.eevee.use_bloom = False
bpy.context.scene.eevee.use_ssr = False
```

**Astuces:**
- Utiliser Solid shading pendant modeling (Z key)
- Réduire Subdivision Surface levels dans viewport
- Utiliser Simplify dans Render Properties
- Cacher collections non utilisées

## 📐 Problèmes de modélisation / Modeling Issues

### Mesh apparaît noir / Mesh Appears Black
**Causes communes:**
- Normales inversées
- Pas de matériau
- Pas de lumières

**Solutions:**
```python
import bpy

# Recalculer normales extérieures
bpy.ops.mesh.select_all(action='SELECT')
bpy.ops.mesh.normals_make_consistent(inside=False)

# Ajouter matériau basique
obj = bpy.context.active_object
mat = bpy.data.materials.new(name="Material")
obj.data.materials.append(mat)
```

### Modifier ne fonctionne pas / Modifier Not Working
**Solutions:**
1. Vérifier l'ordre des modifiers (haut en bas)
2. Apply Scale (Ctrl+A) avant d'ajouter modifiers
3. Vérifier que l'objet n'a pas de transformations non appliquées
4. Essayer d'appliquer les modifiers précédents

### Boolean operation échoue / Boolean Fails
**Solutions:**
```python
# Utiliser Fast solver
bpy.context.object.modifiers["Boolean"].solver = 'FAST'

# Ou Exact solver avec overlap threshold
bpy.context.object.modifiers["Boolean"].solver = 'EXACT'
bpy.context.object.modifiers["Boolean"].double_threshold = 0.0001
```

## 🎭 Problèmes d'animation / Animation Issues

### Animation ne joue pas / Animation Won't Play
**Solutions:**
1. Vérifier que le bon workspace est sélectionné (Animation)
2. Frame range correct dans Timeline
3. Vérifier que l'objet a des keyframes: Info Editor > Statistics
4. Auto Keying n'est pas activé par erreur

### Armature ne déforme pas le mesh / Armature Doesn't Deform Mesh
**Solutions:**
```python
import bpy

obj = bpy.context.object
armature = bpy.data.objects["Armature"]

# Re-parent avec automatic weights
bpy.ops.object.select_all(action='DESELECT')
obj.select_set(True)
armature.select_set(True)
bpy.context.view_layer.objects.active = armature

bpy.ops.object.parent_set(type='ARMATURE_AUTO')
```

**Vérifications:**
- Le mesh a un Armature modifier
- L'armature est en Pose Mode, pas Edit Mode
- Bones ont du poids (Weight Paint mode)

## 📤 Problèmes d'export / Export Issues

### Export FBX ne fonctionne pas / FBX Export Not Working
**Solutions:**
```python
import bpy

# Settings recommandés pour game engines
bpy.ops.export_scene.fbx(
    filepath="output.fbx",
    use_selection=True,
    global_scale=1.0,
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
    bake_anim=True
)
```

**Checklist avant export:**
```
☑ Apply Scale (Ctrl+A > Scale)
☑ Apply Rotation si nécessaire
☑ Supprimer doubles vertices
☑ Recalculer normales
☑ Vérifier qu'il n'y a pas de N-gons
☑ Nommer correctement tous les objets
```

### Textures manquantes après export / Missing Textures After Export
**Solutions:**
1. Pack textures dans le .blend: File > External Data > Pack Resources
2. Copier textures manuellement dans dossier export
3. Utiliser chemins relatifs: File > External Data > Make All Paths Relative
4. Exporter textures séparément avec nodes Bake

### Échelle incorrecte / Wrong Scale
**Solutions:**
```python
import bpy

# Pour Unreal Engine (100x scale)
bpy.ops.export_scene.fbx(
    filepath="output.fbx",
    global_scale=100.0,
    apply_scale_options='FBX_SCALE_ALL'
)

# Pour Unity (1x scale)
bpy.ops.export_scene.fbx(
    filepath="output.fbx",
    global_scale=1.0,
    apply_scale_options='FBX_SCALE_ALL'
)

# Toujours appliquer scale avant export
bpy.ops.object.transform_apply(scale=True)
```

## 🔌 Problèmes d'add-ons / Add-on Issues

### Add-on ne charge pas / Add-on Won't Load
**Solutions:**
1. Edit > Preferences > Add-ons > Refresh list
2. Vérifier compatibilité version Blender
3. Installer depuis fichier .zip directement
4. Vérifier console pour erreurs Python: Window > Toggle System Console

### Add-on cause des crashes / Add-on Crashes Blender
**Solutions:**
```
1. Désactiver l'add-on
2. Redémarrer Blender en safe mode:
   blender --factory-startup
3. Contacter développeur de l'add-on
4. Chercher version mise à jour
```

## 💻 Problèmes de scripting / Scripting Issues

### Script Python erreur / Python Script Error
**Debugging:**
```python
import bpy
import traceback

try:
    # Votre code
    bpy.ops.mesh.primitive_cube_add()
except Exception as e:
    print("Error:")
    print(traceback.format_exc())
```

**Console Python:**
- Window > Toggle System Console (Windows)
- Terminal (Linux/Mac) - lancer Blender depuis terminal
- Voir erreurs complètes avec stack trace

### Addon development tips
```python
# Recharger module pendant développement
import importlib
import my_addon
importlib.reload(my_addon)

# Print debug info
def debug_print(obj):
    import sys
    print(str(obj), file=sys.stderr)

# Check Blender version
if bpy.app.version >= (3, 0, 0):
    # Code for 3.0+
    pass
```

## 🎬 Problèmes de rendu / Rendering Issues

### Render est noir / Render Is Black
**Solutions:**
1. Ajouter une lumière (Shift+A > Light)
2. Vérifier que World a de l'émission
3. Camera est positionnée correctement
4. Objet n'est pas sur un layer caché

### Cycles render très lent / Cycles Very Slow
**Solutions:**
```python
import bpy

# Réduire samples
bpy.context.scene.cycles.samples = 128

# Activer denoising
bpy.context.scene.cycles.use_denoising = True

# Utiliser GPU si disponible
bpy.context.preferences.addons['cycles'].preferences.compute_device_type = 'CUDA'
bpy.context.scene.cycles.device = 'GPU'

# Tile size optimal
bpy.context.scene.cycles.tile_size = 256
```

### Eevee artifacts / glitches
**Solutions:**
1. Augmenter Shadow Cube Size
2. Augmenter Viewport Samples
3. Activer Ambient Occlusion
4. Ajuster Clip Start/End sur camera

## 📋 Maintenance / Maintenance

### Cleanup automatique / Automatic Cleanup
```python
import bpy

def cleanup_scene():
    # Purge unused data
    bpy.ops.outliner.orphans_purge(do_recursive=True)
    
    # Remove unused materials
    for mat in bpy.data.materials:
        if not mat.users:
            bpy.data.materials.remove(mat)
    
    # Remove unused images
    for img in bpy.data.images:
        if not img.users:
            bpy.data.images.remove(img)
    
    print("Scene cleaned!")

cleanup_scene()
```

### Optimisation fichier / File Optimization
```python
# Compresser fichier .blend
# File > Save As > Enable Compress

# Pack external data
bpy.ops.file.pack_all()

# Purge unused data
bpy.ops.outliner.orphans_purge(do_recursive=True)
```

## 🔍 Diagnostic / Diagnostics

### Info système / System Info
```python
import bpy

# Version Blender
print(bpy.app.version_string)

# GPU info
print(bpy.context.preferences.system.gpu_backend)

# Memory usage
import sys
print(f"Memory: {sys.getsizeof(bpy.data) / 1024 / 1024:.2f} MB")
```

### Statistiques scène / Scene Statistics
```
Enable in Viewport Overlays:
- Statistics
- Shows poly count, vertices, objects
```

## 📚 Ressources / Resources

### Documentation
- [Blender Manual](https://docs.blender.org/manual/en/latest/)
- [Blender Python API](https://docs.blender.org/api/current/)
- [Blender Stack Exchange](https://blender.stackexchange.com/)

### Support
- [Official Forums](https://blender.community/)
- [DevTalk](https://devtalk.blender.org/)
- [Bug Tracker](https://developer.blender.org/)
- [r/blenderhelp](https://www.reddit.com/r/blenderhelp/)

### Logs
- System Console (Window > Toggle System Console)
- Preferences > Save & Load > Auto Save Location
- Crash logs: Look in temp folder for crash.txt
