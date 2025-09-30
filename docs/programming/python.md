# Python

## 📝 Vue d'ensemble / Overview
Documentation Python pour l'automatisation et les outils de pipeline dans le contexte du cours NAND510.

Python documentation for automation and pipeline tools in the context of the NAND510 course.

## 🚀 Configuration / Setup

### Installation
```bash
# Python 3.10+ recommandé
python --version

# pip (package manager)
pip --version
pip install <package>
pip install -r requirements.txt
```

### Environnements virtuels / Virtual Environments
```bash
# Créer un venv
python -m venv venv

# Activer (Windows)
venv\Scripts\activate

# Activer (Linux/Mac)
source venv/bin/activate

# Désactiver
deactivate

# requirements.txt
pip freeze > requirements.txt
```

## 📚 Fondamentaux / Fundamentals

### Types de base / Basic Types
```python
# Nombres / Numbers
integer = 42
floating = 3.14
complex_num = 1 + 2j

# Strings
text = "Hello"
multi_line = """
Multiple
lines
"""
f_string = f"Value: {integer}"

# Booléens / Booleans
is_active = True
is_disabled = False

# None (null equivalent)
value = None

# Collections
list_items = [1, 2, 3, 4, 5]
tuple_items = (1, 2, 3)  # Immutable
set_items = {1, 2, 3}    # Unique values
dict_items = {"key": "value", "count": 42}
```

### Structures de contrôle / Control Structures
```python
# If/Elif/Else
if condition:
    pass
elif other_condition:
    pass
else:
    pass

# For loops
for item in collection:
    print(item)

for i in range(10):
    print(i)

for index, value in enumerate(collection):
    print(f"{index}: {value}")

# While
while condition:
    pass

# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]

# Dict comprehension
name_lengths = {name: len(name) for name in names}
```

### Fonctions / Functions
```python
# Fonction basique / Basic function
def greet(name):
    return f"Hello, {name}!"

# Arguments par défaut / Default arguments
def create_character(name, health=100, level=1):
    return {"name": name, "health": health, "level": level}

# *args et **kwargs
def process_values(*args, **kwargs):
    for arg in args:
        print(arg)
    for key, value in kwargs.items():
        print(f"{key}: {value}")

# Lambda functions
square = lambda x: x**2
result = square(5)

# Type hints (Python 3.5+)
def add_numbers(a: int, b: int) -> int:
    return a + b

from typing import List, Dict, Optional

def process_names(names: List[str]) -> Dict[str, int]:
    return {name: len(name) for name in names}

def find_player(id: int) -> Optional[str]:
    # Peut retourner str ou None
    return player_dict.get(id)
```

## 🎮 Programmation orientée objet / Object-Oriented Programming

### Classes
```python
class Character:
    # Variable de classe / Class variable
    character_count = 0
    
    def __init__(self, name: str, health: float):
        """Constructeur / Constructor"""
        self.name = name
        self.health = health
        self.max_health = health
        Character.character_count += 1
    
    def __str__(self):
        """String representation"""
        return f"{self.name} ({self.health}/{self.max_health} HP)"
    
    def __repr__(self):
        """Debug representation"""
        return f"Character(name='{self.name}', health={self.health})"
    
    # Property (getter/setter)
    @property
    def is_alive(self) -> bool:
        return self.health > 0
    
    @property
    def health_percentage(self) -> float:
        return (self.health / self.max_health) * 100
    
    # Méthode d'instance / Instance method
    def take_damage(self, amount: float):
        self.health = max(0, self.health - amount)
        if not self.is_alive:
            self.die()
    
    def heal(self, amount: float):
        self.health = min(self.max_health, self.health + amount)
    
    # Méthode privée (convention) / Private method
    def _calculate_damage(self, base_damage: float) -> float:
        return base_damage * 1.5
    
    def die(self):
        print(f"{self.name} has died!")
    
    # Méthode de classe / Class method
    @classmethod
    def get_character_count(cls) -> int:
        return cls.character_count
    
    # Méthode statique / Static method
    @staticmethod
    def is_valid_name(name: str) -> bool:
        return len(name) > 0 and len(name) <= 20

# Usage
player = Character("Hero", 100.0)
player.take_damage(25)
print(player)  # Hero (75.0/100.0 HP)
print(f"Alive: {player.is_alive}")
```

### Héritage / Inheritance
```python
class Player(Character):
    def __init__(self, name: str, health: float, player_class: str):
        super().__init__(name, health)
        self.player_class = player_class
        self.experience = 0
        self.level = 1
    
    def gain_experience(self, amount: int):
        self.experience += amount
        while self.experience >= self.experience_to_next_level:
            self.level_up()
    
    def level_up(self):
        self.level += 1
        self.max_health += 10
        self.health = self.max_health
        print(f"{self.name} reached level {self.level}!")
    
    @property
    def experience_to_next_level(self) -> int:
        return self.level * 100
    
    # Override méthode / Override method
    def die(self):
        print(f"Player {self.name} has been defeated!")
        # Respawn logic here

class Enemy(Character):
    def __init__(self, name: str, health: float, damage: float):
        super().__init__(name, health)
        self.damage = damage
    
    def attack(self, target: Character):
        target.take_damage(self.damage)
```

### Mixins et héritage multiple / Mixins and Multiple Inheritance
```python
class Movable:
    def __init__(self):
        self.x = 0
        self.y = 0
    
    def move(self, dx: float, dy: float):
        self.x += dx
        self.y += dy
    
    def get_position(self) -> tuple:
        return (self.x, self.y)

class Damageable:
    def __init__(self):
        self.health = 100
    
    def take_damage(self, amount: float):
        self.health -= amount

class GameObject(Movable, Damageable):
    def __init__(self, name: str):
        Movable.__init__(self)
        Damageable.__init__(self)
        self.name = name
```

## 🎨 Maya Python API

### Maya Commands (maya.cmds)
```python
import maya.cmds as cmds

# Créer des objets / Create objects
cube = cmds.polyCube(name="MyCube", width=2, height=2, depth=2)[0]
sphere = cmds.polySphere(name="MySphere", radius=1)[0]

# Sélection / Selection
cmds.select(cube)
selected = cmds.ls(selection=True)
all_meshes = cmds.ls(type="mesh")

# Transformations
cmds.move(0, 5, 0, cube)
cmds.rotate(45, 0, 0, cube)
cmds.scale(2, 2, 2, cube)

# Attributs / Attributes
cmds.setAttr(f"{cube}.visibility", 0)
visibility = cmds.getAttr(f"{cube}.visibility")

# Parent/Unparent
cmds.parent(sphere, cube)
cmds.parent(sphere, world=True)

# Delete
cmds.delete(cube)

# Export
cmds.file("C:/Export/scene.fbx", 
          force=True,
          options="v=0",
          type="FBX export",
          exportSelected=True)
```

### Batch operations
```python
def export_all_meshes(output_dir: str):
    """Exporte tous les meshes individuellement"""
    import os
    
    # Obtenir tous les meshes
    meshes = cmds.ls(type="mesh", long=True)
    transforms = [cmds.listRelatives(mesh, parent=True)[0] 
                  for mesh in meshes]
    
    for transform in transforms:
        # Sélectionner l'objet
        cmds.select(transform)
        
        # Nom de fichier
        filename = f"{transform}.fbx"
        filepath = os.path.join(output_dir, filename)
        
        # Exporter
        cmds.file(filepath,
                  force=True,
                  options="v=0",
                  type="FBX export",
                  exportSelected=True)
        
        print(f"Exported: {filepath}")
    
    cmds.select(clear=True)

# Usage:
export_all_meshes("C:/Exports/Models")
```

## 🔷 Blender Python API

### bpy basics
```python
import bpy

# Accéder aux objets / Access objects
obj = bpy.data.objects["Cube"]
all_objects = bpy.data.objects

# Scène active / Active scene
scene = bpy.context.scene

# Créer des objets / Create objects
bpy.ops.mesh.primitive_cube_add(location=(0, 0, 0))
bpy.ops.mesh.primitive_uv_sphere_add(location=(3, 0, 0))

# Sélection / Selection
obj = bpy.context.active_object
selected = bpy.context.selected_objects

# Transformations
obj.location = (1, 2, 3)
obj.rotation_euler = (0, 0, 1.57)  # radians
obj.scale = (2, 2, 2)

# Materials
mat = bpy.data.materials.new(name="MyMaterial")
mat.use_nodes = True
obj.data.materials.append(mat)

# Delete
bpy.data.objects.remove(obj, do_unlink=True)
```

### Batch operations
```python
def export_selected_fbx(filepath: str, scale: float = 1.0):
    """Exporte les objets sélectionnés en FBX"""
    import os
    
    if not bpy.context.selected_objects:
        print("Aucun objet sélectionné")
        return
    
    bpy.ops.export_scene.fbx(
        filepath=filepath,
        use_selection=True,
        global_scale=scale,
        apply_unit_scale=True,
        apply_scale_options='FBX_SCALE_ALL',
        axis_forward='-Z',
        axis_up='Y',
        use_mesh_modifiers=True,
        mesh_smooth_type='FACE',
        bake_anim=True
    )
    
    print(f"Exporté vers: {filepath}")

def batch_export_objects(output_dir: str):
    """Exporte chaque objet sélectionné individuellement"""
    import os
    
    selected = bpy.context.selected_objects
    
    for obj in selected:
        # Désélectionner tout
        bpy.ops.object.select_all(action='DESELECT')
        
        # Sélectionner cet objet
        obj.select_set(True)
        bpy.context.view_layer.objects.active = obj
        
        # Exporter
        filename = f"{obj.name}.fbx"
        filepath = os.path.join(output_dir, filename)
        export_selected_fbx(filepath)
    
    # Resélectionner tous
    for obj in selected:
        obj.select_set(True)
```

## 📦 Bibliothèques utiles / Useful Libraries

### Fichiers et chemins / Files and Paths
```python
import os
import pathlib
from pathlib import Path

# os.path (ancien style / old style)
filepath = "C:/Projects/Assets/model.fbx"
dirname = os.path.dirname(filepath)
basename = os.path.basename(filepath)
name, ext = os.path.splitext(basename)

# pathlib (moderne / modern - recommandé)
path = Path("C:/Projects/Assets/model.fbx")
print(path.parent)      # C:/Projects/Assets
print(path.name)        # model.fbx
print(path.stem)        # model
print(path.suffix)      # .fbx

# Opérations
path.exists()
path.is_file()
path.is_dir()
path.mkdir(parents=True, exist_ok=True)

# Lister fichiers / List files
for file in path.parent.glob("*.fbx"):
    print(file)

# Joindre chemins / Join paths
export_path = Path("C:/Exports") / "Models" / "character.fbx"
```

### JSON (pour configuration)
```python
import json

# Écrire JSON / Write JSON
data = {
    "project": "NAND510",
    "settings": {
        "resolution": [1920, 1080],
        "quality": "high"
    }
}

with open("config.json", "w") as f:
    json.dump(data, f, indent=4)

# Lire JSON / Read JSON
with open("config.json", "r") as f:
    loaded_data = json.load(f)

print(loaded_data["settings"]["quality"])
```

### Logging
```python
import logging

# Configuration
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('tool.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

# Usage
logger.debug("Debug message")
logger.info("Info message")
logger.warning("Warning message")
logger.error("Error message")
logger.critical("Critical message")

try:
    risky_operation()
except Exception as e:
    logger.exception("An error occurred")
```

### argparse (CLI arguments)
```python
import argparse

parser = argparse.ArgumentParser(description='Export tool')
parser.add_argument('input', help='Input file path')
parser.add_argument('output', help='Output directory')
parser.add_argument('--scale', type=float, default=1.0,
                    help='Export scale factor')
parser.add_argument('--format', choices=['fbx', 'obj', 'gltf'],
                    default='fbx', help='Export format')
parser.add_argument('--verbose', action='store_true',
                    help='Verbose output')

args = parser.parse_args()

if args.verbose:
    print(f"Input: {args.input}")
    print(f"Output: {args.output}")
    print(f"Scale: {args.scale}")
    print(f"Format: {args.format}")

# Usage: python script.py input.ma output/ --scale 100 --format fbx --verbose
```

## 🛠️ Outils de pipeline / Pipeline Tools

### Batch file processor
```python
from pathlib import Path
from typing import List, Callable
import logging

class BatchProcessor:
    def __init__(self, input_dir: str, output_dir: str):
        self.input_dir = Path(input_dir)
        self.output_dir = Path(output_dir)
        self.output_dir.mkdir(parents=True, exist_ok=True)
        self.logger = logging.getLogger(__name__)
    
    def process_files(self, 
                     pattern: str,
                     processor: Callable[[Path, Path], bool]) -> List[Path]:
        """Process tous les fichiers matchant le pattern"""
        processed_files = []
        
        for input_file in self.input_dir.glob(pattern):
            output_file = self.output_dir / input_file.name
            
            try:
                if processor(input_file, output_file):
                    processed_files.append(output_file)
                    self.logger.info(f"Processed: {input_file.name}")
                else:
                    self.logger.warning(f"Failed: {input_file.name}")
            except Exception as e:
                self.logger.error(f"Error processing {input_file.name}: {e}")
        
        return processed_files

# Usage example:
def convert_texture(input_path: Path, output_path: Path) -> bool:
    # Logique de conversion
    return True

processor = BatchProcessor("C:/Input", "C:/Output")
results = processor.process_files("*.tga", convert_texture)
```

### Asset validator
```python
from dataclasses import dataclass
from typing import List

@dataclass
class ValidationResult:
    is_valid: bool
    errors: List[str]
    warnings: List[str]

class AssetValidator:
    def __init__(self):
        self.errors = []
        self.warnings = []
    
    def validate_naming(self, name: str) -> bool:
        """Valide la nomenclature"""
        if ' ' in name:
            self.errors.append(f"Name contains spaces: {name}")
            return False
        
        if not name[0].isupper():
            self.warnings.append(f"Name should start with uppercase: {name}")
        
        return True
    
    def validate_mesh(self, mesh_data: dict) -> ValidationResult:
        """Valide un mesh"""
        self.errors.clear()
        self.warnings.clear()
        
        # Vérifier le nombre de polygones
        poly_count = mesh_data.get('poly_count', 0)
        if poly_count > 50000:
            self.warnings.append(f"High poly count: {poly_count}")
        
        # Vérifier les UVs
        has_uvs = mesh_data.get('has_uvs', False)
        if not has_uvs:
            self.errors.append("Missing UV coordinates")
        
        # Vérifier les materials
        materials = mesh_data.get('materials', [])
        if not materials:
            self.errors.append("No materials assigned")
        
        is_valid = len(self.errors) == 0
        return ValidationResult(
            is_valid=is_valid,
            errors=self.errors.copy(),
            warnings=self.warnings.copy()
        )

# Usage:
validator = AssetValidator()
result = validator.validate_mesh({
    'poly_count': 25000,
    'has_uvs': True,
    'materials': ['M_Character_Skin']
})

if not result.is_valid:
    for error in result.errors:
        print(f"ERROR: {error}")
```

## 📚 Ressources / Resources

### Documentation
- [Python Official Docs](https://docs.python.org/3/)
- [Real Python](https://realpython.com/) - Tutoriels excellents
- [Python Package Index (PyPI)](https://pypi.org/)

### Maya Python
- [Maya Commands Reference](https://help.autodesk.com/view/MAYAUL/ENU/?guid=CommandsPython)
- [Maya Python API](https://help.autodesk.com/view/MAYAUL/ENU/?guid=Maya_SDK_py_ref_index_html)

### Blender Python
- [Blender Python API](https://docs.blender.org/api/current/)
- [Blender Stack Exchange](https://blender.stackexchange.com/questions/tagged/python)

## 💡 Best Practices

### Code Style (PEP 8)
```python
# Bon / Good
def calculate_damage(base_damage, multiplier):
    return base_damage * multiplier

# Mauvais / Bad
def CalculateDamage(BaseDamage,multiplier):return BaseDamage*multiplier

# Imports groupés et triés
import os
import sys

import maya.cmds as cmds
import numpy as np

from . import utils
from .core import manager

# Docstrings
def export_mesh(mesh_name: str, output_path: str) -> bool:
    """
    Exporte un mesh vers un fichier FBX.
    
    Args:
        mesh_name: Nom du mesh à exporter
        output_path: Chemin de destination du fichier
        
    Returns:
        True si l'export a réussi, False sinon
        
    Raises:
        ValueError: Si le mesh n'existe pas
        IOError: Si le chemin n'est pas accessible
    """
    pass
```

### Error Handling
```python
# Spécifique > Général
try:
    result = risky_operation()
except FileNotFoundError:
    logger.error("File not found")
except PermissionError:
    logger.error("Permission denied")
except Exception as e:
    logger.exception(f"Unexpected error: {e}")
finally:
    cleanup()

# Context managers pour ressources
with open("file.txt", "r") as f:
    data = f.read()
# Fichier automatiquement fermé
```

## 📌 Notes importantes / Important Notes

### Virtual Environment
Toujours utiliser un venv pour éviter les conflits de dépendances.

Always use a venv to avoid dependency conflicts.

### Python 2 vs 3
- Maya 2022+: Python 3
- Blender 2.8+: Python 3
- Unreal/Unity: Python 3 pour tools externes

### Performance
- Utiliser list comprehensions au lieu de loops quand possible
- Profiler avec `cProfile` pour identifier bottlenecks
- Utiliser NumPy pour calculs numériques intensifs
