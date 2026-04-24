# Design & Decision Record (DDR) / Entregable 1
GreenField Technologies | IoT Systems Design

**Team Members:**
1. Mateo Alexander Cárdenas Cuesta
2. Juan David Romero Calderón

---

## 1. System Overview

- System Type: Component

- Description:
ESP32-C6 con radio IEEE 802.15.4 usado para medir RSSI y PER.
El alcance confiable obtenido fue aproximadamente 10 m.

---

## 2. Lab 1: RF Characterization

### To Samuel (Architect)
Se midió RSSI y PER a diferentes distancias.
El RSSI disminuye desde -40 dBm (1 m) hasta -96 dBm (30 m).
La pérdida de paquetes empieza a ser significativa (>1%) a partir de 10 m.

### To Edwin (Operations)
El sistema funciona bien hasta 5–10 m.
Después de esa distancia, la señal baja y se pierden mensajes.
Se recomienda instalar los dispositivos a menos de 10 m.

---

## 3. ADR-001: Channel Selection

- Context:
Se realizó un escaneo de energía en los canales.

- Decision:
Se seleccionó el canal 22.

- Rationale:
Presenta menor ruido (~ -106 dBm), lo que mejora la comunicación.

- Status: Accepted

---

## 4. ISO Mapping

| Component | Domain |
|----------|--------|
| ESP32-C6 | SCD |
| Radio 802.15.4 | SCD |
| Antena | PED |
| Aire | PED |

---

## 5. First Principles

1. La señal disminuye con la distancia.
2. A partir de ~ -90 dBm la comunicación falla.

---

## 6. Performance

| Metric | Target | Measured |
|--------|--------|----------|
| Max Range | >20 m | 10 m |

---

## 7. Results Table

| Distancia (m) | RSSI A->B | RSSI B->A | PER A->B | PER B->A |
|--------------|-----------|-----------|----------|----------|
| 1 | -40 | -51.6 | 0 | 0 |
| 5 | -64.4 | -74.8 | 0 | 0 |
| 10 | -90.8 | -85.6 | 2 | 0 |
| 20 | -92.6 | -87.4 | 2 | 1 |
| 30 | -96.2 | -89.2 | 9 | 2 |

---

## Conclusion

El RSSI disminuye con la distancia y el PER aumenta.
El alcance confiable del sistema es aproximadamente 10 m.


# GreenField SoilSense - Informe de desempeño del Laboratorio 1 / Entregable 2

## Objetivo
Probar si la radio ESP32-C6 funciona para redes de sensores agrícolas

---

## Hallazgos clave

✅ Rango máximo fiable: 10 m  
(pérdida de paquetes < 1% cuando RSSI > -90 dBm)

✅ Espaciado recomendado de sensores: 8 m  
(incluye margen de seguridad para vegetación/obstáculos)

✅ Mejor canal de radio: 802.15.4 Canal 22  
(menor interferencia: -106 dBm ruido de fondo)

⚠️ Riesgo: Interferencia WiFi detectada en canales 18 y 26  
(evitar desplegar sensores cerca de estos)

---

## Impacto en el producto

Para un campo agrícola de 10 hectáreas:  
≈ 1,560 nodos sensores  

Coste estimado de hardware:  
1,560 nodos × $40/nodo = $62,400  

---

## Recomendación

⚠️ Necesito más pruebas  

- El alcance es limitado (≈10 m), lo que incrementa el número de nodos  
- Se debe evaluar rendimiento en condiciones reales (vegetación, humedad)  
- Posible necesidad de red mesh para cubrir áreas grandes


# Despliegue de campo SoilSense - Lista de verificación RF / Entregable 3

## Síntoma: El dispositivo no se conecta a la red

- Verificar que ambos dispositivos estén en el mismo canal:
  comando: ot dataset channel
- Revisar que la antena esté bien conectada (evitar conectores flojos)
- Confirmar que no haya obstrucciones metálicas cercanas (silos, estructuras, etc.)
- Reiniciar la red si es necesario:
  comando: ot thread start

---

## Síntoma: Pérdida intermitente de paquetes

- Verificar RSSI:
  debe ser > -70 dBm para comunicación estable
- Escanear interferencia:
  comando: ot scan energy 500
- Reducir la distancia entre nodos si hay vegetación
- Cambiar de canal si se detecta interferencia WiFi

---

## Directrices para el alcance máximo

- Línea de visión: 10 m  
- A través de vegetación ligera: 8 m  
- A través de cultivos densos: 5 m (reducir espaciamiento entre nodos)
