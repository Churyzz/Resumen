Diseño de redes de atraso por análisis en frecuencia

## 1. Diagrama de Bode

El diagrama de Bode es una representación gráfica de cómo varía la **ganancia** y la **fase** de un sistema en función de la frecuencia. Es crucial para el análisis en frecuencia de sistemas de control, ya que permite evaluar su estabilidad y el comportamiento ante diferentes señales.

- **Diagrama de ganancia**: Muestra la magnitud de la respuesta del sistema en dB frente a la frecuencia.

   $$
   \text{Ganancia (dB)} = 20 \log_{10} \left| G(j\omega) \right|
   $$

   Donde \( G(j\omega) \) es la función de transferencia del sistema.

- **Diagrama de fase**: Muestra el desfase (en grados) entre la señal de entrada y salida del sistema frente a la frecuencia.

   $$
   \text{Fase (grados)} = \arg \left( G(j\omega) \right) \times \frac{180^\circ}{\pi}|
   $$

En el diseño de redes de atraso, el diagrama de Bode permite:
- **Evaluar el margen de ganancia y margen de fase**, que están relacionados con la estabilidad del sistema.
- **Corregir el error de estado estacionario** midiendo cómo el sistema responde en frecuencias bajas.

---

## 2. Compensadores

Los compensadores son elementos de control que se añaden al sistema para mejorar su rendimiento, corregir errores y aumentar la estabilidad.

- **Compensador de adelanto de fase**:
   - **Objetivo**: Aumentar el margen de fase y mejorar la respuesta transitoria.
   - **Efecto**: Aumenta la ganancia en frecuencias medias, mejorando la velocidad de respuesta, pero también amplifica el ruido en frecuencias altas.

   $$
   C(s) = K \frac{s + z}{s + p}, \quad z > p
   $$

- **Compensador de atraso de fase**:
   - **Objetivo**: Mejorar el error en estado estacionario sin afectar significativamente la estabilidad del sistema.
   - **Efecto**: Reduce la ganancia en frecuencias altas, lo que disminuye el ruido, pero también puede reducir la velocidad de respuesta.

   $$
   C(s) = K \frac{s + p}{s + z}, \quad p > z
   $$

- **Compensador de atraso-adelanto**:
   - Es una combinación de los dos compensadores anteriores.
   - Mejora tanto la respuesta transitoria como el error en estado estacionario, mientras que se minimizan los efectos negativos en la amplificación del ruido.

---

## 3. Control PID

Un controlador PID (Proporcional, Integral y Derivativo) puede considerarse como una combinación de compensadores de adelanto y atraso.

La ecuación general del controlador PID es:

$$
C(s) = K_p + \frac{K_i}{s} + K_d s
$$

Donde:
- **Control Proporcional (P)**: Aumenta la ganancia del sistema, mejorando la respuesta general, pero si se usa en exceso, puede generar oscilaciones.
- **Control Integral (I)**: Elimina el error de estado estacionario. Se comporta como un compensador de atraso, pero puede reducir la estabilidad si no se ajusta correctamente.
- **Control Derivativo (D)**: Mejora la respuesta transitoria y reduce el sobreimpulso. Funciona como un compensador de adelanto, ya que predice el futuro comportamiento de la salida.

El controlador PID puede ajustarse para tener el **efecto de una red de atraso-adelanto**, permitiendo una mejora en la estabilidad y en el error de seguimiento.

---

## 4. Márgenes de ganancia y fase

Estos dos márgenes son fundamentales para evaluar la **estabilidad** de un sistema en lazo cerrado:

- **Margen de Ganancia (MG)**: Es el factor por el cual puede incrementarse la ganancia del sistema en lazo abierto antes de que el sistema se vuelva inestable. Se mide en dB.

   $$
   \text{MG (dB)} = 20 \log_{10} \left( \frac{1}{|G(j\omega_{\text{cruce de fase}})|} \right)
   $$

   Si el margen de ganancia es demasiado bajo (menor que 6 dB), el sistema está cerca de la inestabilidad.

- **Margen de Fase (MP)**: Indica cuántos grados puede disminuir la fase del sistema antes de que se vuelva inestable. Se relaciona directamente con el **overshoot** (el sobreimpulso) en la respuesta del sistema.

   $$
   \text{MP (grados)} = 180^\circ + \text{Fase en } \omega_{\text{cruce de ganancia}}
   $$

Un margen de fase alto (por encima de 45°) generalmente indica un sistema estable con un **bajo sobreimpulso**.

Estos márgenes se observan fácilmente en un diagrama de Bode y son objetivos clave en el diseño de compensadores.

---

## 5. Diseño de Redes de Atraso

El diseño de **redes de atraso** se utiliza para mejorar el error de estado estacionario sin sacrificar significativamente el margen de estabilidad del sistema.

El procedimiento para diseñar redes de atraso en **control discreto** incluye:

1. **Corrección del error de estado estacionario**: Primero, se verifica el error en estado estacionario del sistema original. El objetivo de una red de atraso es reducir este error a un valor aceptable.
   
2. **Medición de márgenes de estabilidad**: Se evalúan los márgenes de ganancia y fase del sistema para asegurar que, después de aplicar la red de atraso, el sistema siga siendo estable.
   
3. **Cálculo de la frecuencia de cruce de ganancia**: Este es un punto clave en el diseño de compensadores, ya que se busca una frecuencia de cruce que permita mejorar la estabilidad sin aumentar demasiado el error.

La función de transferencia de una **red de atraso** es:

$$
C(s) = \frac{1 + T_1 s}{1 + T_2 s}, \quad T_1 < T_2
$$

En **control discreto**, esto se convierte en:

$$
C(z) = \frac{1 + T_1 z^{-1}}{1 + T_2 z^{-1}}
$$

El diseño implica ajustar \( T_1 \) y \( T_2 \) para obtener los márgenes de estabilidad y el error en estado estacionario deseados.

---

## 6. Metodología de diseño de redes de atraso

La metodología incluye varios pasos para ajustar correctamente una red de atraso:

1. **Cálculo del valor \( K_p \)**:

   $$
   K_p = \lim_{s \to 0} G(s) = \frac{1}{\text{Error en estado estacionario}}
   $$

   Este valor proporcional se ajusta para garantizar que el sistema cumpla con los requisitos de error en estado estacionario.

2. **Ajuste de márgenes de estabilidad**:
   Después de ajustar \( K_p \), se evalúan los márgenes de estabilidad del sistema. Si el margen de fase o el margen de ganancia no cumplen con los requisitos, se diseñan compensadores para ajustarlos.

3. **Cálculo de \( T_1 \)**:
   \( T_1 \) es el parámetro que define el tiempo de retraso en la red de atraso. Este parámetro se ajusta para lograr el margen de fase deseado en el sistema.

4. **Verificación con diagrama de Bode**:
   Finalmente, se grafica el diagrama de Bode del sistema con la red de atraso aplicada y se verifica que los márgenes de ganancia y fase sean los adecuados.

Este proceso es iterativo, ajustando \( K_p \) y \( T_1 \) hasta cumplir con los requisitos de diseño.

---

## Ejemplos de Redes de Atraso

Aquí se presentan dos ejemplos de cómo diseñar redes de atraso para diferentes situaciones:

### **Ejemplo 1: Red de Atraso para Mejorar el Error en Estado Estacionario**

Función de transferencia original:

$$
G(s) = \frac{1}{s(s+1)}
$$

Después de agregar una red de atraso con \( T_1 = 0.1 \) y \( T_2 = 1 \):

$$
C(s) = \frac{1 + 0.1s}{1 + s}
$$

Nueva función de transferencia en lazo abierto:

$$
G'(s) = \frac{1 + 0.1s}{s^2 (1 + s)^2}
$$

### **Ejemplo 2: Red de Atraso para Aumentar el Margen de Estabilidad**

Función de transferencia original:

$$
G(s) = \frac{10}{s(s + 5)}
$$

Después de agregar una red de atraso con \( T_1 = 0.2 \) y \( T_2 = 2 \):

$$
C(s) = \frac{1 + 0.2s}{1 + 2s}
$$

Nueva función de transferencia:

$$
G'(s) = \frac{(1 + 0.2s) \cdot 10}{s(s + 5)(1 + 2s)}
$$
