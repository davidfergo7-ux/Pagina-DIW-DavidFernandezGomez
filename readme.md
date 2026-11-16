# Página Web Diseño de Interfaces Web (2º DAW)
Repositorio de recursos, apuntes y prácticas de la asignatura, diseñado en Figma con estética **Frutiger Aero / Windows Vista/XP** y maquetado con HTML5 semántico y CSS3.

## 🌐 Enlace a la web publicada
* **GitHub Pages:** [https://davidfergo7-ux.github.io/Pagina-DIW-DavidFernandezGomez/](https://davidfergo7-ux.github.io/Pagina-DIW-DavidFernandezGomez/)

---

## 🪄 Justificación cambios después del prototipo
El diseño html final presenta ciertas mejoras frente al diseño original en Figma para tener una interfaz visualmente mejor. Resumen de los cambios aplicados:
* **Cambio en el encabezado:** Ya no se encuentra la barra de búsqueda, que era demasiado complicada de aplicar y tenía poca utilidad. Se han opciones nuevas al menú superior para que cada menú corresponda a un tema. Además se ha eliminado el menú hamburguesa en la versión de PC.
* **Cambio en el inicio:** Al ser 6 temas en vez de 8 es más fácil de dividir las columnas en 3 en versión PC, en 2 en versión tablet y 1 en versión móvil.
* **Cambio en los temas:** En Figma no tenía claro qué distribución necesitaría, así que hice una página bastante genérica. Pero la nueva distribución es muy intuitiva y sencilla.
* **Cambio en el pie de página:** En Figma no añadí ningún pie de página. En la página oficial he añadido un pie sencillo.
* **Cambio en los puntos de ruptura:** Ahora se cuenta con 3 versiones de la misma página, mientras que antes eran 2. Se ha añadido una versión intermedia orientada a tablets y dispositivos medianos. 
---

## 📐 Justificación técnica: CSS Grid vs. Flexbox

Para la maquetación de las vistas se han combinado **Flexbox** y **CSS Grid** en función de la dimensionalidad y comportamiento requerido por cada componente:

* **Barra superior y navegación (`<header class="barraSuperior">` y `<nav>` / `.listaMenu`):**  
  Se ha empleado **Flexbox** (`display: flex`) porque los elementos (logotipo de inicio y botones de navegación) se organizan en un único eje unidimensional (horizontal en escritorio). Esto facilita alinear verticalmente el icono con los enlaces (`align-items: center`), controlar el espacio entre botones (`gap: 10px`) y gestionar el crecimiento flexible de la sección izquierda (`flex-grow: 1`).
* **Catálogo de Unidades de Trabajo (`.cuadriculaTemas` en `index.html`):**  
  Se ha utilizado **CSS Grid** (`display: grid; grid-template-columns: repeat(3, 1fr)`) al tratarse de una distribución bidimensional de tarjetas en filas y columnas. Grid permite mantener proporciones uniformes entre todas las tarjetas (`gap: 30px`) y reorganizar el número de columnas de forma limpia según el ancho de pantalla sin alterar el flujo HTML.
* **Interior de cada tarjeta (`.itemTemas`):**  
  Se ha aplicado **Flexbox** en dirección vertical (`flex-direction: column`) para apilar de forma ordenada la miniatura (`16:9`) sobre el título descriptivo de cada unidad.
* **Pie de página (`<footer>`):**  
  Se ha estructurado con **Flexbox** (`flex-direction: row; justify-content: space-between`) para distribuir los tres bloques informativos (módulo, autoría y curso académico) equitativamente a lo largo del eje horizontal.

---

## 📱 Diseño Responsive (Puntos de ruptura)

Se han definido **dos puntos de ruptura (`@media`)** en la hoja de estilos (`frutiger.css`) para adaptar la interfaz a distintos tamaños de pantalla:

1. **Tablets / Pantallas medianas — `@media (max-width: 1024px)`:**
   * La cuadrícula de unidades (`.cuadriculaTemas`) pasa de **3 columnas a 2 columnas** (`repeat(2, 1fr)`) para evitar que las tarjetas se compriman en exceso.
   * Se habilita `flex-wrap: wrap` en `.barraSuperior` para evitar desbordamientos horizontales de la botonera.

2. **Dispositivos móviles — `@media (max-width: 768px)`:**
   * La cuadrícula de unidades pasa a **1 sola columna** (`grid-template-columns: 1fr`) y se reducen los márgenes exteriores (`padding: 10px`) para maximizar el área de lectura.
   * Se oculta la lista de enlaces de escritorio (`.elementoEscritorio`) y se muestra el **menú hamburguesa** (`.botonHamburguesa`), desplegando la navegación en columna mediante JavaScript (`.listaMenu.desplegado`).
   * Los botones de visualización y descarga de PDFs (`.botonPdf`, `.descargar`) y la imagen de portada (`.imagenPortada`) pasan a ocupar el **100% del ancho** disponible para mejorar la usabilidad táctil.
   * El `<footer>` activa `flex-flow: wrap` para apilar sus bloques sin superposiciones.