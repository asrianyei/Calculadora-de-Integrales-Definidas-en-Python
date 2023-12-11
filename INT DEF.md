import tkinter as tk
from tkinter import simpledialog, messagebox
from sympy import integrate, symbols, sympify
import numpy as np
import matplotlib.pyplot as plt
from sympy.utilities.lambdify import lambdify

def calcular_integral():
    funcion_str = entrada_funcion.get()
    limite_inferior_str = entrada_limite_inferior.get()
    limite_superior_str = entrada_limite_superior.get()

    try:
        x = symbols('x')
        funcion = sympify(funcion_str)
        antiderivada = integrate(funcion, x)

        limite_inferior = float(limite_inferior_str)
        limite_superior = float(limite_superior_str)

        resultado = round(integrate(funcion, (x, limite_inferior, limite_superior)),2)
        resultado_formateado = "{:.3f}".format(resultado)
        etiqueta_resultado.config(text=f"Resultado: {resultado_formateado} u²")

        # Almacenar la antiderivada para la gráfica posterior
        ventana_principal.antiderivada = antiderivada

    except Exception as e:
        messagebox.showerror("Error", f"Error en el cálculo de la integral:\n{str(e)}")

def graficar_antiderivada():
    try:
        antiderivada = ventana_principal.antiderivada

        # Obtener límites de la gráfica
        limite_inferior = float(entrada_limite_inferior.get())
        limite_superior = float(entrada_limite_superior.get())

        # Convertir la antiderivada a una función numérica
        antiderivada_numerica = lambdify('x', antiderivada, 'numpy')

        # Graficar la antiderivada
        x_vals = np.linspace(limite_inferior, limite_superior, 1000)
        y_vals = antiderivada_numerica(x_vals)

        plt.figure(figsize=(6, 4))
        plt.plot(x_vals, y_vals, label='Antiderivada', color='blue')  # Color de la línea de la antiderivada
        plt.axhline(0, color='red')  # Línea horizontal del eje x en rojo
        plt.axvline(0, color='red')  # Línea vertical del eje y en rojo

        plt.title('Gráfica de la Antiderivada')
        plt.xlabel('x')
        plt.ylabel('y')
        plt.grid(True)
        plt.legend()

        # Mostrar la gráfica en una ventana emergente
        plt.show()

    except AttributeError:
        messagebox.showinfo("Información", "Primero calcula la integral para obtener la antiderivada.")


# Crear la aplicación
ventana_principal = tk.Tk()
ventana_principal.title("Calculadora de Integrales Definidas")

# Tamaño personalizado de la ventana
window_width = 600
window_height = 500
ventana_principal.geometry(f"{window_width}x{window_height}")

# Configuración del tema oscuro
color_fondo_oscuro = "#1E1E1E"  # Negro
color_texto = "#FFFFFF"  # Blanco

ventana_principal.configure(bg=color_fondo_oscuro)

# Crear una fuente con un tamaño más grande y color blanco
tamano_fuente = 14
fuente_personalizada = ("Archivo", tamano_fuente, "bold")

# Añadir espaciado vertical
valor_espaciado_vertical = 5

# Configuración de esquinas redondeadas
ancho_borde_redondeado = 3
relieve_redondeado = tk.GROOVE

# Título
titulo = tk.Label(ventana_principal, text="Calculadora de Integrales Definidas", font=("Archivo", 24, "bold", "underline"), bg=color_fondo_oscuro, fg=color_texto)
titulo.pack(pady=valor_espaciado_vertical)

# Crear widgets
etiqueta_funcion = tk.Label(ventana_principal, text="Función (en términos de \"x\"):", font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto)
etiqueta_funcion.pack(pady=valor_espaciado_vertical)

entrada_funcion = tk.Entry(ventana_principal, width=30, font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto,
                            borderwidth=ancho_borde_redondeado, relief=relieve_redondeado)
entrada_funcion.pack(pady=valor_espaciado_vertical)

etiqueta_limite_inferior = tk.Label(ventana_principal, text="Límite Inferior:", font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto)
etiqueta_limite_inferior.pack(pady=valor_espaciado_vertical)

entrada_limite_inferior = tk.Entry(ventana_principal, width=30, font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto,
                                     borderwidth=ancho_borde_redondeado, relief=relieve_redondeado)
entrada_limite_inferior.pack(pady=valor_espaciado_vertical)

etiqueta_limite_superior = tk.Label(ventana_principal, text="Límite Superior:", font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto)
etiqueta_limite_superior.pack(pady=valor_espaciado_vertical)

entrada_limite_superior = tk.Entry(ventana_principal, width=30, font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto,
                                     borderwidth=ancho_borde_redondeado, relief=relieve_redondeado)
entrada_limite_superior.pack(pady=valor_espaciado_vertical)

etiqueta_resultado = tk.Label(ventana_principal, text="Resultado:", font=fuente_personalizada, bg=color_fondo_oscuro, fg=color_texto)
etiqueta_resultado.pack(pady=valor_espaciado_vertical)

# Cambios en el botón Calcular
color_boton_calcular = "#4CAF50"  # Verde
tamano_fuente_boton_calcular = 16

boton_calcular = tk.Button(ventana_principal, text="Calcular", command=calcular_integral, font=("Archivo", tamano_fuente_boton_calcular),
                            bg=color_boton_calcular)
boton_calcular.pack(pady=valor_espaciado_vertical)

# Botón para graficar
color_boton_graficar = "#007BFF"  # Azul
tamano_fuente_boton_graficar = 16

boton_graficar = tk.Button(ventana_principal, text="Graficar [para funciones simples]", command=graficar_antiderivada, font=("Archivo", tamano_fuente_boton_graficar),
                            bg=color_boton_graficar)
boton_graficar.pack(pady=valor_espaciado_vertical)

# Variable para almacenar la antiderivada
ventana_principal.antiderivada = None

# Iniciar la aplicación
ventana_principal.mainloop()
