# Requirements — App de conteo de calorías (MVP)

> Fuente: [00-relevamiento.md](00-relevamiento.md) · Estado: **borrador para revisión**
> Formato: historias de usuario + criterios de aceptación (Dado / Cuando / Entonces).
> Prioridad: **M** = Must (MVP) · **S** = Should (MVP si da el tiempo) · **C** = Could (post-MVP).

## Glosario
- **Registro**: un ítem guardado (alimento, ejercicio, agua o peso) con fecha.
- **Comida**: categoría fija del día → `desayuno | almuerzo | merienda | cena | snack`.
- **Meta diaria**: kcal objetivo + gramos de proteína, carbohidratos y grasas.
- **Balance**: kcal consumidas − kcal de ejercicio.
- **Día**: día calendario en la zona horaria `America/Argentina/Buenos_Aires`.

---

## RF-01 · Autenticación (M)
**Como** usuario **quiero** entrar con mi cuenta de Google **para** acceder a mis datos desde cualquier dispositivo.

- **CA-01.1** Dado que no tengo sesión, cuando abro la app, entonces veo la pantalla de login con el botón "Continuar con Google".
- **CA-01.2** Dado que me autentiqué por primera vez, entonces se crea mi perfil vacío y se me redirige al onboarding (RF-02).
- **CA-01.3** Dado que tengo sesión, cuando cierro y reabro la app, entonces sigo logueado.
- **CA-01.4** Cuando toco "Cerrar sesión", entonces vuelvo al login.
- **CA-01.5** Un usuario solo puede leer y escribir sus propios datos (se valida en la base de datos, no solo en la UI).

## RF-02 · Perfil y meta diaria automática (M)
**Como** usuario **quiero** cargar mis datos físicos y mi objetivo **para** que la app calcule mi meta de calorías y macros.

- **CA-02.1** El onboarding pide: sexo (M/F), fecha de nacimiento, altura (cm), peso actual (kg), nivel de actividad y objetivo.
  - Actividad: sedentario (1.2), ligero (1.375), moderado (1.55), alto (1.725), muy alto (1.9).
  - Objetivo: bajar (−500 kcal), mantener (0), subir (+300 kcal).
- **CA-02.2** El TMB se calcula con Mifflin-St Jeor:
  - Hombre: `10·peso + 6.25·altura − 5·edad + 5`
  - Mujer: `10·peso + 6.25·altura − 5·edad − 161`
  - Meta kcal = `round(TMB × factor_actividad + ajuste_objetivo)`, con un mínimo de 1200 kcal.
- **CA-02.3** Macros por defecto: proteína 1.8 g/kg de peso, grasas 25 % de las kcal y carbohidratos el resto.
- **CA-02.4** Dado un perfil válido, cuando guardo, entonces veo la meta calculada y llego al dashboard.
- **CA-02.5** Puedo editar el perfil después; la meta se recalcula y aplica desde ese día en adelante (los días pasados conservan su meta).
- **CA-02.6** Validaciones: altura de 100 a 250 cm, peso de 30 a 300 kg y edad de 14 a 100 años; si un dato está fuera de rango, se muestra un error en ese campo.

## RF-03 · Registro de comida por texto con IA (M)
**Como** usuario **quiero** escribir lo que comí en lenguaje natural **para** registrarlo en segundos.

- **CA-03.1** Desde el dashboard, toco "+ Comida", elijo la comida (se preselecciona según la hora: <11 h desayuno, <15 h almuerzo, <19 h merienda, <23 h cena, si no snack) y escribo un texto libre.
- **CA-03.2** Cuando envío el texto, entonces la IA devuelve una lista de ítems con nombre, cantidad, unidad, gramos estimados, kcal, proteína, carbohidratos y grasas.
- **CA-03.3** La IA entiende comidas y porciones argentinas ("una milanesa napolitana", "3 facturas", "un mate cocido con leche", "porción de muzza").
- **CA-03.4** Mientras la IA procesa, veo un indicador de carga; la respuesta tarda menos de 10 s en el p95.
- **CA-03.5** Si el texto no contiene comida reconocible, entonces veo el mensaje "No pude identificar alimentos, probá reformularlo" y no se guarda nada.
- **CA-03.6** Si la IA falla o se alcanzó el límite del free tier, entonces veo un error claro y la opción de cargar los ítems a mano (RF-05).

## RF-04 · Confirmación y edición antes de guardar (M)
**Como** usuario **quiero** revisar lo que interpretó la IA **para** corregir errores antes de que cuenten.

- **CA-04.1** Los ítems interpretados se muestran en una pantalla de revisión con el total de kcal y macros.
- **CA-04.2** Puedo editar el nombre, los gramos y los valores de cada ítem. Si cambio los gramos, las kcal y los macros se recalculan de forma proporcional.
- **CA-04.3** Puedo eliminar un ítem o agregar uno manual.
- **CA-04.4** Cuando toco "Guardar", entonces los ítems se registran en la comida y la fecha elegidas, y el dashboard se actualiza.
- **CA-04.5** Si toco "Cancelar", no se guarda nada.

## RF-05 · Carga manual de un ítem (M)
- **CA-05.1** Puedo agregar un ítem escribiendo el nombre, los gramos (opcional), las kcal y, opcionalmente, P/C/G.

## RF-06 · Gestión de registros del día (M)
- **CA-06.1** En el dashboard veo los registros agrupados por comida, con el subtotal de kcal de cada una.
- **CA-06.2** Puedo editar o eliminar cualquier registro.
- **CA-06.3** Puedo navegar a días anteriores o posteriores y cargar registros en cualquier fecha (no en fechas futuras).

## RF-07 · Ejercicio por texto con IA (M)
**Como** usuario **quiero** escribir qué ejercicio hice **para** que se descuenten las calorías quemadas.

- **CA-07.1** Escribo, por ejemplo, "45 min de bici moderado" y la IA devuelve la actividad, la duración (min) y las kcal estimadas usando mi peso actual (METs).
- **CA-07.2** Reviso y edito el resultado antes de guardar (igual que en RF-04).
- **CA-07.3** Las kcal de ejercicio se restan en el balance del día; **la meta no cambia**.

## RF-08 · Agua (M)
- **CA-08.1** En el dashboard veo un contador de agua con botones rápidos (+250 ml, +500 ml) y la opción de deshacer el último.
- **CA-08.2** La meta de agua por defecto es 35 ml/kg de peso, editable en el perfil.

## RF-09 · Peso corporal (M)
- **CA-09.1** Puedo registrar mi peso (kg, con 1 decimal) para una fecha; si ya hay un registro ese día, se reemplaza.
- **CA-09.2** El último peso registrado actualiza el peso del perfil y recalcula la meta (según CA-02.5).

## RF-10 · Dashboard / resumen diario (M)
- **CA-10.1** Veo un anillo de kcal: consumidas, ejercicio, restantes = meta − consumidas + ejercicio.
- **CA-10.2** Veo barras de proteína, carbohidratos y grasas contra la meta.
- **CA-10.3** Si las kcal consumidas superan la meta, el anillo lo indica visualmente (color de alerta).
- **CA-10.4** Veo el agua del día y el último peso.

## RF-11 · Tendencias (S)
- **CA-11.1** Hay una vista con un selector 7 / 30 / 90 días.
- **CA-11.2** Gráfico de barras de kcal consumidas por día, con una línea de la meta.
- **CA-11.3** Gráfico de línea de la evolución del peso.
- **CA-11.4** Promedios del período: kcal/día y macros/día.

## RF-12 · Comidas frecuentes y favoritas (S)
- **CA-12.1** Puedo marcar como favorito un conjunto de ítems ya guardado (por ejemplo, "Desayuno habitual").
- **CA-12.2** Al agregar una comida, veo mis favoritos y los 5 ítems más registrados en los últimos 30 días; con un toque los agrego a la revisión (RF-04) sin llamar a la IA.

## RF-13 · PWA (M)
- **CA-13.1** La app es instalable (manifest + íconos + service worker).
- **CA-13.2** Sin conexión, la app abre y muestra un mensaje de "sin conexión"; en el MVP no se requiere registrar offline.

---

## Requisitos no funcionales
| ID | Requisito |
|---|---|
| RNF-01 | Mobile-first; se usa con una mano en pantallas de 360 px de ancho o más; tema claro/oscuro. |
| RNF-02 | Idioma: español rioplatense (voseo). Unidades métricas. Zona horaria AR. |
| RNF-03 | Costo de operación USD 0: Vercel Hobby, Supabase free, Gemini free tier. |
| RNF-04 | La API key de la IA solo vive en el servidor; nunca se expone en el cliente. |
| RNF-05 | Row Level Security en todas las tablas. |
| RNF-06 | Rate limit propio de la IA: máximo 30 pedidos/día por usuario, para no agotar el free tier. |
| RNF-07 | Las salidas de la IA se validan con un schema (Zod); si la respuesta es inválida, se trata como error (CA-03.6). |
| RNF-08 | Accesibilidad: contraste AA, labels en inputs y navegación por teclado. |
| RNF-09 | Tests: las funciones de cálculo (TMB, macros, balance, METs) tienen tests unitarios. |

## Fuera de alcance (MVP)
Foto con IA · buscador de base de alimentos · código de barras · notificaciones · wearables · exportación · multiusuario/nutricionista · registro offline.
