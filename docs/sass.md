# GUÍA COMPLETA Y CHULETA DE REFERENCIA: SASS / SCSS MODERNO

Esta guía de referencia reúne la sintaxis moderna de Sass (SCSS) utilizando el sistema de módulos oficiales (`@use`), manipulación de colecciones (listas y mapas), estructuras de control de flujo, mixins paramétricos, funciones personalizadas y herencia.

---

## 1. Módulos Oficiales de Sass (`@use`)

En el sistema de módulos moderno de Sass, las funciones globales antiguas (como `lighten()`, `map-get()`, etc.) han sido sustituidas por módulos incorporados. Para usarlas, debes importar el módulo correspondiente al principio de tu archivo `.scss`.

| Módulo | Descripción | Ejemplo de Importación |
| :--- | :--- | :--- |
| **`sass:color`** | Manipulación y ajuste de colores (luminosidad, opacidad, mezcla). | `@use "sass:color";` |
| **`sass:map`** | Operaciones avanzadas con mapas (lectura, fusión, verificación). | `@use "sass:map";` |
| **`sass:list`** | Operaciones con listas (acceso por índice, añadir elementos, longitud). | `@use "sass:list";` |
| **`sass:math`** | Funciones matemáticas (división, redondeo, porcentajes). | `@use "sass:math";` |
| **`sass:string`** | Manipulación de cadenas de texto (comillas, búsqueda). | `@use "sass:string";` |

---

## 2. Variables, Listas y Mapas

### 2.1 Declaración de Colecciones
```scss
@use "sass:color";
@use "sass:map";
@use "sass:list";

// Variables simples
$color-principal: #e11d48;
$fuente-base: 'Roboto', sans-serif;

// Lista (indexada desde 1)
$tamanos-fuente: 0.875rem, 1rem, 1.25rem, 1.5rem, 2rem;
$colores-tema: (#0f172a, #1e293b, #334155);

// Mapa (Clave: Valor)
$paleta-colores: (
  "primario": #e11d48,
  "secundario": #7c3aed,
  "exito": #16a34a,
  "advertencia": #d97706,
  "peligro": #dc2626,
  "oscuro": #0f172a
);
```

---

## 3. Funciones del Módulo `sass:map`

Para leer o manipular mapas en Sass moderno, se utilizan las funciones del módulo `map`.

```scss
@use "sass:map";

$botones: (
  "primario": #e11d48,
  "secundario": #7c3aed,
  "cancelar": #64748b
);

// 1. Obtener valor por clave: map.get($mapa, $clave)
.btn-principal {
  background-color: map.get($botones, "primario");
}

// 2. Comprobar si existe una clave: map.has-key($mapa, $clave)
@if map.has-key($botones, "secundario") {
  .btn-secundario {
    background-color: map.get($botones, "secundario");
  }
}

// 3. Fusionar dos mapas: map.merge($mapa1, $mapa2)
$botones-extra: ("info": #0284c7);
$mapa-completo: map.merge($botones, $botones-extra);

// 4. Obtener claves y valores
$todas-las-claves: map.keys($botones);   // ("primario", "secundario", "cancelar")
$todos-los-valores: map.values($botones); // (#e11d48, #7c3aed, #64748b)
```

---

## 4. Funciones del Módulo `sass:list`

Las listas en Sass comienzan en el índice **1** (no 0).

```scss
@use "sass:list";

$espaciados: 4px, 8px, 16px, 24px, 32px;

.contenedor {
  // 1. Obtener elemento por índice (indexado desde 1): list.nth($lista, $indice)
  padding: list.nth($espaciados, 3); // 16px

  // 2. Obtener número de elementos: list.length($lista)
  margin-bottom: list.length($espaciados) * 2px; // 5 * 2px = 10px

  // 3. Buscar índice de un valor: list.index($lista, $valor)
  $posicion: list.index($espaciados, 16px); // 3

  // 4. Añadir elemento al final: list.append($lista, $nuevo-valor)
  $nuevos-espaciados: list.append($espaciados, 48px);
}
```

---

## 5. Estructuras de Control de Flujo

### 5.1 Condicionales (`@if`, `@else if`, `@else`)
Permiten aplicar reglas de estilos en función de condiciones lógicas.

```scss
@use "sass:color";
@use "sass:map";

@mixin estilo-tarjeta($tipo-tema) {
  @if $tipo-tema == "oscuro" {
    background-color: #0f172a;
    color: #f8fafc;
    border: 1px solid #334155;
  } @else if $tipo-tema == "claro" {
    background-color: #ffffff;
    color: #0f172a;
    border: 1px solid #e2e8f0;
  } @else {
    background-color: #f1f5f9;
    color: #334155;
    border: 1px dashed #cbd5e1;
  }
}

.tarjeta-modo-oscuro {
  @include estilo-tarjeta("oscuro");
}
```

### 5.2 Bucles sobre Mapas y Listas (`@each`)
El bucle `@each` es la estructura más eficiente para generar clases utilitarias de forma dinámica.

```scss
@use "sass:map";
@use "sass:color";

$estados: (
  "exito": #16a34a,
  "alerta": #d97706,
  "error": #dc2626,
  "info": #0284c7
);

// Bucle sobre un mapa (clave, valor)
@each $nombre-estado, $color-base in $estados {
  .badge-#{$nombre-estado} {
    background-color: $color-base;
    color: #ffffff;
    border: 1px solid color.adjust($color-base, $lightness: -15%);
    padding: 4px 8px;
    border-radius: 4px;
  }
}
```

### 5.3 Bucles Numéricos (`@for`)
Se utilizan para generar secuencias numéricas (por ejemplo, sistemas de columnas o utilidades de margen).

```scss
// @for $i from <inicio> through <fin> (Inclusivo)
@for $i from 1 through 4 {
  .col-#{$i} {
    width: 25% * $i;
  }
}

// Genera: .col-1 (25%), .col-2 (50%), .col-3 (75%), .col-4 (100%)
```

### 5.4 Bucles Condicionales (`@while`)
Itera mientras una condición sea verdadera.

```scss
$tamano: 40px;
$paso: 1;

@while $tamano > 10px {
  .avatar-nivel-#{$paso} {
    width: $tamano;
    height: $tamano;
  }
  $tamano: $tamano - 10px;
  $paso: $paso + 1;
}
```

---

## 6. Anidamiento y Selector Padre (`&`)

```scss
.movie-card {
  background-color: #1e293b;
  border-radius: 8px;
  padding: 16px;

  h3 {
    color: #ffffff;
    font-size: 1.2rem;
  }

  // Modificadores o pseudoclases con el selector padre '&'
  &:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  }

  &.destacada {
    border: 2px solid #e11d48;
  }
}
```

---

## 7. Mixins Paramétricos y Funciones

### 7.1 Mixin (`@mixin` y `@include`)
Genera bloques de código reutilizable con parámetros opcionales.

```scss
@use "sass:color";

@mixin flex-centro($direccion: row, $gap: 12px) {
  display: flex;
  flex-direction: $direccion;
  justify-content: center;
  align-items: center;
  gap: $gap;
}

.galeria {
  @include flex-centro(row, 20px);
}
```

### 7.2 Funciones Personalizadas (`@function` y `@return`)
Procesan valores y devuelven un resultado computable.

```scss
@use "sass:math";

// Función para convertir px a rem (base 16px)
@function px-a-rem($px) {
  @return math.div($px, 16px) * 1rem;
}

.titulo-principal {
  font-size: px-a-rem(32px); // 2rem
}
```

---

## 8. Herencia y Extensión (`%placeholder` y `@extend`)

Los placeholders (`%`) previenen la emisión de código CSS no utilizado hasta que se extienden.

```scss
%boton-base {
  display: inline-block;
  padding: 10px 20px;
  border-radius: 6px;
  font-weight: bold;
  text-decoration: none;
  text-align: center;
}

.btn-accion {
  @extend %boton-base;
  background-color: #e11d48;
  color: white;
}

.btn-cancelar {
  @extend %boton-base;
  background-color: #64748b;
  color: white;
}
```

---

## 9. Cuadro Resumen de Directivas y Módulos

| Concepto | Directiva / Función | Ejemplo / Uso |
| :--- | :--- | :--- |
| **Importar Módulo** | `@use "<modulo>";` | `@use "sass:map";` / `@use "sass:color";` |
| **Interpolación** | `#{$variable}` | `.icono-#{$nombre} { ... }` |
| **Obtener de Mapa** | `map.get($mapa, $clave)` | `color: map.get($colores, "primario");` |
| **Obtener de Lista** | `list.nth($lista, $index)` | `font-size: list.nth($tamanos, 2);` |
| **Bucle Colección** | `@each $var in $coleccion` | `@each $clave, $valor in $mapa` |
| **Bucle Rango** | `@for $i from 1 through N` | `@for $i from 1 through 12` |
| **Condicional** | `@if / @else if / @else` | `@if $modo == "dark" { ... }` |
| **Mixin / Incluir** | `@mixin` / `@include` | `@mixin caja($w) ... @include caja(100px);` |
| **Función** | `@function` / `@return` | `@function rem($px) { @return ... }` |
| **Herencia** | `%placeholder` / `@extend` | `%base { ... } .clase { @extend %base; }` |
