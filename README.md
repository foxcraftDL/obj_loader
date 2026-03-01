# obj_loader
obj_loader
obj_loader es un cargador de modelos Wavefront .OBJ escrito en Python, diseñado para ser ligero, rápido y completamente independiente de cualquier librería gráfica. Permite cargar vértices, normales, UVs e índices sin requerir OpenGL, pygame, moderngl ni ningún motor específico.
El objetivo es ofrecer un loader universal que pueda integrarse en cualquier motor gráfico, desde pygame hasta OpenGL moderno o renderizadores propios.

Instalación
pip install obj_loader



Características principales
- Carga archivos .obj sin dependencias externas.
- Extrae:
- vértices (x, y, z)
- normales (nx, ny, nz)
- coordenadas UV (u, v)
- índices triangulados
- Funciona con cualquier motor gráfico o backend.
- Incluye backends.py, que documenta motores compatibles.
- No requiere OpenGL, pygame ni moderngl para cargar modelos.
- API simple, limpia y portable.

Uso básico
from obj_loader import OBJMesh

mesh = OBJMesh.from_file("modelo.obj")

print(len(mesh.vertices))
print(len(mesh.indices))



Integración con cualquier motor gráfico
obj_loader no dibuja nada por sí mismo.
En su lugar, expone datos listos para enviar a cualquier backend gráfico.
Obtener triángulos listos para render
tris = mesh.as_triangles()

for a, b, c in tris:
    print(a, b, c)


Ejemplo con pygame (sin OpenGL)
import pygame
from obj_loader import OBJMesh

mesh = OBJMesh.from_file("modelo.obj")

def project(v):
    x, y, z = v
    return int(x*50 + 300), int(-y*50 + 300)

pygame.init()
screen = pygame.display.set_mode((600, 600))

running = True
while running:
    for e in pygame.event.get():
        if e.type == pygame.QUIT:
            running = False

    screen.fill((0, 0, 0))

    for a, b, c in mesh.as_triangles():
        pygame.draw.polygon(screen, (255, 255, 255), [project(a), project(b), project(c)], 1)

    pygame.display.flip()



Backends compatibles
Tu librería incluye backends.py, que lista los motores gráficos compatibles:
from obj_loader import OBJMesh

print(OBJMesh.supported_backends())


Salida:
{
  "pygame": "Renderizado software o proyección manual 3D→2D.",
  "pyopengl": "Render inmediato o VBOs.",
  "moderngl": "Render moderno con shaders.",
  "pyglet": "Render 3D con OpenGL integrado.",
  "panda3d": "Motor 3D completo.",
  "ursina": "Motor 3D simple basado en panda3d.",
  "custom": "Motores propios o render CPU."
}



API
OBJMesh.from_file(path)
Carga un archivo .obj y devuelve un objeto OBJMesh.
mesh.vertices
Lista de vértices (x, y, z).
mesh.normals
Lista de normales (nx, ny, nz).
mesh.uvs
Lista de coordenadas UV (u, v).
mesh.indices
Lista de índices enteros triangulados.
mesh.as_triangles()
Devuelve una lista de triángulos:
[(v1, v2, v3), ...]


OBJMesh.supported_backends()
Devuelve un diccionario con los motores gráficos compatibles.

🛠️ Requisitos
- Python 3.8 o superior.
- No requiere librerías gráficas.

- Contribuciones
Las contribuciones son bienvenidas.
Puedes abrir issues o pull requests en:
https://github.com/foxcraftDL/obj_loader



Licencia
MIT License.
