---
sidebar_position: 1
---

# Título 1

esta es una línea de prueba y fórmula matemática:

$$
f(x) = \cfrac{x^2}{\frac{1}{\sqrt{x}} + \frac{1}{\sqrt[3]{x}}}
$$

esta es una tabla de ejemplo:

| Nombre     | Edad | Ciudad       | Profesión      |
|------------|------|--------------|----------------|
| Juan Pérez | 25   | Madrid       | Desarrollador  |
| María López| 30   | Barcelona    | Diseñadora     |
| Carlos Ruiz| 22   | Valencia     | Estudiante     |
| Ana García | 35   | Sevilla      | Ingeniera      |



## Sub título 1.1

sin negrita **con negrita**.

Or **try Docusaurus immediately** with **[docusaurus.new](https://docusaurus.new)**.

### sub sub título 1.1.1

- [Node.js](https://nodejs.org/en/download/) version 18.0 or above:
  - When installing Node.js, you are recommended to check all checkboxes related **to dependencies.**

## sub título 1.2

Ejemplo para insertar **código python**: 

```python
# Ejemplo de función en Python
def saludar(nombre):
    """Saluda al usuario."""
    return f"¡Hola, {nombre}! Bienvenido/a."

# Llamada a la función
print(saludar("Ana"))  # Output: ¡Hola, Ana! Bienvenido/a.

# Ejemplo con lista y bucle
frutas = ["manzana", "banana", "naranja"]
for fruta in frutas:
    print(f"Me gusta la {fruta}")
```

esto es **negrita**

The classic **template** will **automatically** be added to your project after you run the command:

```r
library(data.table)  # Cargar la librería data.table

# Ejemplo de análisis en R
data <- c(23, 45, 67, 89, 34, 56)  # Vector de datos

# Calcular media y desviación estándar
media <- mean(data)
desviacion <- sd(data)

# Mostrar resultados
print(paste("Media:", round(media, 2)))
print(paste("Desviación estándar:", round(desviacion, 2)))

# Gráfico básico
plot(data, type = "o", col = "blue", main = "Análisis de datos")
```

You can type this command into Command Prompt, Powershell, Terminal, or any other integrated terminal of your code editor.

The command also installs all necessary dependencies you need to run Docusaurus.

## sub título 1.3

Run the development server:

```bash
cd my-website
npm run start
```

The `cd` command changes the directory you're working with. In order to work with your newly created Docusaurus site, you'll need to navigate the terminal there.

The `npm run start` command builds your website locally and serves it through a development server, ready for you to view at http://localhost:3000/.

Open `docs/intro.md` (this page) and edit some lines: the site **reloads automatically** and displays your changes.
