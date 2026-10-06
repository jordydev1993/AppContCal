# Relevamiento — App de conteo de calorías

> Fecha: 2026-10-05 · Metodología: Spec Driven Development + Vibe Engineering
> Estado: **cerrado** (ronda inicial). Cambios posteriores se registran al final en "Decisiones / cambios".

## 1. Visión
PWA de **uso personal** para registrar lo que como, el ejercicio, el agua y el peso, con carga
rápida asistida por IA y una meta diaria calculada automáticamente.

## 2. Usuario y contexto
- Usuario: uso personal (1 persona, eventualmente unos pocos conocidos).
- Región: **Argentina** → español rioplatense, comidas típicas (milanesa, empanadas, mate, facturas…), marcas locales.
- Plataforma: **web responsive / PWA instalable** (mobile-first).

## 3. Funcionalidades relevadas

| Área | Decisión | Fase |
|---|---|---|
| Registro por **texto libre con IA** | "2 empanadas de carne y una coca" → ítems con porción, kcal y macros | **MVP** |
| Confirmación de IA | Mostrar los ítems interpretados, editables, y confirmar antes de guardar | **MVP** |
| Registro por **foto con IA** | Foto del plato → estimación de ítems | Post-MVP |
| **Buscador** de base de alimentos | Alimentos con sus valores nutricionales | Post-MVP |
| **Código de barras** | Productos envasados | Post-MVP |
| Comidas | Categorías fijas: desayuno, almuerzo, merienda, cena y snacks | MVP |
| Métricas | Calorías, macros (P/C/G), peso corporal, agua, ejercicio | MVP |
| Ejercicio | Texto libre con IA ("45 min bici moderado") → kcal estimadas según el peso | MVP |
| Meta diaria | Automática: Mifflin-St Jeor + factor de actividad + objetivo (bajar/mantener/subir) | MVP |
| Reportes | Resumen diario vs. meta; tendencia semanal/mensual (kcal y peso); comidas frecuentes/favoritas | MVP (diario) / iterativo |
| Exportación | No requerida | — |
| Notificaciones | No en el MVP | Futuro (push PWA) |
| Wearables | No | Futuro |

## 4. Restricciones no funcionales
- **Presupuesto IA: USD 0** → solo free tiers (ver riesgos).
- Datos en la **nube con login**, sincronizados entre dispositivos.
- Login con **Google**.
- Hosting gratuito.

## 5. Stack propuesto (a validar en `design.md`)
- Next.js (App Router) + TypeScript + Tailwind + shadcn/ui, PWA.
- Supabase (free tier): Postgres + Auth con Google.
- IA: AI SDK con un proveedor que tenga free tier y visión (candidato: Google Gemini Flash).
- Deploy: Vercel Hobby.

## 6. Riesgos y preguntas abiertas
1. **Free tier de IA**: tiene límites de requests por minuto y por día, y en algunos proveedores
   los datos del free tier pueden usarse para entrenar modelos. Es aceptable para uso personal, pero hay que confirmarlo.
2. **Precisión de la IA**: las estimaciones de porciones son aproximadas → por eso se confirman y editan antes de guardar.
3. **Base de alimentos argentina** (post-MVP): evaluar Open Food Facts (códigos de barras AR) y la tabla de composición ARGENFOODS.

## 7. Decisiones / cambios
- 2026-10-05: relevamiento inicial cerrado.
- 2026-10-05: aprobado el uso de Gemini Flash en free tier, aceptando sus límites y el posible uso de los datos para entrenamiento.
