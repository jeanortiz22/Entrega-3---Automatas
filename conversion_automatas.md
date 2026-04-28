# Conversión entre Modelos Formales  
## ER → AFD y AFD → Gramática  

---

## Integrantes

- Jean Paul Ortiz  
- Iván Daniel Naranjo  
- María Andrea Avendaño  
- Alexandra Puerta  

---

##  Introducción

En este documento se presentan dos procesos fundamentales dentro de la teoría de autómatas:

- Conversión de **Expresión Regular (ER) a Autómata Finito Determinista (AFD)**  
- Conversión de **Autómata Finito Determinista (AFD) a Gramática Regular**

Estos procesos permiten demostrar la equivalencia entre diferentes representaciones de los lenguajes regulares.

---

#  Parte 1: Conversión de ER → AFD

##  Expresión Regular

(a|b)*abb


---

##  Paso 1: ER → AFND (Método de Thompson)

```mermaid
graph LR
    q0((q0)):::inicio -->|ε| q1
    q1 -->|ε| q2
    q1 -->|ε| q4
    q2 -->|a| q3
    q4 -->|b| q5
    q3 -->|ε| q6
    q5 -->|ε| q6
    q6 -->|ε| q1
    q6 -->|ε| q7
    q7 -->|ε| q8
    q8 -->|a| q9
    q9 -->|ε| q10
    q10 -->|b| q11
    q11 -->|ε| q12
    q12 -->|b| q13
    q13((qf)):::final

    classDef inicio fill:#87CEFA,stroke:#000,stroke-width:2px;
    classDef final fill:#90EE90,stroke:#000,stroke-width:2px;

```
---

## Paso 2: AFND → AFD (Construcción de subconjuntos)

### Tabla de transición

| Estado | a | b |
|--------|---|---|
| A      | B | A |
| B      | B | C |
| C      | B | D |
| D      | B | A |

## AFD Final

graph LR
    A((A)):::inicio -->|a| B
    A -->|b| A

    B((B)) -->|a| B
    B -->|b| C

    C((C)) -->|a| B
    C -->|b| D

    D((D)):::final -->|a| B
    D -->|b| A

    classDef inicio fill:#87CEFA,stroke:#000,stroke-width:2px;
    classDef final fill:#90EE90,stroke:#000,stroke-width:2px;

## Conclusiones
- Se requiere un paso intermedio (AFND)
- Se eliminan transiciones ε
- Se obtiene un autómata determinista equivalente