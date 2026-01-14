# Product Requirement Document (PRD): tudu

**Estado**: Draft / Iteración 1
**Fecha**: 2026-01-14

## 1️⃣ Problema real

**"El contexto switch mata el flujo"**

Para desarrolladores, sysadmins y usuarios avanzados de terminal, salir de la línea de comandos para gestionar tareas en una aplicación web (Jira, Trello, Todoist) o una GUI pesada es doloroso. Rompe el estado de flujo (flow).

Por otro lado, los archivos de texto plano (`todo.txt`) se quedan cortos rápidamente:
*   No manejan jerarquías (subtareas de subtareas) de forma nativa y limpia.
*   Es difícil visualizar qué está pendiente vs. qué está hecho sin borrarlo.
*   No hay fechas de entrega ni prioridades estructuradas.

**A quién le duele:**
*   Al desarrollador que vive en la terminal y usa Vim/Emacs.
*   Al sysadmin que necesita organizar pasos de una migración compleja sin salir de SSH.
*   A los usuarios que se encargan de mantener paquetes linux y necesitan organizar proyectos grandes desglosándolos en pasos pequeños sin perder la estructura.

## 2️⃣ Usuario objetivo (cliente cero)

*   **Perfil**: Desarrollador de software, DevOps, Site Reliability Engineer (SRE).
*   **Contexto**: Trabaja >80% del tiempo en una terminal (Linux/macOS/WSL).
*   **Comportamiento actual**:
    *   Usa archivos `.txt` o `.md` dispersos que terminan desactualizados.
    *   Usa comentarios `// TODO` en el código que nunca revisa.
    *   Usa herramientas complejas como Org-Mode pero siente que "es demasiado" solo para una lista de tareas rápida.
    *   Usa Taskwarrior pero le cuesta visualizar la jerarquía del proyecto.

## 3️⃣ Solución propuesta

**tudu**: Un gestor de listas de tareas **jerárquicas** para la terminal (TUI - Text User Interface).

**Qué hace:**
*   **Interfaz Visual en Terminal**: Usa `ncurses` para dibujar una interfaz rápida y navegable con teclado (estilo Vim).
*   **Jerarquía Infinita**: Permite crear tareas, subtareas, subtareas de subtareas, etc. Ideal para desglosar "features" en "tickets" y luego en "pasos de implementación".
*   **Gestión de Estado**: Marcar como pendiente/hecho, colapsar/expandir ramas del árbol.
*   **Fechas y Prioridades**: Asignar deadlines y niveles de importancia.
*   **Persistencia Simple**: Guarda todo en un archivo XML (o similar) portable.

**Qué NO hace:**
*   No es colaborativo (no hay "nube" ni sync multi-usuario nativo en el MVP).
*   No es un gestor de proyectos visual (no hay diagramas de Gantt gráficos).
*   No tiene interfaz web ni móvil.

**Diferenciador clave:**
La combinación de **visualización de árbol (jerarquía)** + **velocidad de terminal**. La mayoría de CLIs son basados en comandos (Taskwarrior), no en interfaz visual navegable.

## 4️⃣ Competencia / alternativas

| Herramienta | Qué hacen bien | Qué hacen mal (para nuestro caso) |
| :--- | :--- | :--- |
| **Taskwarrior** | Potentísimo motor de filtrado y gestión de fechas. | Curva de aprendizaje alta. La visualización jerárquica no es su fuerte nativo (es más lista plana con tags). |
| **Org-Mode (Emacs)** | El estándar de oro en organización jerárquica. | Requiere aprender Emacs. Es un ecosistema, no una herramienta ligera. Overkill para muchos. |
| **todo.txt** | Simplicidad universal. | Cero soporte nativo para jerarquías visuales. |
| **Aplicaciones Web (Todoist, Notion)** | UX moderna, sync, colaboración. | Requieren navegador, mouse, y salir del contexto de desarrollo. Lentas comparadas con TUI. |

## 5️⃣ Alcance inicial (MVP - Adopción)

Dado que es una adopción de código legacy, el MVP se define como "Restaurar y Modernizar".

**Funcionalidades imprescindibles (Must Haves):**
*   Compilación exitosa en sistemas modernos (Linux/WSL) sin errores de dependencias obsoletas.
*   Navegación completa con teclado (flechas + vi-bindings: j, k, h, l).
*   CRUD de tareas: Crear, Leer, Editar, Borrar.
*   Jerarquía: Poder anidar tareas y colapsar/expandir nodos.
*   Persistencia: Guardar y cargar archivos sin corrupción.

**Funcionalidades deseables (Nice to have - Next Steps):**
*   Soporte de mouse básico.
*   Temas de colores configurables fácilmente.
*   Exportación a Markdown/JSON.

**Fuera de scope (Explicit):**
*   Sincronización en la nube (backend server).
*   Integración con calendarios (Google Calendar).
*   Versión GUI (Qt/GTK).

## 6️⃣ Uso de IA (Crítico y Responsable)

**Dónde la IA aporta leverage:**
*   **Refactorización del Legacy**: Explicar y refactorizar código C++ antiguo a estándares modernos (C++17/20).
*   **Generación de Tests**: Crear tests unitarios para la lógica de negocio (parser, data structures) que hoy no existen.
*   **Documentación**: Generar manuales de uso y man pages a partir del código, y contribuir con traducciones.

**Dónde NO usar IA:**
*   **Lógica Core de Persistencia**: No dejar que la IA reescriba el formato de guardado sin supervisión estricta (riesgo de perder datos de usuarios antiguos).
*   **Decisiones de UX**: La "sensación" de la navegación TUI debe ser probada por humanos.

**Riesgos:**
*   Alucinaciones en la refactorización que introduzcan bugs sutiles de gestión de memoria en C++.

## 7️⃣ Riesgos técnicos y preguntas abiertas

*   **Dependencia Ncurses**: ¿Es `ncurses` lo suficientemente portable hoy en día o deberíamos considerar alternativas modernas como `ftxui` (C++) o reescribir en Rust/Go con `tui-rs`/`bubbletea` en el futuro lejano? *Decisión: Quedarse en C++/ncurses por ahora.*
*   **Deuda Técnica**: El código es antiguo. ¿Hay fugas de memoria? ¿Es seguro ante overflows?
*   **Formato de Archivo**: ¿El XML actual es eficiente? ¿Deberíamos migrar a JSON o YAML para hacerlo más "humano" y versionable?

## 8️⃣ Iteración y Referencias

**Repositorio:** `hks-bornemann/tudu` (Fork)
**Referencias:**
*   Repo original: `meskio/tudu`
*   Inspiración: `vim`, `org-mode`

---
*Este documento es vivo. Se iterará tras probar la compilación en entornos CI/CD y recibir feedback de los primeros usuarios de la versión "Revived".*
