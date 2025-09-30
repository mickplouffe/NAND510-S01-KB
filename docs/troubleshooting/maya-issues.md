# Maya - Résolution de problèmes / Maya Troubleshooting

## 🔧 Problèmes d'ouverture et de fichiers / File Opening Issues

### Maya ne s'ouvre pas / Maya Won't Start
**Solutions:**
1. Supprimer le dossier `preferences`:
   - Windows: `Documents\maya\<version>\prefs`
   - Mac: `~/Library/Preferences/Autodesk/maya/<version>`
2. Lancer en mode sans plugins: `maya -noAutoloadPlugins`
3. Vérifier les logs: `Documents\maya\<version>\scripts\userSetup.log`
4. Réinstaller Maya si nécessaire

### Fichier .ma/.mb ne s'ouvre pas / Scene File Won't Open
**Solutions:**
```mel
// Essayer d'ouvrir en mode de référence
file -reference -namespace "temp" "path/to/file.ma";

// Importer au lieu d'ouvrir
file -import -namespace "imported" "path/to/file.ma";

// Forcer l'ouverture sans plugins
file -open -ignoreVersion -force "path/to/file.ma";
```

## 🎨 Problèmes de viewport / Viewport Issues

### Viewport noir ou corrompu / Black or Corrupted Viewport
**Solutions:**
1. Changer le renderer du viewport:
   - Menu: Renderer > Viewport 2.0 / Legacy Viewport
2. Réinitialiser les settings:
   ```mel
   // Reset viewport
   setRendererInModelPanel "base_OpenGL_Renderer" modelPanel4;
   ```
3. Mettre à jour drivers GPU
4. Désactiver le multi-sampling dans les préférences

### Viewport lent / Slow Viewport
**Solutions:**
```mel
// Réduire la qualité du viewport
setAttr "hardwareRenderingGlobals.multiSampleEnable" 0;
setAttr "hardwareRenderingGlobals.lineAAEnable" 0;

// Désactiver textures en haute résolution
setAttr "hardwareRenderingGlobals.textureMaxResolution" 512;
```

**Paramètres à ajuster:**
- Lighting > Use Default Lighting
- Renderer > Wireframe on Shaded
- Display > NURBS Smoothness (réduire)

## 📐 Problèmes de modélisation / Modeling Issues

### "Non-manifold geometry" erreur
**Cause:** Edges partagés par plus de 2 faces, ou vertices libres

**Solutions:**
```mel
// Cleanup automatique
polyCleanupArgList 4 {
    "0","2","1","0","0","0","0","0","0","1e-05",
    "0","1e-05","0","1e-05","0","-1","0","0"
};

// Trouver les non-manifold edges
select -r `polyInfo -nme`;

// Trouver les non-manifold vertices
select -r `polyInfo -nmv`;
```

### Mesh a des trous / Mesh Has Holes
**Solutions:**
```mel
// Sélectionner les border edges
select -r `polyListComponentConversion -te -bo`;

// Fill holes
polyCloseBorder;

// Ou manuellement avec Fill Hole
// Edit Mesh > Fill Hole
```

### Historique cause des problèmes / History Causing Issues
**Symptômes:** Déformations, lag, comportement bizarre

**Solutions:**
```mel
// Supprimer historique sur objet sélectionné
delete -ch;

// Supprimer historique sur tout
delete -all -ch;

// Script pour auto-delete history
global proc autoDeleteHistory()
{
    scriptJob -event "SelectionChanged" "catch(delete -ch)";
}
```

## 🎭 Problèmes d'animation / Animation Issues

### Animation saccadée / Choppy Animation
**Solutions:**
1. Vérifier le frame rate: `window -e -wh 200 100 playbackOptions;`
2. Changer playback speed à "Play every frame"
3. Désactiver High Quality Rendering pendant playback
4. Réduire la complexité de la scène

### Keyframes ne fonctionnent pas / Keyframes Not Working
**Solutions:**
```mel
// Vérifier si l'attribut est keyable
setAttr -keyable true "pSphere1.translateX";

// S'assurer que l'attribut n'est pas locked
setAttr -lock false "pSphere1.translateX";

// Set keyframe manuellement
setKeyframe -attribute translateX -time 1 "pSphere1";
```

### Rig se casse / Rig Breaks
**Solutions:**
1. Vérifier que tous les joints ont l'orientation correcte
2. Reset bind pose: `dagPose -restore bindPose1;`
3. Rebind skin si nécessaire:
   ```mel
   // Détacher skin
   DetachSkin;
   
   // Rattacher
   select joint1 joint2 mesh;
   skinCluster;
   ```

## 📤 Problèmes d'export / Export Issues

### Export FBX échoue / FBX Export Fails
**Solutions:**
1. Load FBX plugin:
   ```mel
   loadPlugin "fbxmaya";
   ```

2. Vérifier et corriger la géométrie:
   ```mel
   // Delete history
   delete -ch;
   
   // Freeze transformations
   makeIdentity -apply true -t 1 -r 1 -s 1;
   
   // Center pivot
   xform -cp;
   ```

3. Export settings recommandés:
   - Smoothing Groups: ON
   - Triangulate: OFF (laisser le moteur le faire)
   - Animation: Bake Animation si animation
   - Convert Units: Automatiquement

### Textures ne s'exportent pas / Textures Don't Export
**Solutions:**
```mel
// Assigner les chemins de texture correctement
string $fileNodes[] = `ls -type "file"`;
for ($node in $fileNodes)
{
    string $path = `getAttr ($node + ".fileTextureName")`;
    print ("Texture: " + $path + "\n");
}

// Copier textures dans le dossier d'export
// Utiliser "Copy Textures" dans FBX export options
```

### Échelle incorrecte après export / Wrong Scale After Export
**Solutions:**
```mel
// Vérifier les unités du projet
currentUnit -q -linear;  // Devrait être "cm" pour games

// Changer si nécessaire
currentUnit -linear "cm";

// Freeze transforms avant export
select -all;
makeIdentity -apply true -t 1 -r 1 -s 1;

// Settings FBX:
// File Units: Automatic
// Scale Factor: 1.0
```

## 🔌 Problèmes de plugins / Plugin Issues

### Plugin ne charge pas / Plugin Won't Load
**Solutions:**
1. Vérifier compatibilité version Maya
2. Load manuellement:
   ```mel
   loadPlugin "pluginName";
   ```
3. Vérifier dans Window > Settings/Preferences > Plug-in Manager
4. Réinstaller le plugin si nécessaire

### Mental Ray / Arnold manquant / Mental Ray / Arnold Missing
**Note:** Mental Ray est deprecated dans les versions récentes

**Solutions:**
1. Installer Arnold séparément (gratuit pour Maya)
2. Utiliser un autre renderer (Arnold, V-Ray, Redshift)
3. Vérifier les plugins chargés

## 💻 Problèmes de script / Scripting Issues

### Script Editor ne fonctionne pas / Script Editor Not Working
**Solutions:**
```mel
// Réinitialiser Script Editor
deleteUI scriptEditorPanel1Window;

// Rouvrir
ScriptEditor;
```

### MEL Script erreur / MEL Script Error
**Debugging:**
```mel
// Activer echo all commands
scriptEditorInfo -e -suppressErrors false -suppressWarnings false;

// Tracer variables
print ("Variable value: " + $variableName + "\n");

// Try-catch
catch ($result = `command`);
if ($result == 0)
{
    print "Command failed\n";
}
```

### Python Script erreur / Python Script Error
**Debugging:**
```python
import maya.cmds as cmds
import traceback

try:
    # Votre code ici
    cmds.polySphere()
except Exception as e:
    print("Error occurred:")
    print(traceback.format_exc())
```

## 🎬 Problèmes de rendu / Rendering Issues

### Arnold ne rend pas / Arnold Doesn't Render
**Solutions:**
1. Vérifier qu'Arnold est chargé dans Plugin Manager
2. S'assurer que la scène a des lights
3. Vérifier Camera > Renderable
4. Réinitialiser Arnold settings:
   - Render Settings > Restore Default Settings

### Textures pixélisées dans le rendu / Pixelated Textures in Render
**Solutions:**
1. Vérifier la résolution des textures
2. Filter Type dans file node: Quadratic
3. Arnold Texture settings: Auto mipmap

## 📋 Maintenance préventive / Preventive Maintenance

### Nettoyage régulier / Regular Cleanup
```mel
// Script de nettoyage complet
global proc cleanupScene()
{
    // Supprimer historique
    delete -all -ch;
    
    // Supprimer nodes inutilisés
    MLdeleteUnused;
    
    // Optimize scene
    cleanUpScene 3;
    
    // Supprimer layers vides
    string $layers[] = `ls -type "displayLayer"`;
    for ($layer in $layers)
    {
        if (`size(listConnections($layer))` == 0)
            delete $layer;
    }
    
    print "Scene cleaned up!\n";
}
```

### Optimisation / Optimization
```mel
// Désactiver auto-keyframe si pas nécessaire
autoKeyframe -state off;

// Limiter undo queue
undoInfo -state on -infinity off -length 50;

// Désactiver viewport shading si pas nécessaire
modelEditor -e -displayTextures off -displayLights "default" modelPanel4;
```

## 📚 Ressources / Resources

### Logs et fichiers de diagnostic / Logs and Diagnostic Files
- Output Window: `Window > General Editors > Script Editor`
- Log files: `Documents\maya\<version>\scripts\`
- Crash logs: `Documents\maya\<version>\`

### Commandes utiles / Useful Commands
```mel
// Info système
about -version;
about -operatingSystem;

// Lister tous les plugins
pluginInfo -q -listPlugins;

// Vérifier scène
file -q -sceneName;
file -q -modified;
```

### Support
- [Maya Knowledge Base](https://knowledge.autodesk.com/support/maya)
- [Area Forums](https://forums.autodesk.com/t5/maya-forum/bd-p/area-maya)
- [Maya Help Documentation](https://help.autodesk.com/view/MAYAUL/ENU/)
