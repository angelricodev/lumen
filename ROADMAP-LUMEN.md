# Roadmap — LUMEN

> Asistente de voz para el hogar tipo Jarvis. Detecta mi llegada y me saluda con el clima y las noticias, con conversación por voz. Local-first y 100% gratis.

**Proyecto:** personal · aprendizaje · portafolio
**Última actualización:** 2026-06-21
**Versión del roadmap:** 1.0

> Nota: el nombre "LUMEN" es provisional. Si lo cambio, es un buscar-y-reemplazar en este archivo, el repo y la palabra de activación.

---

## 1. Objetivo

Construir a LUMEN: un asistente de voz para la casa que, cuando llego, me saluda con el clima del momento y las 3 noticias de tecnología más interesantes, y con el que puedo hablar por voz ("Lumen, ..."). El objetivo doble es **aprender** (poder explicar cada decisión y cada línea) y dejar una **pieza fuerte de portafolio**.

## 2. Principios

- **Todo gratis.** Siempre open source y free-tiers. Si algo cuesta, se busca la alternativa libre.
- **Local-first.** Lo que se pueda resolver en el equipo (presencia, comandos simples, voz) corre local; solo lo pesado se delega a la nube.
- **Aprender haciendo.** El *por qué* antes del *cómo*. Una cosa a la vez.
- **Prácticas senior.** Commits, ramas, documentación y tablero como en un proyecto real.
- **Aislado de AquaCES.** Repo, contexto y entorno propios. Cero mezcla.

## 3. Arquitectura y stack

| Capa | Herramienta | Dónde corre |
|------|-------------|-------------|
| Orquestador | Home Assistant (Docker) | Equipo (edge) |
| Palabra de activación | openWakeWord | Equipo |
| Voz a texto (STT) | Whisper | Equipo |
| Texto a voz (TTS) | Piper o TTS free-tier (voz femenina) | Equipo / nube |
| Cerebro (LLM) | Free-tier en la nube (ej. Groq / Gemini) | Nube |
| Datos | APIs gratuitas de clima y noticias | Nube |
| Dashboard | React 18 + TypeScript + Vite | Equipo |
| Historial | PostgreSQL | Equipo |

**Decisión clave (edge vs nube):** el equipo de producción es un PC viejo con ~4 GB de RAM, así que **no corre un LLM local**. El "cerebro" se delega a un free-tier en la nube; todo lo demás corre en el equipo. Esta separación es deliberada y se documenta como ADR.

**Entornos:** el desarrollo se hace en mi PC del día a día; el despliegue final va al PC viejo, siempre encendido.

## 4. Prácticas de trabajo

- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `refactor:`, `chore:`...), atómicos (un cambio lógico por commit).
- **Ramas:** `main` estable; trabajo en ramas `feat/...`; Pull Requests aunque trabaje solo.
- **Issues:** cada tarea es un issue; los commits/PRs se enlazan (`closes #N`).
- **Tablero:** GitHub Projects + Issues (público, parte del portafolio).
- **Documentación en el repo:** `README.md` como puerta de entrada, ADRs para decisiones clave, carpeta `/docs` para el resto.

## 5. Sprints

> Orientados a objetivo, no a fechas. Un sprint está "listo" cuando cumple su meta. Jalo el siguiente cuando tenga tiempo.

### Sprint 0 — Cimientos
- **Objetivo:** dejar el andamiaje listo y validar el pipeline de extremo a extremo con un "hola mundo".
- **Entregables:** repo en GitHub (README, `.gitignore`, licencia, primer commit); tablero GitHub Projects + Issues; Home Assistant corriendo en Docker (puerto y nombre de proyecto propios); una automatización mínima que dispare una notificación.
- **Aprendo:** estructura de repo, GitHub Projects/Issues, Docker para HA, flujo Git (ramas + PR), automatizaciones básicas.
- **Listo cuando:** HA levanta en el navegador, la automatización "hola mundo" dispara una notificación, el repo tiene su primer commit y el board tiene los issues del sprint.

### Sprint 1 — Presencia + primera voz
- **Objetivo:** que LUMEN detecte mi llegada y diga un saludo básico por voz.
- **Entregables:** geofencing con la app Companion (zona "Casa"); Piper (TTS) configurado; automatización: al entrar a zona Casa → frase fija por el parlante.
- **Aprendo:** zonas y geofencing, eventos y disparadores, pipeline de audio (TTS → parlante), depuración de automatizaciones.
- **Listo cuando:** al llegar a casa (o simular el evento) suena un "Bienvenido..." por el parlante.

### Sprint 2 — Datos reales (clima + noticias)
- **Objetivo:** que el saludo incluya el clima actual y las 3 noticias de tecnología, con datos reales.
- **Entregables:** integración de clima (OpenWeatherMap free); fuente de noticias (RSS o NewsAPI free); mi primer componente custom (script/sensor que arma las 3 noticias).
- **Aprendo:** consumo de APIs, manejo de secrets/keys, plantillas (Jinja) en HA, escribir un componente propio en Python.
- **Listo cuando:** el saludo dice el clima actual + 3 titulares reales de tecnología.

### Sprint 3 — El cerebro (LLM) + voz natural
- **Objetivo:** que el saludo sea natural y variado (no plantilla), generado por un LLM, con buena voz.
- **Entregables:** integración con un LLM free-tier; prompt que arma el saludo a partir de clima + noticias + hora; voz femenina afinada.
- **Aprendo:** integrar un LLM por API, diseño de prompts, manejo de latencia y límites del free-tier, separación edge/nube en la práctica.
- **Listo cuando:** cada llegada produce un saludo natural distinto y fluido, con buena voz.

### Sprint 4 — Conversación por voz
- **Objetivo:** poder hablarle ("Lumen, ...") y que responda.
- **Entregables:** openWakeWord con la palabra "Lumen"; Whisper (STT); pipeline de Assist completo; el LLM respondiendo preguntas; comandos simples resueltos en local para fluidez.
- **Aprendo:** wake word, STT, el pipeline de voz completo, presupuesto de latencia, local-first.
- **Listo cuando:** digo "Lumen, ¿qué clima hace?" y responde por voz en pocos segundos.

### Sprint 5 — Dashboard en React/TS
- **Objetivo:** un panel web propio que muestre estado, historial, clima y noticias.
- **Entregables:** app React 18 + TS + Vite conectada a la API de Home Assistant; vistas de estado/eventos/clima/noticias; historial de eventos persistido en PostgreSQL.
- **Aprendo:** consumir la API/websocket de HA, frontend desde cero, persistencia, integrar front con el asistente.
- **Listo cuando:** abro el dashboard y veo en vivo el estado de LUMEN y el historial de saludos/eventos.

### Sprint 6 — Pulido, documentación y despliegue
- **Objetivo:** dejar LUMEN listo para portafolio y corriendo 24/7 en el PC viejo.
- **Entregables:** README pulido (con GIF/demo y diagrama de arquitectura); ADRs de las decisiones clave; carpeta `/docs`; video demo de 30–60 s; despliegue al PC viejo siempre encendido.
- **Aprendo:** documentación de nivel profesional, ADRs, despliegue, preparar un proyecto para mostrarlo.
- **Listo cuando:** el repo cuenta la historia solo, hay video demo y LUMEN corre solo en el equipo de producción.

## 6. Backlog de funciones extra

Funciones gratis para jalar cuando quiera, una vez el núcleo esté firme:

- Multi-usuario: saludo distinto para mí y para Mónica.
- Briefing de Google Calendar ("hoy tienes 3 reuniones").
- Recordatorio de paraguas si va a llover.
- Rutina de "buenos días" aparte de la de llegada.
- Control de luces / enchufes inteligentes.
- Modos de voz según la hora o el clima.
- Resumen de tráfico / ruta.
- Recordatorios y temporizadores por voz.
- Despedida al salir de casa ("hasta luego").

## 7. Estado actual

- [x] Sprint 0 — Cimientos
- [ ] Sprint 1 — Presencia + primera voz
- [ ] Sprint 2 — Datos reales
- [ ] Sprint 3 — Cerebro + voz natural
- [ ] Sprint 4 — Conversación por voz
- [ ] Sprint 5 — Dashboard
- [ ] Sprint 6 — Pulido y despliegue
