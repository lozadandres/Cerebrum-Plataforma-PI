# Cerebrum - Plataforma PI

![Cerebrum Banner](https://img.shields.io/badge/Architecture-Blazor%20Server-blue)
![Cerebrum Banner](https://img.shields.io/badge/Database-PostgreSQL-blue)
![Cerebrum Banner](https://img.shields.io/badge/Performance-Optimized-green)

Cerebrum es una plataforma empresarial de alto rendimiento diseñada para la gestión integral de procesos académicos, financieros y administrativos. Este repositorio documenta la arquitectura técnica, las optimizaciones de rendimiento y las decisiones que fundamentan la escalabilidad y eficiencia del sistema.

## **Visión General del Proyecto**

![Reporte de Países](https://portafolio-andresloz-66.vercel.app/Reportes_paises.jpeg)
![Reporte de Departamentos](https://portafolio-andresloz-66.vercel.app/reportedepartamentos.png)
![Editor de Factura](https://portafolio-andresloz-66.vercel.app/Factura_editable.jpeg)
![Kardex de Inventario](https://portafolio-andresloz-66.vercel.app/kardex.png)

El objetivo principal de Cerebrum es ofrecer una experiencia de usuario (UX) fluida y reactiva, minimizando la latencia en el procesamiento de grandes volúmenes de datos. La plataforma utiliza un stack tecnológico moderno basado en **C#**, **Blazor Server** y **PostgreSQL**, optimizando la comunicación con el motor de base de datos mediante ejecución directa con **Npgsql**.

---

## **Arquitectura Técnica y Decisiones de Diseño**

### **1. Arquitectura de Doble Consulta (Metadatos → Datos)**
Para garantizar la flexibilidad y el dinamismo de la interfaz, se implementó una capa de persistencia basada en ADO.NET + Npgsql que opera bajo un esquema de doble consulta:
- **Capa de Metadatos (DbConnection0):** Obtiene la estructura, reglas y definiciones de los formularios.
- **Capa de Datos de Negocio (DbConnection):** Recupera la información transaccional y de negocio.

Este desacoplamiento permite que el sistema sea altamente configurable sin necesidad de recompilación de código para cambios menores en la estructura de datos.

### **2. Motor de Formularios Dinámicos**
El sistema renderiza interfaces en tiempo real leyendo definiciones desde tablas relacionales en PostgreSQL:
- `formulariosparametros`: Configuración general del formulario.
- `formulariostablascampos`: Definición técnica de campos y tipos de datos.
- `formulariosreglas`: Lógica de validación y comportamiento de campos.

La generación de la UI se apoya en **Radzen Blazor Components**, asegurando consistencia visual y funcional.

### **3. Optimización de Navegación y Rendimiento**
Se realizó una reingeniería en el flujo de navegación para mejorar la percepción de velocidad:
- **Centralización de Eventos:** Migración de enlaces declarativos (`Path="..."`) a una lógica centralizada vía `NavigateTo`.
- **Control de Recarga:** Implementación de `forceLoad: true` controlado. Esta decisión técnica redujo los tiempos de carga de más de 3 segundos a un rango de **0.8s – 2.5s**, logrando una mejora de eficiencia entre el **30% y 70%**.

---

## **Componentes Crave del Sistema**

### **Radzen DataGrid & Grillas Interactivas**
Las vistas principales utilizan **Radzen DataGrid** con filtros avanzados. Actualmente, el sistema opera con carga en memoria mediante `NpgsqlDataAdapter`, pero la arquitectura está preparada para la transición a **Lazy Loading** (Server-side pagination) utilizando técnicas de `Skip-Take`, garantizando escalabilidad ante el crecimiento de la data.

### **Módulo de Kardex Avanzado**
Ubicado en [ModeloBasicoKardex.razor](file:///c:/Users/lozad\Downloads\repo\Cerebrum-Plataforma-PI\ModeloBasicoKardex.razor), este componente es el corazón logístico del sistema:
- **Consultas Complejas:** Implementación de queries SQL optimizadas para PostgreSQL.
- **Jerarquía de Datos:** Visualización estructurada de Referencia → Bodega → Movimientos.
- **Cálculos en Tiempo Real:** Algoritmos de saldos y costos integrados directamente en la lógica de negocio.

### **Generación de Reportes**
Integración robusta con **Syncfusion** (`Syncfusion.Pdf` y `Syncfusion.XlsIO`) para permitir exportaciones personalizadas de alta fidelidad desde cualquier grilla del sistema.

---

## **Retrospectiva de Ingeniería: "El Porqué de las Cosas"**

### **Gestión de Tiempos y Prioridades**
El desarrollo se centró inicialmente en la robustez de los datos. La prioridad fue asegurar que la arquitectura de metadatos fuera sólida antes de escalar la UI. Esto permitió que la implementación de nuevos módulos (como Países o Departamentos) fuera una tarea de configuración más que de codificación.

### **Retrospectiva: Si empezara de nuevo**
Si bien el uso de `forceLoad: true` resolvió problemas inmediatos de latencia por recargas múltiples, una aproximación de navegación SPA pura desde el primer día habría sido ideal. No obstante, la solución actual equilibra perfectamente la estabilidad con la velocidad requerida por el usuario final.

### **Detrás del Código: Mi Narrativa**
El mayor reto no fue solo hacer que el código funcionara, sino que fuera **rápido**. Ver la transformación de una pantalla que tardaba 4 segundos en cargar a una que responde en menos de un segundo es la mayor satisfacción técnica de este proyecto. Cerebrum no es solo una plataforma de datos; es una herramienta diseñada para la eficiencia humana.

---

## **Stack Tecnológico**
- **Frontend:** Blazor Server (C#), Radzen Components, Syncfusion.
- **Backend:** .NET 8, ADO.NET.
- **Base de Datos:** PostgreSQL con Npgsql.
- **Arquitectura:** Metadatos-Driven UI.

---
*Desarrollado con estándares de ingeniería de software de alta calidad para la plataforma Cerebrum.*
