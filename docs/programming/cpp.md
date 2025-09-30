# C++

## 📝 Vue d'ensemble / Overview
Documentation C++ pour le développement de jeux dans le contexte du cours NAND510.

C++ documentation for game development in the context of the NAND510 course.

## 🚀 Configuration / Setup

### Environnements de développement / Development Environments
- **Visual Studio** (Windows) - Recommandé pour UE5
- **Visual Studio Code** (Cross-platform) - Léger et extensible
- **CLion** (Cross-platform) - JetBrains IDE puissant
- **Xcode** (macOS) - Pour développement iOS/macOS

### Compilateurs / Compilers
- **MSVC** (Microsoft Visual C++) - Windows
- **GCC** (GNU Compiler Collection) - Linux/macOS
- **Clang** - Cross-platform, utilisé par UE5

## 📚 Fondamentaux / Fundamentals

### Types de base / Basic Types
```cpp
// Entiers / Integers
int8_t smallInt = 127;              // -128 to 127
uint8_t unsignedByte = 255;         // 0 to 255
int16_t shortInt = 32767;           // -32,768 to 32,767
int32_t normalInt = 2147483647;     // Standard int
int64_t longInt = 9223372036854775807LL; // Large numbers

// Flottants / Floating Point
float decimal = 3.14f;              // 32-bit (suffixe 'f')
double preciseDecimal = 3.14159265; // 64-bit

// Booléen / Boolean
bool isActive = true;
bool isDisabled = false;

// Caractères / Characters
char letter = 'A';
wchar_t wideChar = L'Ω';

// String
std::string text = "Hello";
const char* cString = "World";
```

### Pointeurs et Références / Pointers and References
```cpp
// Pointeur
int value = 42;
int* ptr = &value;      // Pointeur vers value
*ptr = 100;            // Déréférence - modifie value

// Référence
int& ref = value;      // Référence à value
ref = 200;            // Modifie directement value

// Pointeur nul
int* nullPtr = nullptr; // C++11+
if (nullPtr == nullptr) {
    // Handle null
}

// Allocation dynamique / Dynamic Allocation
int* dynamicInt = new int(42);
delete dynamicInt;

int* array = new int[10];
delete[] array;

// Smart Pointers (C++11+) - PRÉFÉRER CECI
#include <memory>
std::unique_ptr<int> uniquePtr = std::make_unique<int>(42);
std::shared_ptr<int> sharedPtr = std::make_shared<int>(42);
// Pas besoin de delete!
```

## 🎮 Programmation orientée objet / Object-Oriented Programming

### Classes et Structures / Classes and Structures
```cpp
// Header file (.h)
#pragma once  // ou include guards

class Character {
private:
    float m_health;
    float m_maxHealth;
    std::string m_name;

protected:
    int m_level;
    
public:
    // Constructeur / Constructor
    Character(const std::string& name, float health);
    
    // Destructeur / Destructor
    ~Character();
    
    // Getters
    float GetHealth() const { return m_health; }
    const std::string& GetName() const { return m_name; }
    
    // Setters
    void SetHealth(float health);
    
    // Méthodes / Methods
    void TakeDamage(float damage);
    virtual void Attack() = 0; // Pure virtual (abstract)
    
    // Static
    static int GetCharacterCount();
    
private:
    void Die();
    static int s_characterCount;
};

// Source file (.cpp)
#include "Character.h"

int Character::s_characterCount = 0;

Character::Character(const std::string& name, float health)
    : m_name(name)
    , m_health(health)
    , m_maxHealth(health)
    , m_level(1)
{
    s_characterCount++;
}

Character::~Character() {
    s_characterCount--;
}

void Character::TakeDamage(float damage) {
    m_health -= damage;
    if (m_health <= 0) {
        Die();
    }
}

void Character::SetHealth(float health) {
    m_health = std::clamp(health, 0.0f, m_maxHealth);
}
```

### Héritage / Inheritance
```cpp
class Player : public Character {
private:
    int m_experience;
    
public:
    Player(const std::string& name, float health)
        : Character(name, health)
        , m_experience(0)
    {}
    
    // Override de méthode virtuelle / Virtual method override
    void Attack() override {
        // Player-specific attack
    }
    
    void GainExperience(int amount) {
        m_experience += amount;
        if (m_experience >= 100) {
            LevelUp();
        }
    }
    
private:
    void LevelUp();
};
```

### Polymorphisme / Polymorphism
```cpp
// Utilisation polymorphe / Polymorphic usage
std::vector<std::unique_ptr<Character>> characters;
characters.push_back(std::make_unique<Player>("Hero", 100.0f));
characters.push_back(std::make_unique<Enemy>("Goblin", 50.0f));

for (auto& character : characters) {
    character->Attack(); // Appelle la méthode appropriée
}
```

## 📦 STL (Standard Template Library)

### Containers
```cpp
#include <vector>
#include <array>
#include <map>
#include <unordered_map>
#include <set>

// Vector - Tableau dynamique / Dynamic array
std::vector<int> numbers = {1, 2, 3, 4, 5};
numbers.push_back(6);
numbers.pop_back();
int first = numbers[0];
int size = numbers.size();

// Array - Taille fixe / Fixed size
std::array<float, 3> position = {0.0f, 1.0f, 0.0f};

// Map - Clé-valeur triée / Sorted key-value
std::map<std::string, int> scores;
scores["Alice"] = 100;
scores["Bob"] = 95;

// Unordered Map - Hash map (plus rapide)
std::unordered_map<int, std::string> idToName;
idToName[1] = "Player1";

// Set - Valeurs uniques triées
std::set<std::string> uniqueNames;
uniqueNames.insert("Alice");
uniqueNames.insert("Bob");
```

### Algorithmes / Algorithms
```cpp
#include <algorithm>
#include <numeric>

std::vector<int> numbers = {5, 2, 8, 1, 9};

// Tri / Sort
std::sort(numbers.begin(), numbers.end());

// Recherche / Find
auto it = std::find(numbers.begin(), numbers.end(), 8);
if (it != numbers.end()) {
    // Trouvé!
}

// Transform
std::vector<int> doubled(numbers.size());
std::transform(numbers.begin(), numbers.end(), doubled.begin(),
    [](int n) { return n * 2; });

// Somme / Sum
int sum = std::accumulate(numbers.begin(), numbers.end(), 0);

// Min/Max
int minVal = *std::min_element(numbers.begin(), numbers.end());
int maxVal = *std::max_element(numbers.begin(), numbers.end());
```

### Lambdas (C++11+)
```cpp
// Lambda basique
auto add = [](int a, int b) { return a + b; };
int result = add(5, 3);

// Capture de variables
int multiplier = 10;
auto multiply = [multiplier](int x) { return x * multiplier; };

// Capture types:
[=]     // Capture tout par valeur
[&]     // Capture tout par référence
[x]     // Capture x par valeur
[&x]    // Capture x par référence
[this]  // Capture this

// Usage avec algorithmes
std::vector<int> numbers = {1, 2, 3, 4, 5};
std::for_each(numbers.begin(), numbers.end(), 
    [](int n) { std::cout << n << std::endl; });
```

## 🎯 Patterns de jeux / Game Patterns

### Singleton
```cpp
class GameManager {
private:
    static GameManager* s_instance;
    
    GameManager() {} // Constructeur privé
    GameManager(const GameManager&) = delete; // Pas de copie
    GameManager& operator=(const GameManager&) = delete;
    
public:
    static GameManager& GetInstance() {
        if (!s_instance) {
            s_instance = new GameManager();
        }
        return *s_instance;
    }
    
    static void Destroy() {
        delete s_instance;
        s_instance = nullptr;
    }
    
    void Update() {
        // Game logic
    }
};

GameManager* GameManager::s_instance = nullptr;

// Usage:
GameManager::GetInstance().Update();
```

### Object Pool
```cpp
template<typename T>
class ObjectPool {
private:
    std::vector<std::unique_ptr<T>> m_pool;
    std::queue<T*> m_available;
    
public:
    T* Acquire() {
        if (m_available.empty()) {
            auto obj = std::make_unique<T>();
            T* ptr = obj.get();
            m_pool.push_back(std::move(obj));
            return ptr;
        }
        
        T* obj = m_available.front();
        m_available.pop();
        return obj;
    }
    
    void Release(T* obj) {
        m_available.push(obj);
    }
};

// Usage:
ObjectPool<Bullet> bulletPool;
Bullet* bullet = bulletPool.Acquire();
// Use bullet...
bulletPool.Release(bullet);
```

### Component System
```cpp
class Component {
public:
    virtual ~Component() = default;
    virtual void Update(float deltaTime) = 0;
};

class GameObject {
private:
    std::vector<std::unique_ptr<Component>> m_components;
    
public:
    template<typename T, typename... Args>
    T* AddComponent(Args&&... args) {
        auto component = std::make_unique<T>(std::forward<Args>(args)...);
        T* ptr = component.get();
        m_components.push_back(std::move(component));
        return ptr;
    }
    
    template<typename T>
    T* GetComponent() {
        for (auto& component : m_components) {
            if (T* casted = dynamic_cast<T*>(component.get())) {
                return casted;
            }
        }
        return nullptr;
    }
    
    void Update(float deltaTime) {
        for (auto& component : m_components) {
            component->Update(deltaTime);
        }
    }
};
```

## 🔧 Unreal Engine C++

### Actor basique / Basic Actor
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
    float Speed = 100.0f;
    
    UFUNCTION(BlueprintCallable, Category = "Actions")
    void Move(FVector Direction);
    
protected:
    virtual void BeginPlay() override;
    
public:
    virtual void Tick(float DeltaTime) override;
    
private:
    UPROPERTY()
    UStaticMeshComponent* MeshComponent;
};

// MyActor.cpp
#include "MyActor.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;
    
    MeshComponent = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
    RootComponent = MeshComponent;
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();
}

void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
}

void AMyActor::Move(FVector Direction)
{
    FVector NewLocation = GetActorLocation() + Direction * Speed * GetWorld()->GetDeltaSeconds();
    SetActorLocation(NewLocation);
}
```

## 🐛 Debugging

### Techniques de debug / Debug Techniques
```cpp
// Logs
#include <iostream>
std::cout << "Value: " << value << std::endl;

// Debug assertions
#include <cassert>
assert(pointer != nullptr && "Pointer should not be null!");

// Unreal Engine
UE_LOG(LogTemp, Warning, TEXT("Health: %f"), Health);
UE_LOG(LogTemp, Error, TEXT("Critical error!"));

// Conditional compilation
#ifdef _DEBUG
    std::cout << "Debug mode active" << std::endl;
#endif

#if UE_BUILD_DEBUG
    DrawDebugLine(GetWorld(), Start, End, FColor::Red, false, 2.0f);
#endif
```

### Memory Leaks
```cpp
// Éviter les fuites / Avoid leaks
// MAUVAIS:
int* leaked = new int(42);
// Jamais delete!

// BON: Utiliser smart pointers
auto managed = std::make_unique<int>(42);
// Automatiquement nettoyé

// BON: RAII (Resource Acquisition Is Initialization)
class FileHandle {
private:
    FILE* m_file;
public:
    FileHandle(const char* filename) {
        m_file = fopen(filename, "r");
    }
    ~FileHandle() {
        if (m_file) {
            fclose(m_file);
        }
    }
};
```

## 📚 Ressources / Resources

### Documentation
- [cppreference.com](https://en.cppreference.com/) - Référence complète
- [LearnCpp.com](https://www.learncpp.com/) - Tutoriels complets
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/) - Best practices

### Livres / Books
- "Effective C++" - Scott Meyers
- "C++ Primer" - Stanley Lippman
- "Game Programming Patterns" - Robert Nystrom

### Communauté / Community
- [Stack Overflow](https://stackoverflow.com/questions/tagged/c++)
- [r/cpp](https://www.reddit.com/r/cpp/)
- [C++ Slack](https://cpplang.slack.com/)

## 💡 Best Practices

### Performance
```cpp
// Pass par référence const pour éviter copies
void ProcessData(const std::vector<int>& data) {
    // data n'est pas copié
}

// Move semantics (C++11+)
std::vector<int> CreateLargeVector() {
    std::vector<int> result(1000000);
    return result; // Move, pas de copie!
}

// Reserve pour éviter réallocations
std::vector<int> numbers;
numbers.reserve(1000);
for (int i = 0; i < 1000; ++i) {
    numbers.push_back(i);
}

// Utiliser emplace au lieu de push
std::vector<std::string> names;
names.emplace_back("Alice"); // Construit in-place
names.push_back("Bob");      // Crée temporaire puis copie
```

### Modern C++ (C++11/14/17/20)
```cpp
// Auto type deduction
auto value = 42;
auto name = std::string("Player");

// Range-based for loops
for (const auto& item : collection) {
    // Process item
}

// nullptr au lieu de NULL
MyObject* ptr = nullptr;

// enum class (type-safe)
enum class Color { Red, Green, Blue };
Color c = Color::Red;

// Uniform initialization
std::vector<int> numbers{1, 2, 3, 4, 5};

// constexpr (compile-time constants)
constexpr int MaxPlayers = 4;

// std::optional (C++17)
std::optional<int> FindValue() {
    if (found) {
        return 42;
    }
    return std::nullopt;
}
```

## 📌 Notes importantes / Important Notes

### Compilation
```bash
# GCC/Clang
g++ -std=c++17 -Wall -Wextra main.cpp -o program

# Avec optimisation
g++ -std=c++17 -O2 main.cpp -o program

# Debug symbols
g++ -std=c++17 -g main.cpp -o program_debug
```

### Différences C vs C++
- C++: Classes et OOP
- C++: Namespaces
- C++: Références
- C++: Exceptions
- C++: Templates
- C++: STL
- C++: RAII et smart pointers
