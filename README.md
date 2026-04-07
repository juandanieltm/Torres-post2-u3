# Laboratorio: Ensamblado y Ejecución Paso a Paso

## Descripción
En este laboratorio se utilizó el DEBUG de DOS para ensamblar programas directamente en memoria, ejecutar instrucciones paso a paso con el comando T y analizar el comportamiento de los registros y banderas del procesador.

Se desarrollaron dos programas:
- Programa de suma de tres operandos
- Programa con bucle utilizando la instrucción LOOP

---

## Parte A — Programa de Suma

### Código ensamblado

MOV AX, 000A
MOV BX, 0005
MOV CX, 0003
ADD AX, BX
ADD AX, CX
INT 20


---

## Checkpoint 1: Tabla de traza — Programa de suma

| Instrucción     | AX   | BX   | CX   | IP siguiente | ZF | CF | SF |
|----------------|------|------|------|-------------|----|----|----|
| MOV AX,000A    | 000A | 0000 | 0000 | 0103        | 0  | 0  | 0  |
| MOV BX,0005    | 000A | 0005 | 0000 | 0106        | 0  | 0  | 0  |
| MOV CX,0003    | 000A | 0005 | 0003 | 0109        | 0  | 0  | 0  |
| ADD AX,BX      | 000F | 0005 | 0003 | 010B        | 0  | 0  | 0  |
| ADD AX,CX      | 0012 | 0005 | 0003 | 010D        | 0  | 0  | 0  |

**Verificación:**  
AX = 0012 (18 decimal)

---

## Parte B — Programa con Bucle usando LOOP

### Código ensamblado

MOV AX, 0000
MOV CX, 0004
ADD AX, 0002
LOOP 0106
INT 20


---

## Checkpoint 2: Tabla de traza — Programa con LOOP

| Iteración | Instrucción     | AX después | CX después | IP siguiente | ¿LOOP salta? |
|----------|----------------|------------|------------|-------------|-------------|
| 1        | MOV AX,0000    | 0000       | 0000       | 0103        | No          |
| 2        | MOV CX,0004    | 0000       | 0004       | 0106        | No          |
| 3        | ADD AX,0002    | 0002       | 0004       | 0109        | -           |
| 4        | LOOP           | 0002       | 0003       | 0106        | Sí          |
| 5        | ADD AX,0002    | 0004       | 0003       | 0109        | -           |
| 6        | LOOP           | 0004       | 0002       | 0106        | Sí          |
| 7        | ADD AX,0002    | 0006       | 0002       | 0109        | -           |
| 8        | LOOP           | 0006       | 0001       | 0106        | Sí          |
| 9        | ADD AX,0002    | 0008       | 0001       | 0109        | -           |
| 10       | LOOP           | 0008       | 0000       | 010B        | No          |

**Verificación:**  
AX = 0008 (8 decimal)

---

## Parte C — Análisis del Código Máquina


B8 00 00 -> MOV AX,0000
B9 04 00 -> MOV CX,0004
05 02 00 -> ADD AX,0002
E2 FB -> LOOP 0106
CD 20 -> INT 20


### Análisis:
- MOV AX, imm16 → 3 bytes  
- MOV CX, imm16 → 3 bytes  
- ADD AX, imm16 → 3 bytes  
- LOOP → 2 bytes  
- INT 20 → 2 bytes  

**Tamaño total:** 13 bytes

El LOOP utiliza un desplazamiento relativo:

0106 - 010B = -5 = FB


---

## Evidencias

Las capturas se encuentran en la carpeta:


capturas/
├── CP1_traza_suma.png
├── CP2_traza_loop.png


---

## Repositorio

El repositorio incluye:
- README.md
- Carpeta capturas con evidencias

### Commits realizados:
- Agrega estructura inicial
- Completa traza suma (CP1)
- Completa traza loop (CP2)

---

## Conclusión

Este laboratorio permitió comprender:
- Cómo ensamblar instrucciones en memoria con DEBUG
- Ejecución paso a paso con el comando T
- Uso de registros AX, BX y CX
- Funcionamiento de banderas (ZF, CF, SF)
- Implementación de bucles con LOOP
- Representación de instrucciones en código máquina
