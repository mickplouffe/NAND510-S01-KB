# Unity

## 📝 Vue d'ensemble / Overview
Documentation et ressources pour Unity dans le contexte du cours NAND510.

Documentation and resources for Unity in the context of the NAND510 course.

## 🚀 Démarrage rapide / Quick Start

### Création de projet / Project Creation
```
Unity Hub:
- New Project
- Template: 3D (URP ou Built-in)
- Version: LTS (Long Term Support) recommandée

Templates populaires:
- 3D Core: Standard, sans features extra
- 3D (URP): Universal Render Pipeline
- 3D (HDRP): High Definition Render Pipeline
- 2D: Pour jeux 2D
```

### Structure de projet / Project Structure
```
ProjectName/
├── Assets/           # Assets du jeu
│   ├── Scenes/
│   ├── Scripts/
│   ├── Prefabs/
│   ├── Materials/
│   ├── Models/
│   └── Textures/
├── Packages/         # Package dependencies
├── ProjectSettings/  # Configuration projet
├── Library/          # Cache (ne pas versionner)
└── Temp/            # Fichiers temporaires
```

## 📐 Assets et Organisation / Assets and Organization

### Nomenclature recommandée / Recommended Naming
```
Scenes:
Level01.unity
MainMenu.unity
TestScene.unity

Scripts:
PlayerController.cs
EnemyAI.cs
GameManager.cs

Prefabs:
Player_Prefab
Enemy_Zombie_Prefab
Projectile_Bullet_Prefab

Materials:
M_Stone_Granite
M_Metal_Brushed
M_Character_Skin

Textures:
T_Wall_Albedo
T_Wall_Normal
T_Wall_Roughness
```

### Dossiers spéciaux / Special Folders
```
Assets/
├── Editor/              # Scripts éditeur uniquement
├── Resources/           # Loadable at runtime via Resources.Load()
├── StreamingAssets/     # Fichiers copiés tels quels au build
├── Plugins/            # Plugins natifs
└── Standard Assets/    # Assets standards Unity
```

## 💻 C# Scripting

### Script de base / Basic Script
```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    // Variables publiques (visibles dans l'Inspector)
    public float moveSpeed = 5.0f;
    public int maxHealth = 100;
    
    // Variables privées sérialisées (visibles mais encapsulées)
    [SerializeField]
    private GameObject weaponPrefab;
    
    // Variables privées
    private int currentHealth;
    private bool isGrounded;
    
    // Propriétés
    public bool IsAlive => currentHealth > 0;
    
    // Awake: Appelé en premier (initialisation interne)
    void Awake()
    {
        currentHealth = maxHealth;
    }
    
    // Start: Appelé après Awake (initialisation avec autres objets)
    void Start()
    {
        Debug.Log("Player initialized");
    }
    
    // Update: Appelé chaque frame
    void Update()
    {
        HandleMovement();
    }
    
    // FixedUpdate: Appelé à intervalle fixe (physique)
    void FixedUpdate()
    {
        // Physique ici
    }
    
    // Fonction publique
    public void TakeDamage(int damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }
    
    // Fonction privée
    private void HandleMovement()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        
        Vector3 movement = new Vector3(horizontal, 0, vertical);
        transform.Translate(movement * moveSpeed * Time.deltaTime);
    }
    
    private void Die()
    {
        Debug.Log("Player died");
        Destroy(gameObject);
    }
}
```

### Coroutines
```csharp
// Démarrer une coroutine
StartCoroutine(FadeOut(2.0f));

// Définir une coroutine
IEnumerator FadeOut(float duration)
{
    float elapsed = 0;
    Color startColor = renderer.material.color;
    
    while (elapsed < duration)
    {
        elapsed += Time.deltaTime;
        float alpha = Mathf.Lerp(1, 0, elapsed / duration);
        
        Color newColor = startColor;
        newColor.a = alpha;
        renderer.material.color = newColor;
        
        yield return null; // Attendre la prochaine frame
    }
    
    Destroy(gameObject);
}

// Autres yield options:
yield return null;                              // Prochaine frame
yield return new WaitForSeconds(2.0f);         // Attendre X secondes
yield return new WaitForFixedUpdate();         // Prochain FixedUpdate
yield return StartCoroutine(OtherCoroutine()); // Attendre autre coroutine
```

### Events et Delegates
```csharp
// Définir un event
public event System.Action OnPlayerDeath;
public event System.Action<int> OnHealthChanged;

// Déclencher l'event
OnPlayerDeath?.Invoke();
OnHealthChanged?.Invoke(currentHealth);

// S'abonner à l'event (dans autre script)
player.OnPlayerDeath += HandlePlayerDeath;
player.OnHealthChanged += UpdateHealthUI;

// Se désabonner (important!)
void OnDestroy()
{
    if (player != null)
    {
        player.OnPlayerDeath -= HandlePlayerDeath;
        player.OnHealthChanged -= UpdateHealthUI;
    }
}
```

## 🎮 GameObject et Components

### Manipulation de GameObjects
```csharp
// Créer un GameObject
GameObject obj = new GameObject("MyObject");
GameObject clone = Instantiate(prefab, position, rotation);

// Trouver des GameObjects
GameObject player = GameObject.Find("Player");
GameObject enemy = GameObject.FindGameObjectWithTag("Enemy");
GameObject[] enemies = GameObject.FindGameObjectsWithTag("Enemy");

// Components
Rigidbody rb = GetComponent<Rigidbody>();
Collider col = GetComponentInChildren<Collider>();
PlayerController[] controllers = FindObjectsOfType<PlayerController>();

// Ajouter/Enlever components
gameObject.AddComponent<Rigidbody>();
Destroy(GetComponent<Collider>());

// Activer/Désactiver
gameObject.SetActive(false);
enabled = false; // Désactive seulement ce script
```

### Transform
```csharp
// Position
transform.position = new Vector3(0, 1, 0);
transform.Translate(Vector3.forward * speed * Time.deltaTime);

// Rotation
transform.rotation = Quaternion.Euler(0, 90, 0);
transform.Rotate(Vector3.up * rotationSpeed * Time.deltaTime);
transform.LookAt(target.position);

// Scale
transform.localScale = new Vector3(2, 2, 2);

// Hiérarchie
transform.SetParent(parentTransform);
Transform child = transform.GetChild(0);
int childCount = transform.childCount;
```

## 🎨 Materials et Shaders / Materials and Shaders

### Manipulation de materials
```csharp
// Accéder au material
Renderer renderer = GetComponent<Renderer>();
Material material = renderer.material; // Crée une instance

// Modifier les propriétés
material.color = Color.red;
material.SetFloat("_Metallic", 0.5f);
material.SetTexture("_MainTex", texture);

// Material partagé (modifie tous les objets avec ce material)
renderer.sharedMaterial.color = Color.blue;
```

### Shader Graph (URP/HDRP)
```
1. Create > Shader > PBR Graph ou Unlit Graph
2. Ouvrir le Shader Graph
3. Ajouter des nodes
4. Connecter au Master Stack
5. Sauvegarder
6. Créer un material utilisant ce shader
```

## 🎬 Animation

### Animator Controller
```
Structure:
- States: Idle, Walk, Run, Jump
- Transitions: Conditions entre états
- Parameters: Bool, Float, Int, Trigger
- Blend Trees: Blending entre animations
```

### Scripting Animation
```csharp
Animator animator = GetComponent<Animator>();

// Set parameters
animator.SetBool("isWalking", true);
animator.SetFloat("speed", currentSpeed);
animator.SetInteger("attackType", 2);
animator.SetTrigger("jump");

// Get info
AnimatorStateInfo state = animator.GetCurrentAnimatorStateInfo(0);
bool isJumping = state.IsName("Jump");
float normalizedTime = state.normalizedTime;

// Play animation directly
animator.Play("Attack", 0, 0); // Layer 0, time 0
```

## 🔊 Audio

### Audio Source
```csharp
AudioSource audioSource = GetComponent<AudioSource>();

// Jouer des sons
audioSource.Play();
audioSource.PlayOneShot(soundEffect);
audioSource.PlayScheduled(AudioSettings.dspTime + 2.0);

// Contrôle
audioSource.Pause();
audioSource.Stop();
audioSource.volume = 0.5f;
audioSource.pitch = 1.2f;

// Audio 3D
audioSource.spatialBlend = 1.0f; // 0=2D, 1=3D
audioSource.minDistance = 5f;
audioSource.maxDistance = 50f;
```

## 🎯 Physics

### Rigidbody
```csharp
Rigidbody rb = GetComponent<Rigidbody>();

// Forces
rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
rb.AddTorque(Vector3.up * torque);

// Velocity
rb.velocity = new Vector3(0, 10, 0);
rb.angularVelocity = Vector3.up;

// Properties
rb.mass = 10f;
rb.drag = 0.5f;
rb.useGravity = true;
rb.isKinematic = false;
rb.constraints = RigidbodyConstraints.FreezeRotation;
```

### Collisions
```csharp
// Collision (physique)
void OnCollisionEnter(Collision collision)
{
    Debug.Log("Collision avec: " + collision.gameObject.name);
    ContactPoint contact = collision.contacts[0];
    Vector3 impactPoint = contact.point;
}

// Trigger (pas de physique)
void OnTriggerEnter(Collider other)
{
    if (other.CompareTag("Pickup"))
    {
        Destroy(other.gameObject);
    }
}

// Raycast
RaycastHit hit;
if (Physics.Raycast(transform.position, Vector3.forward, out hit, 10f))
{
    Debug.Log("Hit: " + hit.collider.name);
}
```

## 🖼️ UI (Canvas)

### UI Scripting
```csharp
using UnityEngine.UI;
using TMPro; // TextMeshPro

public class UIManager : MonoBehaviour
{
    [SerializeField] private TextMeshProUGUI healthText;
    [SerializeField] private Image healthBar;
    [SerializeField] private Button startButton;
    
    void Start()
    {
        // Ajouter listener au bouton
        startButton.onClick.AddListener(OnStartButtonClicked);
    }
    
    public void UpdateHealth(int current, int max)
    {
        healthText.text = $"{current}/{max}";
        healthBar.fillAmount = (float)current / max;
    }
    
    private void OnStartButtonClicked()
    {
        Debug.Log("Start button clicked!");
    }
}
```

## 🔌 Packages utiles / Useful Packages

### Package Manager (Window > Package Manager)
```
Unity Registry:
- Cinemachine: Caméras avancées
- ProBuilder: Modélisation in-engine
- TextMesh Pro: Texte de qualité
- Input System: Nouveau système input
- Post Processing: Effets post-traitement

Assets Store:
- DOTween: Animation par code
- Rewired: Input management avancé
- Odin Inspector: Inspector amélioré
```

## 🚀 Build et Déploiement / Build and Deployment

### Build Settings
```
File > Build Settings:

1. Add Scenes: Ajouter toutes les scènes nécessaires
2. Platform: Sélectionner la plateforme cible
3. Player Settings:
   - Company Name
   - Product Name
   - Icon
   - Splash Screen

4. Build ou Build And Run
```

### Player Settings important
```
Resolution:
- Fullscreen Mode
- Default Resolution
- Resizable Window

Quality Settings:
- Quality Level pour chaque plateforme
- Anti-Aliasing
- Texture Quality
- Shadow Quality

Graphics:
- Render Pipeline Asset (URP/HDRP)
- Always Included Shaders
```

## 📊 Performance et Optimisation / Performance and Optimization

### Profiler (Window > Analysis > Profiler)
```
Sections importantes:
- CPU Usage: Temps script, rendering, physics
- GPU Usage: Draw calls, batches
- Memory: RAM utilisée
- Rendering: Stats de rendu détaillés

Console Commands:
Ctrl + Shift + C: Clear console
Right-click log: Stack trace options
```

### Optimisations communes / Common Optimizations
```csharp
// Object Pooling
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
        return GameObject.Instantiate(prefab);
    }
    
    public void Return(GameObject obj)
    {
        obj.SetActive(false);
        pool.Enqueue(obj);
    }
}

// Éviter Update quand possible
void OnBecameVisible() { enabled = true; }
void OnBecameInvisible() { enabled = false; }

// Cache des components
private Transform _transform;
void Awake() { _transform = transform; }

// Utiliser Tags au lieu de Find
GameObject player = GameObject.FindGameObjectWithTag("Player");
```

## 📚 Ressources / Resources

### Documentation officielle / Official Documentation
- [Unity Manual](https://docs.unity3d.com/Manual/)
- [Unity Scripting Reference](https://docs.unity3d.com/ScriptReference/)
- [Unity Learn](https://learn.unity.com/)

### Tutoriels / Tutorials
- [Brackeys (YouTube)](https://www.youtube.com/c/Brackeys)
- [Code Monkey (YouTube)](https://www.youtube.com/c/CodeMonkeyUnity)
- [Unity Learn Platform](https://learn.unity.com/)
- [Catlike Coding](https://catlikecoding.com/unity/tutorials/)

### Community
- [Unity Forums](https://forum.unity.com/)
- [r/Unity3D](https://www.reddit.com/r/Unity3D/)
- [Unity Answers](https://answers.unity.com/)
- [Unity Discord](https://discord.com/invite/unity)

## 💡 Tips et Astuces / Tips and Tricks

### Editor Shortcuts
```
F: Frame selected object
W/E/R/T: Move/Rotate/Scale/Rect tool
V: Vertex snapping
Ctrl + D: Duplicate
Ctrl + Shift + D: Duplicate sans selection
Alt + Shift + C: Add component
Ctrl + P: Play
Ctrl + Shift + P: Pause
Ctrl + Alt + P: Step frame
```

### Best Practices
- Utiliser Prefabs pour objets réutilisables
- Éviter Find/FindObjectOfType dans Update
- Utiliser ScriptableObjects pour data
- Documenter le code public
- Nommer clairement les variables
- Utiliser namespaces pour organisation

## 📌 Notes importantes / Important Notes

### Version Control (.gitignore)
```
/[Ll]ibrary/
/[Tt]emp/
/[Oo]bj/
/[Bb]uild/
/[Bb]uilds/
/[Ll]ogs/
/[Uu]ser[Ss]ettings/

*.csproj
*.sln
*.suo
*.user
*.unityproj

# Fichiers à versionner:
/Assets/
/Packages/
/ProjectSettings/
```

### URP vs Built-in vs HDRP
```
Built-in:
+ Compatible partout
+ Léger
- Moins de features modernes

URP (Universal):
+ Performant mobile + desktop
+ Shader Graph
+ Moderne
- Quelques limitations

HDRP (High Definition):
+ Graphismes AAA
+ Ray tracing
- Très demandant (desktop uniquement)
```

### Migration de projet
- Toujours sauvegarder avant migration
- Tester dans une copie du projet
- Vérifier la compatibilité des packages
- Recompiler les shaders après migration
