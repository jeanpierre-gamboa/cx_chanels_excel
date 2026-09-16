# Control gerencial de CX / canales (Excel)

Tablero en Excel para responder una pregunta de negocio:

**Si un canal de atención “está mal”, ¿es por volumen, por puntualidad (SLA) o por falta de gente?**

Proyecto de portafolio. Ingeniería de Sistemas — prácticas preprofesionales en analítica / CX / reportes.

![Tablero gerencial](capturas/resumen.png)

## Qué problema resuelve

En CX y canales la operación, la encuesta y la dotación suelen vivir en archivos distintos. Sin cruzarlos es fácil equivocar la causa:

- suben los tickets y se pide más personal, cuando el problema es el proceso;
- un NPS alto se lee como “el cliente está bien”, cuando contestó una minoría;
- un SLA flojo en un canal chico se trata como falta de gente.

Este archivo junta las tres cajas (demanda, experiencia, capacidad) y deja una lectura para decidir.

## Fuentes

| Archivo | Qué es |
|---|---|
| `crudo/technical_support_nps.csv` | Tickets de soporte + NPS (dataset público) |
| `crudo/dotacion.txt` | Dotación diaria por canal (**sintética**): fecha, canal, asesores |

- Periodo de análisis: enero–octubre 2024 (se excluyeron dos fechas atípicas de 2022 y 2023).
- La región original del dataset (AMER / EMEA / APAC) se mapeó a **App / Teléfono / Agencia** como proxy de canal. En un banco serían app, banca telefónica y agencia.
- El NPS del archivo original no se usó tal cual: se recalculó (promotor 9–10, pasivo 7–8, detractor 0–6).

## Qué se hizo (Excel)

- **Power Query:** tipos, nulos, reglas de NPS y SLA, filtro de fechas, join de tickets × dotación por `fecha + canal` (izquierda).
- **Tablas dinámicas:** volumen, % de cumplimiento de SLA, NPS solo de respondientes, carga (tickets vs. promedio de asesores).
- **Hoja Resumen:** KPIs, gráficos y lectura del periodo.

No se usó DAX. El cruce es el equivalente a un `LEFT JOIN` en SQL.

## Números principales

- **20.179** tickets.
- Teléfono **52%** del volumen (10.545), App **38%** (7.727), Agencia **1.907**.
- SLA global **92%**. Agencia tiene el mayor incumplimiento (**10,3%**).
- NPS por canal (solo encuestados): App **88**, Agencia **87**, Teléfono **85**.
- Tasa de respuesta **15%** (3.063 encuestas / 20.179). El perfil de Power Query sobre las primeras 1.000 filas (~11%) no representa el total.
- Carga aproximada por asesor-día: App y Teléfono **~3,3–3,6**; Agencia **~1,4**.

## Lectura / decisiones

1. Cualquier refuerzo de capacidad debería priorizar Teléfono y App, no Agencia.
2. Agencia incumple más el SLA con menor carga por persona. Conviene revisar el proceso de ese canal antes de sumar personal.
3. El NPS 85–88 no es representativo (15% de respuesta, mayoría promotores). No usarlo para evaluar el canal ni para incentivos. Subir la tasa de respuesta o complementar con CSAT al cierre.
4. Enero–marzo tienen poco volumen; el SLA de esos meses no es comparable con el segundo semestre.

## Cómo abrir el archivo

1. Descargar `CX_Analisis_dashboard.xlsx`.
2. Colocar los crudos en `crudo/` o actualizar las rutas en Power Query.
3. Datos → Actualizar todo.
4. Revisar la hoja **Resumen**.

## Limitaciones

- La dotación es sintética (para demostrar el cruce demanda × capacidad).
- El mapeo región → canal es un proxy, no un canal real de un banco.
- El dataset público no trae motivo de contacto ni reapertura; no se calculó FCR.

## Autor

Jeanpierre Gamboa Díaz  
[LinkedIn](https://www.linkedin.com/in/jeanpierre-gamboa-diaz-86191b34a) · [GitHub](https://github.com/jeanpierre-gamboa)
