# Design & Decision Record (DDR)
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
