# BJOTA Reservas: SaaS de citas, caja y reportes para barberías

**SaaS multi-negocio en producción · [reservas.bjota.online](https://reservas.bjota.online) · Trujillo, Perú · 2026**

Cada barbería obtiene **su propia página de reservas** y un **panel para el celular** con agenda por barbero, punto de venta y caja, adelantos por Yape y reportes de cuánto produjo cada barbero.

![Next.js](https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-RLS-4169E1?logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-autoalojado-3FCF8E?logo=supabase&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)
![Tests](https://img.shields.io/badge/pruebas-1779%2F1779%20pasan-15803D)

🌐 **[Ver la barbería de demostración en vivo](https://reservas.bjota.online/estilo-norte)** · 📄 **[Caso de estudio completo (PDF)](docs/caso-de-estudio.pdf)**

![Landing](img/01-landing.png)

---

## El problema

Las barberías pequeñas (2 a 5 barberos) agendan por WhatsApp, cobran en efectivo, Yape o Plin y anotan en un cuaderno:

- **Contestar WhatsApp mientras cortan:** las citas se pierden o se cruzan.
- **Clientes que no llegan:** sin confirmación ni adelanto, una hora vacía es dinero perdido.
- **No saber cuánto hizo cada barbero** ni por qué método entró el dinero.
- **La caja no cuadra** al cerrar el día.

## La solución

| Para | Qué hace |
|---|---|
| **El cliente** | Página de la barbería con fotos, promociones y equipo; reserva en 5 pasos **sin crear cuenta**; adelanto por Yape o Plin |
| **El dueño** | Aviso push al celular por cada reserva; agenda por barbero (día, semana, mes, lista); confirmación por WhatsApp con un toque |
| **La caja** | Punto de venta por método de pago, devoluciones, propinas, apertura y cierre de caja, adelantos verificables |
| **Los números** | Reportes de producción por barbero, servicios y productos top, evolución diaria frente al periodo anterior |
| **La plataforma** | Multi-negocio con planes, accesos con vencimiento, invitaciones de un solo uso, auditoría y 25 videos de ayuda |

## Reservar en 5 pasos

| Servicios | Barbero | Día | Hora | Datos |
|---|---|---|---|---|
| ![](img/03-reserva-servicios.png) | ![](img/04-reserva-profesional.png) | ![](img/05-reserva-dia.png) | ![](img/06-reserva-hora.png) | ![](img/07-reserva-datos.png) |

## El panel del negocio

![Agenda del día](img/10-agenda-dia.png)

| Ventas por método de pago | Reportes por barbero |
|---|---|
| ![Ventas](img/13-ventas.png) | ![Reportes](img/12-reportes.png) |

| Agenda semanal | Cierre de caja | En el celular |
|---|---|---|
| ![Semana](img/11-agenda-semana.png) | ![Caja](img/17-caja.png) | ![Móvil](img/18-agenda-movil.png) |

## Arquitectura

![Arquitectura](img/arquitectura.png)

**Decisiones clave:**
- **Seguridad en la base de datos:** Row Level Security por negocio y por rol (dueño, administrador, encargado y barbero; el barbero solo ve sus citas). El id del negocio nunca se acepta del cliente.
- **El dinero solo se escribe por funciones RPC:** ventas, caja y adelantos bloquean la escritura directa, y un script comprueba que 60 intentos fallen.
- **Sin citas cruzadas:** una restricción `EXCLUDE` con `btree_gist` impide en PostgreSQL que dos citas del mismo barbero se solapen.
- **Reserva pública protegida:** límites por teléfono, por IP y por negocio, y una reserva pendiente por número.
- **Cabeceras de seguridad:** CSP con nonce y `strict-dynamic`, HSTS y límite de intentos de login en la app y en Nginx.
- **Autoalojado:** VPS Linux con Docker Compose (Supabase, Next.js en build standalone y Nginx), migrado desde Vercel y Supabase Cloud.

## Calidad

- **1779 pruebas automáticas** con `node --test`, todas pasan
- **151 migraciones** versionadas
- **33 scripts SQL** que verifican RLS, dinero, concurrencia y candados de acceso
- QA en navegador con Chrome headless, auditoría de seguridad documentada y **474 commits**

## Estado

- ✅ **En producción** desde septiembre de 2026, con sus primeras barberías de Trujillo.
- ⏳ **Pendiente:** correo transaccional (SMTP), copias de seguridad fuera del servidor y personalización completa de colores.

## Mi rol

Es un proyecto propio de principio a fin. Definí el producto con dueños de barberías, diseñé la base de datos y la arquitectura, desarrollé la app con apoyo de asistentes de IA y escribí las pruebas. También la desplegué en un VPS y doy soporte a los negocios que la usan.

---

> 🔒 **Repositorio vitrina:** no se publica el código fuente, los datos de clientes reales ni los detalles internos del servidor. Las capturas del panel provienen de una copia local con la barbería de demostración «Estilo Norte», con datos ficticios.

**Bryan Jeremy Espinoza Herrera** · BJOTA Systems · Trujillo, Perú
