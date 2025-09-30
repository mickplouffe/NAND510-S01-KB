# Unity - Résolution de problèmes / Unity Troubleshooting

## 🚀 Problèmes de démarrage / Startup Issues

### Unity ne démarre pas / Unity Won't Start
**Solutions:**
1. Lancer Unity Hub et vérifier version installée
2. Réinstaller Unity depuis Hub
3. Vérifier Visual Studio ou IDE est installé
4. Clear Editor Preferences: Delete `AppData/Local/Unity/Editor/` (Windows)
5. Run as administrator si problèmes de permissions

### Projet ne s'ouvre pas / Project Won't Open
**Solutions:**
```
1. Vérifier version Unity compatible
2. Supprimer folders: Library/, Temp/
3. Unity Hub > Projects > Add > Sélectionner dossier projet
4. Si upgrade nécessaire: Backup projet d'abord
5. Vérifier logs: Editor.log dans %LOCALAPPDATA%\Unity\Editor\
```

### "Unable to parse file" / Erreur parsing
**Solutions:**
1. Vérifier .meta files intacts
2. Reimport assets: Assets > Reimport All
3. Force text serialization: Edit > Project Settings > Editor > Asset Serialization > Force Text
4. Restore depuis version control si possible

## 🎨 Problèmes de scène / Scene Issues

### Scène noire / Black Scene
**Solutions:**
1. Vérifier qu'il y a une lumière: GameObject > Light > Directional Light
2. Camera Background > Skybox ou Solid Color
3. Window > Rendering > Lighting > Generate Lighting
4. Vérifier que Scene view est en Shaded mode

### Objects disparaissent / Objects Disappear
**Causes communes:**
- Layer mismatch avec Camera
- Culling Mask incorrect
- Object scale = 0
- Trop loin de camera (clipping)

**Solutions:**
```csharp
// Vérifier layer
Debug.Log(gameObject.layer);

// Vérifier camera culling mask
Camera.main.cullingMask = -1; // Show all layers

// Vérifier scale
Debug.Log(transform.localScale);
```

## 💻 Problèmes de script / Scripting Issues

### Script ne compile pas / Script Won't Compile
**Common Errors:**

**"Type or namespace not found":**
```csharp
// Manque using statement
using UnityEngine;
using UnityEngine.UI;      // Pour UI
using System.Collections;  // Pour IEnumerator
using TMPro;              // Pour TextMeshPro
```

**"Method must return a value":**
```csharp
// MAUVAIS
public int GetHealth()
{
    // Manque return!
}

// BON
public int GetHealth()
{
    return health;
}
```

**"Object reference not set":**
```csharp
// MAUVAIS - pas de null check
playerObject.GetComponent<Health>().TakeDamage(10);

// BON - avec validation
if (playerObject != null)
{
    Health health = playerObject.GetComponent<Health>();
    if (health != null)
    {
        health.TakeDamage(10);
    }
}
```

### Script attached ne s'exécute pas / Script Doesn't Run
**Checklist:**
```
☑ Script est attaché au GameObject
☑ GameObject est actif (coché dans Inspector)
☑ Script est enabled (coché)
☑ Pas d'erreur de compilation
☑ Méthode a bon nom (Start, Update, etc.)
☑ Script hérite de MonoBehaviour
```

### Performance script lente / Slow Script Performance
**Éviter dans Update():**
```csharp
// MAUVAIS - Très coûteux dans Update
void Update()
{
    GameObject player = GameObject.Find("Player");
    Camera cam = Camera.main;
    Transform[] all = FindObjectsOfType<Transform>();
}

// BON - Cache les références
private GameObject player;
private Camera mainCamera;

void Start()
{
    player = GameObject.FindGameObjectWithTag("Player");
    mainCamera = Camera.main;
}

void Update()
{
    // Utiliser les références cachées
}
```

## 🎮 Problèmes d'assets / Asset Issues

### Import d'asset échoue / Asset Import Fails
**FBX Import:**
```
Import Settings à vérifier:
- Scale Factor: 1 ou 100 (dépend DCC)
- Convert Units: Activer
- Bake Axis Conversion: Activer
- Import BlendShapes: Si nécessaire
- Import Visibility: Activer
- Import Cameras/Lights: Selon besoin
```

**Solutions si échec:**
1. Ré-exporter depuis DCC avec settings basiques
2. Essayer format alternatif (.obj pour test)
3. Vérifier console pour erreur spécifique
4. Test avec asset simple du même logiciel

### Textures apparaissent roses / Pink Textures
**Causes:**
- Shader manquant ou incompatible
- Texture path brisé
- Render pipeline mismatch

**Solutions:**
```
1. Edit > Render Pipeline > Universal > Upgrade Project Materials
2. Select material > Shader > Universal Render Pipeline > Lit
3. Reimport textures
4. Vérifier URP/HDRP asset assigned dans Project Settings
```

### Prefab broken / Prefab cassé
**Solutions:**
```
1. Select Prefab > Overrides dropdown > Revert All
2. Ou Apply All pour sauver changements
3. Si complètement cassé: Recréer prefab
   - Drag GameObject to Project window
```

## 🎬 Problèmes d'animation / Animation Issues

### Animation ne joue pas / Animation Won't Play
**Solutions:**
1. Vérifier Animator Component attaché
2. Animator has Controller assigned
3. Controller a des states et transitions
4. Parameters correctement configurés
5. Clip d'animation assigné au state

**Debug:**
```csharp
Animator animator = GetComponent<Animator>();
Debug.Log("Has Animator: " + (animator != null));
Debug.Log("Has Controller: " + (animator.runtimeAnimatorController != null));
Debug.Log("Is Playing: " + animator.GetCurrentAnimatorStateInfo(0).IsName("Walk"));
```

### Animator transitions ne fonctionnent pas / Transitions Don't Work
**Checklist:**
```
☑ Transition has condition
☑ Parameter existe et bon type
☑ Has Exit Time correct (ou désactivé)
☑ Transition Duration pas 0 si fade voulu
☑ Can Transition To Self si nécessaire
```

### Animation saccadée / Choppy Animation
**Solutions:**
1. Augmenter sample rate du clip
2. Animation Compression: Off pour test
3. Update Mode sur Animator: Normal ou Animate Physics
4. Vérifier frame rate du jeu

## 🔊 Problèmes audio / Audio Issues

### Son ne joue pas / Audio Won't Play
**Checklist:**
```csharp
AudioSource source = GetComponent<AudioSource>();

// Vérifications
Debug.Log("Audio Clip: " + source.clip != null);
Debug.Log("Is Playing: " + source.isPlaying);
Debug.Log("Volume: " + source.volume);
Debug.Log("Mute: " + source.mute);

// Play audio
source.Play();
```

**Solutions:**
1. Vérifier AudioListener dans scène (1 seul!)
2. Volume pas à 0
3. Audio Clip assigné
4. Format audio supporté (MP3, WAV, OGG)
5. Audio pas muted dans mixer

### Audio 3D ne fonctionne pas / 3D Audio Issues
**Solutions:**
```csharp
// Configurer 3D audio
AudioSource source = GetComponent<AudioSource>();
source.spatialBlend = 1.0f; // 1 = fully 3D
source.minDistance = 1f;
source.maxDistance = 500f;
source.rolloffMode = AudioRolloffMode.Linear;
```

## 🎯 Problèmes de physique / Physics Issues

### Collision ne fonctionne pas / Collision Not Working
**Requirements:**
```
☑ Au moins un objet a Rigidbody
☑ Les deux objets ont Collider
☑ Colliders ne sont pas Trigger (sauf si voulu)
☑ Layers peuvent collider (Physics settings)
☑ Rigidbody n'est pas Kinematic (sauf si voulu)
```

**Debug collision:**
```csharp
void OnCollisionEnter(Collision collision)
{
    Debug.Log("Collision avec: " + collision.gameObject.name);
}

void OnTriggerEnter(Collider other)
{
    Debug.Log("Trigger avec: " + other.name);
}
```

### Physique bizarre / Weird Physics
**Solutions:**
```
1. Vérifier scale est uniforme (1,1,1 si possible)
2. Mass appropriée (pas trop petit/grand)
3. Drag et Angular Drag raisonnables
4. Time.fixedDeltaTime à 0.02 (50 fps)
5. Physics timestep constant
```

**Fixed Update pour physique:**
```csharp
// BON - physique dans FixedUpdate
void FixedUpdate()
{
    rb.AddForce(Vector3.forward * speed);
}

// MAUVAIS - physique dans Update
void Update()
{
    rb.AddForce(Vector3.forward * speed); // Inconsistent!
}
```

## 📦 Problèmes de build / Build Issues

### Build échoue / Build Fails
**Solutions:**
```
1. Vérifier Console pour erreurs
2. File > Build Settings > Player Settings
   - Company Name, Product Name remplis
   - Bundle Identifier correct (mobile)
   - Target API level correct
3. Clear cache: Library/BuildCache folder
4. Rebuild: Build > Clean Build puis Build
```

### Build réussit mais crash / Build Crashes
**Debug:**
```
1. Build Settings > Development Build + Autoconnect Profiler
2. Script Debugging activer
3. Run build et check Console dans Editor
4. Vérifier Player.log:
   - Windows: %USERPROFILE%\AppData\LocalLow\CompanyName\ProductName\Player.log
   - Mac: ~/Library/Logs/Company Name/Product Name/Player.log
```

**Common causes:**
- Resources path incorrect
- Scene pas dans Build Settings
- Platform-specific code pas conditionnel
- Asset bundle missing

### Build size trop gros / Build Size Too Large
**Optimisations:**
```
1. Texture Compression: Enable
2. Audio Compression: Enable
3. Strip Engine Code: Activer
4. Code Stripping: IL2CPP + High
5. Remove unused assets
6. Use Asset Bundles pour content
```

## 🔧 Problèmes de performance / Performance Issues

### Low FPS
**Profiling:**
```
1. Window > Analysis > Profiler
2. Enable Deep Profile (warning: overhead)
3. Regarder CPU Usage, Rendering, Scripts
4. Frame Debugger pour draw calls
```

**Optimisations communes:**
```csharp
// Object Pooling
public class ObjectPool
{
    private Queue<GameObject> pool = new Queue<GameObject>();
    private GameObject prefab;
    private int poolSize = 20;
    
    public void Initialize()
    {
        for (int i = 0; i < poolSize; i++)
        {
            GameObject obj = Instantiate(prefab);
            obj.SetActive(false);
            pool.Enqueue(obj);
        }
    }
    
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

// Éviter Find dans Update
// Utiliser GetComponent une fois puis cache
// Utiliser Tags au lieu de name comparison
// Utiliser Layer Masks pour raycasts
```

### Memory issues / Problèmes mémoire
```
Profiler > Memory module

Solutions:
- Reduce texture sizes
- Use texture atlases
- Clear unused assets: Resources.UnloadUnusedAssets()
- Optimize meshes (vertex count)
- Use object pooling
```

## 🐛 Debugging tips / Astuces debugging

### Debug.Log variations
```csharp
Debug.Log("Info message");
Debug.LogWarning("Warning!");
Debug.LogError("Error occurred!");
Debug.LogException(exception);

// Context pour localiser dans hierarchy
Debug.Log("Message", gameObject);

// Conditional compilation
#if UNITY_EDITOR
    Debug.Log("Editor only");
#endif

#if DEVELOPMENT_BUILD
    Debug.Log("Development build only");
#endif
```

### Gizmos et Debug Draw
```csharp
// OnDrawGizmos pour toujours visible
void OnDrawGizmos()
{
    Gizmos.color = Color.red;
    Gizmos.DrawSphere(transform.position, 0.5f);
    Gizmos.DrawLine(start, end);
}

// OnDrawGizmosSelected pour quand sélectionné
void OnDrawGizmosSelected()
{
    Gizmos.color = Color.green;
    Gizmos.DrawWireSphere(transform.position, attackRange);
}

// Debug.DrawLine (visible dans Scene view)
void Update()
{
    Debug.DrawLine(start, end, Color.blue, 2.0f);
    Debug.DrawRay(origin, direction * 10, Color.red);
}
```

## 📋 Maintenance / Maintenance

### Project cleanup
```
Safe à supprimer:
- Library/ folder (sera régénéré)
- Temp/ folder
- obj/ folder
- *.csproj, *.sln files (régénérés)

Garder:
- Assets/ folder (votre contenu!)
- Packages/ folder (dependencies)
- ProjectSettings/ folder
```

### Reimport all assets
```
Assets > Reimport All

Ou supprimer Library/ folder et restart Unity
```

### Clear cache
```
Edit > Preferences > GI Cache > Clear Cache
Edit > Preferences > Asset Pipeline > Clear Cache
```

## 📚 Ressources / Resources

### Documentation
- [Unity Manual](https://docs.unity3d.com/Manual/)
- [Unity Scripting Reference](https://docs.unity3d.com/ScriptReference/)
- [Unity Answers](https://answers.unity.com/)

### Logs
- Editor: Console window (Ctrl+Shift+C clear)
- Player log locations:
  - Windows: `%USERPROFILE%\AppData\LocalLow\CompanyName\ProductName\Player.log`
  - Mac: `~/Library/Logs/Company Name/Product Name/Player.log`
  - Linux: `~/.config/unity3d/CompanyName/ProductName/Player.log`

### Support
- [Unity Forums](https://forum.unity.com/)
- [Unity Answers](https://answers.unity.com/)
- [r/Unity3D](https://www.reddit.com/r/Unity3D/)
- [Unity Discord](https://discord.com/invite/unity)

### Shortcuts utiles / Useful Shortcuts
```
F: Frame selected
W/E/R/T: Move/Rotate/Scale/Rect
V: Vertex snap
Ctrl+D: Duplicate
Ctrl+P: Play
Ctrl+Shift+P: Pause
Ctrl+Alt+P: Step frame
```
