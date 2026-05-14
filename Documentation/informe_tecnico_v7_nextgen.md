# Documentación Técnica de Ingeniería: Sistema Ambiental V7.0 "Next-Gen"

**Desarrollador:** Reibeiro Velasquez  
**Especialidad:** Ingeniería de Telecomunicación 
**Fecha:** Mayo 2026

---

## 1. Arquitectura de Hardware (ESP32 Deep Dive)

El cerebro del sistema es un SoC **ESP-WROOM-32**, seleccionado por su capacidad de doble núcleo y conectividad dual (Bluetooth/WiFi).

### A. Mapeo de Periféricos (Pinout)
| Componente | Pin ESP32 | Función |
| :--- | :--- | :--- |
| **MQ-135 (Gas)** | ADC1_CH6 (GPIO 34) | Lectura analógica de PPM |
| **DHT11 (Temp/Hum)** | GPIO 4 | Protocolo digital Single-Wire |
| **HC-SR04 (Trig)** | GPIO 5 | Salida de pulso sónico |
| **HC-SR04 (Echo)** | GPIO 18 | Entrada de tiempo de retorno |
| **Puente H L298N** | GPIO 12, 13, 14, 27 | Control PWM de Motores DC |
| **Buzzer Activo** | GPIO 2 | Alerta sonora de proximidad/gas |

### B. Lógica del Firmware (Máquina de Estados)
El firmware opera bajo un loop de tiempo real con tres tareas concurrentes:
1.  **Monitorización:** Escaneo de sensores cada 500ms.
2.  **Parser de Comandos:** Interrupción serial para procesar órdenes del teléfono (F, B, L, R, S).
3.  **Lógica Autónoma:** Algoritmo de evasión de obstáculos basado en histéresis de distancia (<20cm).

---

## 2. Ingeniería de Software (Flutter Application)

La aplicación fue diseñada bajo un patrón de **Gestión de Estado Reactivo**, asegurando que la UI se actualice a 60 FPS sin bloquear el hilo principal de comunicación.

### A. Procesamiento Asíncrono de Datos (The Parser)
Uno de los mayores retos fue la reconstrucción de paquetes Bluetooth. Debido a la fragmentación, implementamos un **Buffer de Reensamblaje**:

```dart
// Algoritmo de procesamiento de flujo continuo
_connection?.input?.listen((Uint8List data) {
  _bluetoothBuffer += utf8.decode(data);
  if (_bluetoothBuffer.contains('\n')) {
    List<String> lines = _bluetoothBuffer.split('\n');
    for (var msg in lines) {
       // Parser etiquetado: T:25,H:60,CO2:450
       _processData(msg); 
    }
    _bluetoothBuffer = lines.last; // Preserva fragmentos incompletos
  }
});
```

### B. Sistema de Telemetría Dual-Stream
La ingeniería de datos separa el flujo en dos canales:
1.  **Canal Crítico (Bluetooth):** Baja latencia para control y alertas inmediatas.
2.  **Canal Histórico (ThingSpeak API):** Peticiones HTTP RESTful asíncronas para visualización de tendencias de larga duración.

### C. Patrón de Diseño UI: Neumorfismo y Neón
Para lograr el efecto **"Neon Intelligence"**, se utilizaron capas de `Stack` con filtros de desenfoque (`BackdropFilter`) y sombras dinámicas calculadas según el color del sensor.

---

## 3. Optimización y Solución de Fallos de Ingeniería de Software

### A. Gestión de Latencia y Buffer Bluetooth
*   **Problema:** La recepción de datos era errática cuando el ESP32 enviaba ráfagas rápidas de telemetría.
*   **Solución:** Se implementó una **cola de procesamiento (Queue)** mediante un buffer de strings que limpia el canal antes de parsear, asegurando que solo se procesen líneas completas (`\n`) y eliminando el ruido de paquetes incompletos.

### B. Hibridación de Datos Cloud/Local
*   **Problema:** ThingSpeak tiene una latencia de 15 segundos, lo que hacía que las gráficas se vieran "congeladas".
*   **Solución:** Ingeniería de flujo dual. La APP ahora alimenta el Dashboard en tiempo real vía Bluetooth, mientras consulta el histórico de ThingSpeak en segundo plano para las estadísticas de tendencias, logrando una interfaz que se siente instantánea.

### C. Sistema de Alertas Proactivas
*   **Implementación:** Desarrollo de una lógica de interrupción por software. Si el valor de `_gas` cruza el umbral crítico, la APP suspende las tareas visuales secundarias para dar prioridad al motor de Voz (TTS) y a la notificación visual de emergencia.

---

## 4. Conclusión Técnica
El sistema V7.0 representa una integración exitosa de robótica móvil y analítica de datos. La robustez del parser Bluetooth y la reactividad de la interfaz de usuario sitúan a este proyecto en un nivel de madurez técnica listo para aplicaciones reales de monitoreo ambiental.
