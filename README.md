# 📚 Instrucciones de desarrollo

Este repositorio forma parte de una arquitectura modular de temas para Shopify.  
A continuación se describe el flujo de trabajo establecido:

## Estructura de repositorios y ramas

- **`base`** → Repositorio principal, fork del tema oficial **Dawn** de Shopify.
  - **`base.base`** → Rama donde se almacenan todos los **componentes y configuraciones comunes** a todos los temas hijos.
- **`natural`** → Repositorio hijo, fork de **`base.base`**.
  - **`natural.base`** → Rama principal del tema hijo **Natural**.

## Principios de trabajo

1. El repositorio **base** se utiliza únicamente para mantener componentes reutilizables y configuraciones comunes.
2. Los desarrollos específicos de cada tema hijo se realizan en su propio repositorio.
3. **No se debe conectar directamente la rama `base` con Shopify**.  
   Shopify puede modificar el código automáticamente al usar el page builder, y estos cambios no deben mezclarse con `base.base` salvo revisión controlada.

## Flujo de trabajo recomendado

1. Trabajar desde una rama alternativa (`dev`) que sirva como espejo de `base`.
2. Crear ramas de tipo **feature** para cada nuevo desarrollo (`feature/mi-nueva-funcionalidad`).
3. Conectar las ramas alternativas (`dev`, `feature/*`) a Shopify para desarrollo y pruebas.
4. Realizar **cherry-pick** selectivo desde las ramas secundarias hacia `natural.base`.
5. Realizar **pull request** desde `natural.base` hacia `base.base` para compartir mejoras comunes con el repositorio padre.

## Notas importantes

- Los cambios no revisados del editor visual de Shopify **no deben** integrarse automáticamente en `base.base`.
- Sólo deben propagarse al repositorio base los componentes o configuraciones que sean útiles para todos los temas derivados.
- Mantener el repositorio `base` lo más limpio y estable posible.
