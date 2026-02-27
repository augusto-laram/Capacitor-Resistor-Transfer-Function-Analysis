# Análisis de un Circuito RC: Función de Transferencia y Respuesta Temporal

Este repositorio contiene el desarrollo técnico de la actividad de Práctica de Control sobre sistemas de primer orden. Se analiza el comportamiento dinámico de un circuito **Resistencia-Capacitor (RC)** para validar la relación entre sus parámetros físicos, la ubicación de sus polos y su estabilidad.

---

## 1. Fundamentación Teórica

Un circuito RC es un sistema de primer orden cuya dinámica se describe en el dominio de la frecuencia mediante la función de transferencia:

$$H(s) = \frac{1}{RCs + 1}$$

### Parámetros de Diseño:
* **Constante de Tiempo ($\tau$):** Se define como $\tau = R \times C$. Es el tiempo característico que tarda el capacitor en cargar al 63.2% del voltaje de entrada.
* **Ubicación del Polo:** El sistema posee un polo en $s = -1/\tau$. Al situarse en el semiplano izquierdo del plano complejo ($s < 0$), se garantiza que el sistema es **estable**.
* **Tiempo de Establecimiento ($T_s$):** Calculado como $5\tau$, representa el tiempo en el que el sistema alcanza el estado estacionario (99.3% del valor final).

---

## 2. Casos de Estudio

Se comparan dos configuraciones distintas para observar cómo la variación de la resistencia afecta la velocidad del sistema, manteniendo una capacitancia constante de $470 \, \mu F$.

| Parámetro | Caso A (Lento) | Caso B (Rápido) |
| :--- | :--- | :--- |
| **Resistencia ($R$)** | $10,000 \, \Omega$ (10 kΩ) | $1,000 \, \Omega$ (1 kΩ) |
| **Capacitor ($C$)** | $470 \times 10^{-6} \, F$ | $470 \times 10^{-6} \, F$ |
| **Polo Teórico ($s$)** | $-0.212$ | $-2.127$ |
| **Estabilidad** | Estable | Estable |

---

## 3. Implementación en MATLAB

Se utilizó el siguiente script para modelar ambos sistemas y comparar su respuesta ante una entrada escalón de **5V DC**.

```matlab
% Parámetros del sistema
C = 470e-6; 

% --- Caso A ---
Ra = 10000;
tau_a = Ra * C;
Ha = tf([1], [tau_a 1]);

% --- Caso B ---
Rb = 1000;
tau_b = Rb * C;
Hb = tf([1], [tau_b 1]);

% Simulación de Respuesta al Escalón
t = 0:0.01:25;
[y_a, t_a] = step(5*Ha, t);
[y_b, t_b] = step(5*Hb, t);

% Gráfica Comparativa
plot(t_a, y_a, 'b', t_b, y_b, 'r', 'LineWidth', 2);
grid on;
title('Respuesta Temporal del Circuito RC (Entrada 5V)');
xlabel('Tiempo (s)'); ylabel('Voltaje en el Capacitor (V)');
legend('Caso A: R=10k\Omega', 'Caso B: R=1k\Omega');


<div align="center">
  <img src="grafcomp.png" width="200">
  <br>
  <p><i>Circuito RC.</i></p>
</div>
