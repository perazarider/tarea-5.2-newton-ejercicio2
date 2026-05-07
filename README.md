# tarea-5.2-newton-ejercicio2
#   Codigo que implementa la interpolacion de Newton 
#   para estimar la eficiencia de un motor térmico
#
#           Autor:
#   Rider Alberto Peraza Ortega
#   Version 1.0 : 06/05/2026
#

import numpy as np
import matplotlib.pyplot as plt

# Datos experimentales (Temperatura T vs Eficiencia %)
x_points = np.array([200, 250, 300, 350, 400])   # Temperatura (°C)
y_points = np.array([30, 35, 40, 46, 53])        # Eficiencia (%)

# Función para calcular las diferencias divididas de Newton
def diferencias_divididas(x_points, y_points):
    n = len(x_points)
    coef = np.copy(y_points).astype(float)
    for j in range(1, n):
        coef[j:n] = (coef[j:n] - coef[j-1:n-1]) / (x_points[j:n] - x_points[0:n-j])
    return coef

# Evaluación del polinomio de Newton
def newton_interpolation(x_eval, x_points, coef):
    n = len(coef)
    resultado = coef[0]
    producto = 1.0
    for i in range(1, n):
        producto *= (x_eval - x_points[i-1])
        resultado += coef[i] * producto
    return resultado

# Calcular coeficientes
coef = diferencias_divididas(x_points, y_points)

# Punto a evaluar
x_eval = 275
y_eval = newton_interpolation(x_eval, x_points, coef)

print(f"Eficiencia estimada para T = {x_eval} °C: {y_eval:.2f} %")

# Gráfica de la interpolación
x_vals = np.linspace(min(x_points), max(x_points), 200)
y_vals = [newton_interpolation(x, x_points, coef) for x in x_vals]

plt.figure(figsize=(8,5))
plt.plot(x_vals, y_vals, label="Polinomio de Newton", color="blue")
plt.scatter(x_points, y_points, color="red", label="Datos originales")
plt.scatter(x_eval, y_eval, color="green", marker="x", s=100, label=f"Interpolación en {x_eval} °C")
plt.xlabel("Temperatura (°C)")
plt.ylabel("Eficiencia (%)")
plt.title("Interpolación de Newton - Eficiencia del motor térmico")
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.savefig("eficiencia_motor_newton.png")
plt.show()
