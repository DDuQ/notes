
# Diagrama de flujo

![[Drawing 2025-09-22 23.20.17.excalidraw|800]]

# Fases del plan de trabajo

## 1 - Preparación de entorno

1. Conseguir terminal Ingenico Desk en **Semi-Integrated Mode**.
2. Configurar red LAN (Ethernet TCP) con IP fija.
3. Preparar máquina donde correrá el **Local Companion App (.NET EXE)**.
4. Habilitar entorno en **Dataverse** (tablas personalizadas).
    - Crear tabla `PaymentTransaction` con columnas:
        - Status
        - AuthCode
        - MaskedCard
        - ApprovedAt 
        - ReferenceId

>[!INFO] Recursos
>[Cómo crear reglas de enmascarado de propiedades en Dataverse (para MaskedCard)](https://youtu.be/cHmVOGu6LxQ?t=66)


## 2 – Desarrollo del Local Companion App

1. Crear proyecto en .NET (Console App).
2. Implementar configuración (`appsettings.json`) con `TerminalIp`, `Port`.
3. Implementar endpoints HTTP locales:
    - `POST /payment`
    - `POST /refund`
    - `POST /void`
4. Conectar endpoints a cliente TCP que envíe comandos al Ingenico Desk.
5. Parsear respuestas del terminal y armar las respuestas en JSON.

---

## 3 – Integración con Power Automate

1. Crear una **conexión** en Power Automate apuntando al Local Companion App.
2. Configurar acciones: `Process Payment`, `Refund`, `Void`.
3. Implementar un **flow** por cada acción:
    - Recibir parámetros necesarios.
    - Llamar al endpoint correspondiente.
    - Recibir JSON de respuesta.
    - Registrar/actualizar data en `PaymentTransaction` (Dataverse), basado en la respuesta.


>[!INFO] Recursos
>[Cómo hacer trigger de un Flow mediante un botón](https://youtu.be/N7nORoAPDK4?t=32)

---

## 4 – Integración con Dynamics 365 Customer Service

1. Añadir **botones en la Command Bar**:
    - `Process Payment`
    - `Refund`
    - `Void`    
2. Configurar que cada botón dispare el Flow correspondiente en Power Automate.

---

## 5 – Pruebas

1. Prueba de conectividad al terminal (ping).
2. Enviar transacciones de prueba con tarjeta (aprobada, declinada).
3. Validar que los datos aparezcan en la tabla `PaymentTransaction`.
4. Confirmar que Dynamics 365 muestra las transacciones asociadas al Case.

---

## 6 – Demo

1. Click en `Process Payment` (Dynamics 365).
2. Efectuar transaccion en el terminal Ingenico Desk.
3. Ver resultado reflejado en Dynamics (Dataverse).
4. Repetir con Refund y Void.

---

# Entregables

- EXE del Local Companion App.
- Conexión en Power Automate <=> Local Companion App.
- Flujos en Power Automate para Payment / Refund / Void.
- Tabla `PaymentTransaction` en Dataverse.
- Interfaz en Dynamics 365 Customer Service (botones).