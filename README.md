# Lab 4: Carga de modelos

Un renderizador de software 3D implementado en Rust que carga y renderiza modelos OBJ con rasterización de triángulos, transformaciones 3D e iluminación básica.

## Características

- **Carga de modelos OBJ**: Soporte completo para archivos Wavefront OBJ
- **Rasterización de triángulos**: Implementación desde cero usando coordenadas barycéntricas
- **Transformaciones 3D**: Traslación, rotación y escalado en tiempo real
- **Z-Buffer**: Depth testing para renderizado correcto de profundidad
- **Iluminación básica**: Sistema de iluminación direccional con normales
- **Controles interactivos**: Navegación completa con teclado

## Tecnologías

- **Rust** - Lenguaje de programación principal
- **nalgebra-glm** - Matemáticas 3D (vectores, matrices)
- **minifb** - Creación de ventanas y manejo de framebuffer
- **tobj** - Cargador de archivos OBJ

## Requisitos

- Rust 1.70 o superior
- Archivo `nave.obj` en la carpeta `assets/`

## Instalación y Ejecución
```bash
# Clonar el repositorio
git clone https://github.com/Albu231311/Modelo_nave.git
cd Modelo_nave

# Compilar y ejecutar
cargo run --release
```

## Controles

| Tecla | Acción |
|-------|--------|
| `←→↑↓` | Mover nave |
| `A/S` | Escalar (A = pequeño, S = grande) |
| `Q/W` | Rotar en eje X |
| `E/R` | Rotar en eje Y |
| `T/Y` | Rotar en eje Z |
| `ESC` | Salir |

## Estructura del Proyecto
```
proyecto-nave/
├── Cargo.toml
├── assets/
│   └── nave.obj          # Modelo 3D de la nave
└── src/
    ├── main.rs           # Programa principal y loop de renderizado
    ├── obj.rs            # Cargador de archivos OBJ
    ├── vertex.rs         # Estructura de vértices
    ├── triangle.rs       # Rasterización de triángulos
    ├── shaders.rs        # Vertex shader y transformaciones
    ├── framebuffer.rs    # Buffer de frame y Z-buffer
    ├── fragment.rs       # Estructura de fragmentos
    ├── color.rs          # Sistema de colores
    └── line.rs           # Rasterización de líneas
```

## 🎯 Pipeline de Renderizado

1. **Carga del modelo**: Lee archivo OBJ y extrae vértices e índices
2. **Vertex Shader**: Aplica transformaciones 3D a cada vértice
3. **Recorrido de caras**: Itera manualmente sobre índices de triángulos
4. **Rasterización**: Convierte triángulos 3D a píxeles 2D
5. **Fragment Shader**: Aplica iluminación y colores
6. **Z-Buffer**: Determina visibilidad por profundidad

## Implementación Técnica

### Recorrido Manual de Caras
```rust
// Cada 3 índices forman un triángulo
for triangle_idx in (0..indices.len()).step_by(3) {
    let i1 = indices[triangle_idx] as usize;
    let i2 = indices[triangle_idx + 1] as usize; 
    let i3 = indices[triangle_idx + 2] as usize;
    
    // Obtener vértices y renderizar triángulo
    let v1 = &transformed_vertices[i1];
    let v2 = &transformed_vertices[i2];
    let v3 = &transformed_vertices[i3];
    
    let fragments = triangle(v1, v2, v3);
}
```

### Rasterización de Triángulos
- **Bounding Box**: Optimización para reducir píxeles a procesar
- **Coordenadas Barycéntricas**: Determinar si un píxel está dentro del triángulo
- **Interpolación**: Suavizar normales y profundidad entre vértices

### Sistema de Iluminación
- **Luz direccional**: Simulación de luz solar/estelar
- **Producto punto**: Cálculo de intensidad basado en normales
- **Colores dinámicos**: Variación de brillo según orientación de superficie

## Características Visuales

- **Fondo espacial**: Azul oscuro (#001122)
- **Nave dorada**: Color base dorado con iluminación realista
- **Sombreado**: Las caras orientadas hacia la luz se ven brillantes, las opuestas se ven oscuras
- **FPS adaptativo**: Reduce automáticamente cuando hay mucha geometría en pantalla


## Capturas
<img width="536" height="288" alt="nave2" src="https://github.com/user-attachments/assets/6702678a-9f49-46c0-9107-2449cda08f28" />
<img width="536" height="288" alt="nave3" src="https://github.com/user-attachments/assets/e675634b-f8a3-4dd2-a5f7-469acb4d5039" />
<img width="536" height="288" alt="image" src="https://github.com/user-attachments/assets/510c3f06-6358-4299-b56e-5e28de05274c" />




