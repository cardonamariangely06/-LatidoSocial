# PRD · LatidoSocial

> Documento de requisitos del producto (Product Requirements Document).
> Complétalo en el Sprint 0 (historia H0.3) y pide la aprobación del profesor en un Issue.
> Cada requisito funcional (RF) se convierte en un Issue con la plantilla "Historia de usuario".

| Campo | Valor |
| --- | --- |
| Alumno | Mariangel Cardona |
| Proyecto n.º 32| Recaudación de Fondos |
| Sitio de referencia | GoFundMe |
| Compañero revisor | daniel-brav |
| URL desplegada | (se completa en el Sprint 5) |

## 1. Problema

Las personas, familias y organizaciones comunitarias que necesitan recaudar fondos de emergencia o financiar causas sociales dependen de cadenas informales en redes sociales y mensajería. Esto genera desconfianza en los donantes al no poder verificar el progreso real de la meta ni la legitimidad de la causa, además de dificultar al organizador el control contable y la rendición de cuentas sobre los aportes recibidos.

## 2. Usuarios

- **Usuario principal (Donante / Colaborador):** Personas solidarias que desean apoyar causas sociales, médicas o comunitarias. Necesitan explorar campañas activas, conocer cuánto dinero falta para alcanzar la meta, aportar de forma rápida con mensajes de apoyo y consultar el historial transparente de donaciones. Entran principalmente desde dispositivos móviles (smartphones).
- **Usuario secundario (Organizador / Creador de campaña):** Personas naturales o representantes comunitarios que necesitan crear una campaña solidaria, establecer una meta económica y fecha límite, publicar actualizaciones sobre el avance de la causa y gestionar los aportes recibidos. Entran tanto desde computadora como desde teléfono móvil.

## 3. Objetivo y métrica de éxito

Un donante encuentra una campaña activa, visualiza su progreso financiero y registra una donación en menos de 30 segundos, actualizando el monto recaudado y la barra de progreso en tiempo real sin errores de cálculo.

## 4. Alcance del MVP

1. Gestión de usuarios y autenticación con roles (Organizador y Donante).
2. Creación, edición, listado y filtrado por categoría de campañas solidarias (salud, educación, emergencia, comunidad).
3. Detalle público de campaña con barra de progreso en tiempo real, porcentaje recaudado, días restantes y lista de donantes.
4. Registro de donaciones vinculadas a una campaña (monto, mensaje opcional, opción de donación anónima) con actualización atómica del saldo recaudado.
5. Publicación de actualizaciones o hitos de progreso por parte del organizador dentro de su campaña.
6. Panel personal del organizador y del donante para consultar campañas creadas e historial de aportes realizados.

### Funcionalidades → carpetas de `src/` (screaming architecture)

| Funcionalidad | Carpeta |
| --- | --- |
| Usuarios y autenticación | `src/users` |
| Campañas solidarias | `src/campaigns` |
| Donaciones y aportes | `src/donations` |
| Actualizaciones de campaña | `src/updates` |
| Categorías | `src/categories` |

## 5. Fuera de alcance

- Integración con pasarelas de pago bancarias reales (Stripe, PayPal o Pago Móvil real; los pagos se simularán mediante transacciones registradas en base de datos).
- Verificación documental automatizada con inteligencia artificial o biometría (KYC).
- Sistema de mensajería privada en tiempo real tipo chat entre donantes y organizadores.
- Aplicación móvil nativa para iOS o Android.

## 6. Requisitos funcionales

| ID | Requisito | Prioridad (alta, media, baja) |
| --- | --- | --- |
| RF-01 | Registrar e iniciar sesión de usuarios diferenciando permisos entre organizador y donante. | alta |
| RF-02 | Crear y editar campañas solidarias definiendo título, descripción, categoría, meta monetaria y fecha límite. | alta |
| RF-03 | Listar y filtrar campañas activas por categoría, estado o búsqueda por palabra clave. | alta |
| RF-04 | Visualizar el detalle de una campaña con monto recaudado, porcentaje de progreso, meta y listado público de donaciones. | alta |
| RF-05 | Registrar una donación hacia una campaña activa (con monto, mensaje de apoyo y opción de anonimato) incrementando el total recaudado de forma consistente. | alta |
| RF-06 | Publicar actualizaciones de progreso en una campaña por parte de su organizador creador. | media |
| RF-07 | Cerrar automáticamente o manualmente una campaña cuando alcanza la fecha límite o es finalizada por su organizador. | media |
| RF-08 | Consultar el panel personal con el historial de donaciones realizadas por un usuario y las campañas creadas por un organizador. | baja |

## 7. Requisitos no funcionales

- Responsive, móvil primero (375 px y 1280 px).
- Lighthouse ≥ 90 en Accesibilidad y Buenas prácticas.
- 0 errores en la consola del navegador.
- Funciona con 10.000 registros sin congelarse.

## 8. Modelo de datos
 ![
 ](<Diagrama Base de Datos (Recaudacion de Fondos).png>)
 
  USUARIO ||--o{ CAMPANA : crea
  USUARIO ||--o{ DONACION : realiza
  CATEGORIA ||--o{ CAMPANA : clasifica
  CAMPANA ||--o{ DONACION : recibe
  CAMPANA ||--o{ ACTUALIZACION : tiene

  USUARIO {
    int id PK
    text nombre
    text email UK
    text password_hash
    text rol
    timestamptz creado_en
  }

  CATEGORIA {
    int id PK
    text nombre UK
    text slug UK
  }

  CAMPANA {
    int id PK
    int organizador_id FK
    int categoria_id FK
    text titulo
    text descripcion
    numeric meta_monto
    numeric recaudado_monto
    text estado
    date fecha_limite
    timestamptz creado_en
  }

  DONACION {
    int id PK
    int campana_id FK
    int donante_id FK
    numeric monto
    text mensaje
    boolean es_anonima
    timestamptz creado_en
  }

  ACTUALIZACION {
    int id PK
    int campana_id FK
    text titulo
    text contenido
    timestamptz creado_en
  }