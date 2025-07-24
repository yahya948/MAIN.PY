import sympy as sp
import numpy as np
import matplotlib.pyplot as plt

x = sp.symbols('x')

# Définir ici la fonction à étudier
f = x**2
f_prime = sp.diff(f, x)

points_critique = sp.solve(f_prime, x)

domaine = "ℝ"

print(f"Étude de la fonction : f(x) = {f}")
print(f"Domaine de définition : {domaine}")
print(f"Dérivée : f'(x) = {f_prime}")
print(f"Points critiques (f'(x)=0) : {points_critique}")

f_double_prime = sp.diff(f_prime, x)

for point in points_critique:
    valeur_second_derivative = f_double_prime.subs(x, point)
    if valeur_second_derivative > 0:
        nature = "valeur minimale"
    elif valeur_second_derivative < 0:
        nature = "valeur maximale"
    else:
        nature = "point selle (inflexion)"
    print(f"x = {point} : {nature}")

f_lambdified = sp.lambdify(x, f, "numpy")

x_vals = np.linspace(-1, 1, 400)
y_vals = f_lambdified(x_vals)

plt.plot(x_vals, y_vals, label=f'f(x) = {sp.srepr(f)}')
plt.axhline(0, color='black', linewidth=0.5) 
plt.axvline(0, color='black', linewidth=0.5)  

for point in points_critique:
    y = f_lambdified(float(point))
    plt.plot(float(point), y, 'ro')
    plt.text(float(point), y, f'  x={point}', color='red')

plt.title('LA COURBE')
plt.xlabel('x')
plt.ylabel('f(x)')
plt.legend()
plt.grid(True)
plt.show()
