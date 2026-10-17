# BJOTA Reservas: SaaS de citas, caja y reportes para barberías

**SaaS multi-negocio (un solo sistema para muchas barberías) en producción · [reservas.bjota.online](https://reservas.bjota.online) · Trujillo, Perú · 2026**

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
| **El dueño** | Aviso al celular por cada reserva; agenda por barbero (día, semana, mes, lista); confirmación por WhatsApp con un toque |
| **La caja** | Punto de venta por método de pago, devoluciones, propinas, apertura y cierre de caja, adelantos verificables |
| **Los números** | Reportes de producción por barbero, servicios y productos top, evolución diaria frente al periodo anterior |
| **La plataforma** | Muchas barberías en un solo sistema, con planes, accesos con vencimiento, invitaciones de un solo uso, auditoría y 25 videos de ayuda |

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

**Decisiones clave (explicadas simple):**
- **Cada negocio ve solo lo suyo:** uso RLS, una función de la base de datos que separa los datos de cada barbería. Además, cada rol ve lo que le toca: dueño, administrador, encargado y barbero (el barbero solo ve sus propias citas). El sistema decide a qué negocio pertenece cada usuario; no se confía en lo que envía el navegador.
- **El dinero no se puede tocar a mano:** ventas, caja y adelantos solo se registran mediante reglas que validan cada movimiento. Nadie puede escribir directo en la caja. Un script hace 60 intentos de escribir directo y comprueba que todos fallen.
- **Sin citas cruzadas:** la base de datos impide que dos citas del mismo barbero se crucen, incluso si dos clientes reservan a la vez.
- **Reserva pública protegida contra abuso:** hay un límite de reservas por teléfono, por conexión y por negocio, y cada número puede tener solo una reserva pendiente.
- **Protección del sitio web:** todo viaja por HTTPS (conexión cifrada), el navegador recibe reglas que bloquean código extraño y hay un límite de intentos para iniciar sesión.
- **Servidor propio:** el sistema corre en un VPS (un servidor Linux alquilado) con Docker, que empaqueta el sistema para que funcione igual en cualquier servidor. Nginx es la puerta de entrada al servidor. Antes estaba en servicios en la nube (Vercel y Supabase Cloud) y lo migré para controlar costos y datos.

## Calidad

- **1779 pruebas automáticas**, todas pasan: revisan solas que cada parte haga lo que debe.
- **151 cambios versionados de la base de datos**: cada cambio queda registrado y se puede repetir en orden.
- **33 scripts de verificación** que comprueban la separación de datos entre negocios, el dinero, las reservas al mismo tiempo y los permisos.
- Revisión de pantallas en el navegador, auditoría de seguridad documentada y **474 commits** (cambios guardados en el historial del proyecto).

## Estado

- ✅ **En producción** desde septiembre de 2026, con sus primeras barberías de Trujillo.
- ⏳ **Pendiente:** envío automático de correos, copias de seguridad (backups) fuera del servidor y personalización completa de colores.

## Mi rol

Es un proyecto propio de principio a fin. Definí el producto con dueños de barberías, diseñé la base de datos y la arquitectura, desarrollé la app con apoyo de asistentes de IA y escribí las pruebas. También la desplegué en un VPS y doy soporte a los negocios que la usan.

---

> 🔒 **Repositorio vitrina:** no se publica el código fuente, los datos de clientes reales ni los detalles internos del servidor. Las capturas del panel provienen de una copia local con la barbería de demostración «Estilo Norte», con datos ficticios.

**Bryan Jeremy Espinoza Herrera** · BJOTA Systems · Trujillo, Perú
