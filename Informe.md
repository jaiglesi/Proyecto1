# Práctica 01 · Misión presupuestos

**Sistemas Informáticos 1 · 1º DAM · [Apellido Nombre]**

Tres equipos para el Director de IT. Plataforma común: **AM4 + DDR4**, elegida por el mercado de 2026: un kit de 32 GB DDR5 cuesta unos 560-650 € en PcComponentes, frente a unos 250-280 € el de DDR4 (trampa extra detectada). El dinero que se ahorra en RAM se gasta donde cada perfil lo nota.

## 1. Resumen

| Perfil | Build en BuildCores | Total aprox. | Bolsa usada | Consumo est. | Estado |
|---|---|---|---|---|---|
| 1 · Desarrolladora Junior | [Build 1](https://es.buildcores.com/builds/eqxb1LOVm?share=true) | ~1.082 € | ~82 € | 188,8 W | Compatible |
| 2 · Técnico de Datos IA | [Build 2](https://es.buildcores.com/builds/d6trHmXHM?share=true) | ~1.568 € | ~568 € | 368,8 W | Compatible |
| 3 · Administración | [Build 3](https://es.buildcores.com/builds/2aV1Pcsol?share=true) | ~800 € | 0 € | 159,4 W | Compatible |
| **Total** | | **~3.450 €** | **~650 € de 2.000 €** | | **< 5.000 €** |

Los precios son de referencia (BuildCores; los N/A, estimados con PcComponentes). Los precios definitivos son los de la tienda enlazada en cada tabla.

## 2. Perfil 1 · Desarrolladora Junior

| Pieza | Modelo | Precio | Enlace |
|---|---|---|---|
| CPU | AMD Ryzen 7 5700G | 222,89 € | [PEGAR ENLACE] |
| Placa | Gigabyte B550M AORUS ELITE (rev. 1.3) | 116,47 € | [PEGAR ENLACE] |
| RAM | G.Skill Aegis DDR4-3200 CL16 32 GB (2×16 GB) | 270,00 € | [PEGAR ENLACE] |
| Disipador | Thermalright Peerless Assassin 120 SE White ARGB | 59,31 € | [PEGAR ENLACE] |
| SSD | Kingston NV3 1 TB, M.2 2280, PCIe 4.0 NVMe | 146,89 € | [PEGAR ENLACE] |
| Fuente | MSI MAG A650BN 650 W 80+ Bronze | 71,90 € | [PEGAR ENLACE] |
| Caja | Corsair 3000D Airflow (blanca) | ~75 € | [PEGAR ENLACE] |
| Monitor | AOC 24B2XH, 23,8" IPS 1080p 75 Hz | ~100 € | [PEGAR ENLACE] |
| Teclado + ratón | Logitech MK120 (combo con cable) | ~20 € | [PEGAR ENLACE] |

**Justificación técnica**
- **CPU:** 8 núcleos/16 hilos Zen 3, todos de rendimiento (sin E-cores). Con 3 VMs de 2 vCPU quedan 10 hilos para el anfitrión y el compilador. Trae gráficos Vega integrados, así que no hace falta comprar una GPU solo para dar vídeo.
- **RAM:** 32 GB en dual channel. 3 VMs × 4 GB = 12 GB + ~8 GB de anfitrión, IDE y contenedores ≈ 20 GB, con 12 GB de margen. Con 16 GB no cabría.
- **SSD:** NVMe de 1 TB para imágenes de VM y capas de contenedores.
- **Fuente:** 188,8 W estimados sobre 650 W = 29 % de carga.
- **Bolsa:** ~82 € por el disipador de torre (carga sostenida al compilar) y el SSD de 1 TB.

**Cuellos de botella**
- El 5700G da PCIe 3.0 al SSD (~3,5 GB/s reales, frente a los hasta 6.000 MB/s del NV3). Compilar y arrancar VMs depende más de la latencia y de los accesos aleatorios que de la lectura secuencial, así que no se nota.
- 16 MB de caché L3 (el 5700X tiene 32 MB): ligeramente más lento al compilar, pero ahorra la GPU.

## 3. Perfil 2 · Técnico de Datos IA

| Pieza | Modelo | Precio | Enlace |
|---|---|---|---|
| CPU | AMD Ryzen 7 5700X | 198,90 € | [PEGAR ENLACE] |
| Placa | MSI PRO B550M-VDH WIFI | ~120 € | [PEGAR ENLACE] |
| GPU | ASUS Dual GeForce RTX 5060 Ti 16 GB GDDR7 | ~490 € | [PEGAR ENLACE] |
| RAM | Corsair Vengeance LPX DDR4-3200 CL16 32 GB (2×16 GB) | ~280 € | [PEGAR ENLACE] |
| Disipador | ARCTIC Freezer 36 (159 mm) | 29,90 € | [PEGAR ENLACE] |
| SSD | Lexar NQ790 1 TB, PCIe 4.0 NVMe | ~150 € | [PEGAR ENLACE] |
| Fuente | Corsair CX650 650 W 80+ Bronze | ~65 € | [PEGAR ENLACE] |
| Caja | NZXT H5 Flow RGB (2024) | 75,95 € | [PEGAR ENLACE] |
| Monitor | Philips 271E1S, 27" IPS 1080p 75 Hz | ~130 € | [PEGAR ENLACE] |
| Teclado + ratón | Logitech MK270 (combo inalámbrico) | ~28 € | [PEGAR ENLACE] |

**Justificación técnica**
- **GPU:** un modelo de ~14B parámetros cuantizado a 4 bits ocupa ~9 GB y cabe entero en los 16 GB de VRAM. Se descarta la versión de 8 GB.
- **Ancho de banda (trampa 3):** cada token obliga a leer el modelo entero, así que el techo teórico de tokens/s es ancho de banda ÷ tamaño del modelo:

| Dónde vive el modelo | Ancho de banda | Techo teórico (~9 GB) |
|---|---|---|
| DDR4-3200 dual channel | 51,2 GB/s | ~5,7 tok/s |
| DDR5-6000 dual channel | ~96 GB/s | ~10,7 tok/s |
| VRAM RTX 5060 Ti | 448 GB/s | ~50 tok/s |

- **Por qué no DDR5:** costaría unos 300 € más y no cambia el resultado, porque el modelo vive en la VRAM.
- **Consumo y ruido:** la GPU es de unos 180 W. El sistema estimado es 368,8 W sobre 650 W (57 % de carga, zona de ventilador bajo). Disipador de torre de 159 mm y caja con frontal perforado para mover aire a pocas revoluciones.
- **RAM 32 GB:** datasets y preprocesado en TensorFlow/PyTorch fuera de la VRAM.
- **Bolsa (~568 €):** GPU, SSD y refrigeración. No se sube a una GPU mayor porque duplicaría consumo y ruido, la restricción crítica del perfil.

**Cuellos de botella**
- La GPU usa PCIe x8: en la B550 (PCIe 4.0) son ~16 GB/s. Solo afecta al tiempo de carga del modelo (~9 GB), no a la inferencia.
- Límite real: 16 GB de VRAM. Modelos mayores o entrenamientos grandes harían spill a RAM (51,2 GB/s) y caería la velocidad.

## 4. Perfil 3 · Administración

| Pieza | Modelo | Precio | Enlace |
|---|---|---|---|
| CPU | AMD Ryzen 5 5600G (con disipador) | ~173 € | [PEGAR ENLACE] |
| Placa | ASUS PRIME A520M-K | 53,89 € | [PEGAR ENLACE] |
| RAM | Kingston FURY Beast DDR4-3200 CL16 16 GB (2×8 GB) | ~130 € | [PEGAR ENLACE] |
| SSD | WD Blue SN5100 1 TB, M.2 2280, PCIe 4.0 NVMe | 179,89 € | [PEGAR ENLACE] |
| Fuente | Cooler Master MWE Gold 550 V2 (80+ Gold) | ~60 € | [PEGAR ENLACE] |
| Caja | Cooler Master MasterBox Q300L (micro-ATX) | 70,39 € | [PEGAR ENLACE] |
| Monitor | LG 24MP400-B, 24" IPS 1080p 75 Hz | ~100 € | [PEGAR ENLACE] |
| Teclado + ratón | HP 230 (combo inalámbrico) | ~25 € | [PEGAR ENLACE] |
| Auriculares | Logitech H111 (con micrófono) | 7,70 € | [PEGAR ENLACE] |

**Justificación técnica**
- **CPU:** 6 núcleos/12 hilos de sobra para Office 365, ERP/BI y videollamadas. Gráficos Vega 7 integrados: sin GPU dedicada ni disipador aparte.
- **RAM:** 16 GB en **dual channel**. Estimación de uso: Windows 11 ~4 GB, 40 pestañas de navegador ~4-6 GB, Office + Teams ~2-3 GB, ERP/BI ~2 GB = 12-15 GB; con 8 GB habría paginación a disco. Además, los gráficos integrados usan la RAM como memoria de vídeo: en canal simple tendrían 25,6 GB/s, en dual 51,2 GB/s.
- **SSD de 1 TB:** el requisito del perfil es almacenamiento. Cubre Windows y Office, cachés de Outlook/Teams, OneDrive local y datos del ERP/BI con margen.
- **Fuente:** 159,4 W estimados sobre 550 W = 29 % de carga; la certificación Gold da eficiencia en uso de oficina.
- **«Ni un euro de más»:** ~800 € de 1.000 €. No usa nada de la bolsa. Sin GPU, sin disipador extra, sin ventiladores adicionales.

**Cuellos de botella**
- La A520 y el 5600G dan PCIe 3.0 al SSD: el SN5100 (PCIe 4.0) rinde ~3,5 GB/s. Para ofimática no se nota. Un SSD PCIe 3.0 de 1 TB no era más barato en BuildCores (Silicon Power Gen3 1 TB: 182,26 € frente a 179,89 €).

## 5. Trampas del mercado detectadas

1. **El falso rápido (SSD):** los tres SSD son NVMe PCIe 4.0 de verdad (Kingston NV3, Lexar NQ790, WD SN5100), no M.2 con bus SATA. Detectado además que el **Kingston NV3 1 TB** existe en formato **2230** a 243,90 € y en **2280** a 146,89 €: mismo nombre, otro formato y casi 100 € de diferencia.
2. **La ilusión óptica (CPU):** los tres procesadores son Zen 3 con núcleos de rendimiento (8, 8 y 6). Se evita el reclamo de «muchos núcleos», como un Core i7-14700 de 20 núcleos con 12 E-cores.
3. **El falso ahorro energético (NPU):** ninguno de los tres equipos necesita NPU; Ollama y llama.cpp usan CPU/GPU y manda el ancho de banda (tabla del perfil 2).
4. **Extra · crisis de RAM 2026:** DDR5 32 GB ~560-650 € frente a DDR4 ~250-280 €.

## 6. Reparto de la bolsa y conclusión

De los 2.000 € de bolsa se usan ~650 €: ~568 € en el perfil 2 (GPU, SSD y refrigeración, donde el perfil lo nota), ~82 € en el perfil 1 (disipador y SSD de 1 TB) y 0 € en el perfil 3, por su restricción de coste. Total ~3.450 € de los 5.000 € máximos: no se supera el límite en ningún perfil.

*Fuentes: BuildCores (compatibilidad y consumo), fichas técnicas de los fabricantes y las tiendas enlazadas en cada tabla.*