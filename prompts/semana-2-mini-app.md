# Mini-aplicación de práctica: Generador de contraseñas

## Versión A

**Objetivo:** construir la mini-aplicación con un prompt de una línea.  
**Herramienta y modelo:** Copilot con el modelo Auto.  
**Prompt:**  
```
Haz una app web que genere contraseñas con FastAPI.
```
**Resultado:** El agente creó un único archivo `main.py` en la raíz de la carpeta usando el módulo `random` en vez de `secrets`, con un endpoint no estándar `/generate-password` y campos en inglés (`password`, `length`). No implementó la estructura por capas ni respetó los límites de longitud (8 a 64). Al aplicar las pruebas de aceptación de Anexo D, se obtuvieron 0 pruebas aprobadas de 7 debido a `ModuleNotFoundError: No module named 'app'`. Ruff reportó 6 advertencias de formato.

## Versión B

**Objetivo:** construir la misma aplicación con un prompt estructurado, AGENTS.md y pruebas de aceptación.  
**Herramienta y modelo:** Copilot con el modelo Auto.  
**Prompt:**  
```markdown
## Rol
Actúa como desarrollador backend con experiencia en FastAPI, Jinja2 y pruebas con pytest.

## Objetivo
Construir la mini-aplicación web Generador de contraseñas con estos requisitos:
1. GET / muestra un formulario para elegir la longitud y si se incluyen números y símbolos, un botón para generar y un botón para copiar la contraseña.
2. GET /api/contrasena recibe los parámetros longitud, numeros y simbolos, y devuelve {"contrasena": texto, "longitud": número}. Valores por defecto: longitud 12, numeros true y simbolos false.
3. La contraseña siempre incluye letras minúsculas y mayúsculas. Si numeros es true, incluye al menos un número; si es false, no incluye ninguno. Si simbolos es true, incluye al menos un símbolo de este grupo: !@#$%&*?-_ ; si es false, no incluye ninguno.
4. La longitud debe estar entre 8 y 64; fuera de ese rango responde con el código 422.
5. La contraseña se genera con el módulo secrets de Python, no con random.

## Contexto
Lee AGENTS.md. El contrato de la API está en tests/test_aceptacion.py: las rutas, los nombres de los campos y los códigos de estado deben coincidir con esas pruebas.

## Restricciones
- Usa solo las dependencias de requirements.txt.
- Define las rutas en app/main.py y la lógica en app/servicios.py, sin importar FastAPI en servicios.
- Guarda los datos en memoria, sin base de datos ni archivos.
- No modifiques tests/test_aceptacion.py.
- La página usa HTML, CSS y JavaScript sin librerías externas.

## Formato de salida
Antes de escribir código, muestra el plan con las rutas completas de los archivos que vas a crear y espera mi aprobación. Al terminar, resume en una línea qué hace cada archivo.

## Ejemplo
GET /api/contrasena?longitud=16&numeros=true&simbolos=true devuelve:
{"contrasena": "q7G!mR2-xW9a#Lp4", "longitud": 16}

## Criterio de éxito
python -m pytest -v pasa todas las pruebas y ruff check . no reporta errores. Si después de 3 intentos las pruebas no pasan, detente y explica el problema.
```
**Resultado:** El agente presentó un plan preciso indicando las rutas completas de `app/main.py`, `app/servicios.py` y `app/templates/index.html`. Implementó la generación criptosegura con `secrets`, separó correctamente la lógica de las rutas y cumplió todo el contrato de la API. Se requirió 1 solo intento. Las 7 pruebas de aceptación pasaron en verde (7 de 7) y Ruff reportó 0 errores.

## Mejora

**Objetivo:** Agregar cálculo del campo de fuerza de la contraseña y retroalimentación visual interactiva en la interfaz web.  
**Herramienta y modelo:** Copilot con el modelo Auto.  
**Prompt:**  
```markdown
## Rol
Actúa como desarrollador backend con experiencia en FastAPI y pruebas con pytest.

## Objetivo
Agregar el cálculo de fuerza de la contraseña y reflejarlo en la interfaz de la mini-aplicación:
- El endpoint GET /api/contrasena incluye en su respuesta el campo 'fuerza'.
- Su valor es 'fuerte' si la longitud es 16 o más e incluye números y símbolos.
- Su valor es 'débil' si la longitud es menor que 12 o no incluye números ni símbolos.
- Su valor es 'media' en los demás casos.
- La página web index.html muestra el nivel de fuerza con color: verde para fuerte, amarillo para media y rojo para débil.

## Contexto
Lee AGENTS.md, app/main.py y app/servicios.py. No modifiques tests/test_aceptacion.py.

## Restricciones
- Usa solo las dependencias de requirements.txt.
- Crea una prueba nueva en tests/test_mejora.py que compruebe la mejora.
- No modifiques tests/test_aceptacion.py.
- La página principal debe permitir visualizar la fuerza y usar la mejora.

## Formato de salida
Antes de escribir código, muestra el plan con las rutas completas de los archivos que vas a modificar o crear y espera mi aprobación.

## Ejemplo
GET /api/contrasena?longitud=16&numeros=true&simbolos=true devuelve:
{"contrasena": "q7G!mR2-xW9a#Lp4", "longitud": 16, "fuerza": "fuerte"}

## Criterio de éxito
python -m pytest tests/test_mejora.py -v pasa todas las pruebas y ruff check . no reporta errores. Si después de 3 intentos no pasa, detente y explica el problema.
```
**Resultado:** El agente modificó `app/main.py`, `app/servicios.py` y `app/templates/index.html`, agregando la prueba en `tests/test_mejora.py`. Se verificó que pasaran las 7 pruebas de aceptación originales más las 3 nuevas pruebas de la mejora (10 de 10 aprobadas en verde). Ruff finalizó con 0 errores.

## Comparación

| Criterio | Versión A | Versión B |
| :--- | :---: | :---: |
| **Modelo usado** | Copilot (Auto) | Copilot (Auto) |
| **Mensajes enviados al agente** | 1 | 2 |
| **La aplicación arranca** | Sí (con comando manual uvicorn main:app) | Sí (uvicorn app.main:app) |
| **Requisitos del anexo que cumple** | 2 de 5 | 5 de 5 |
| **Pruebas de aceptación aprobadas** | 0 de 7 | 7 de 7 |
| **Errores de Ruff** | 6 | 0 |
| **Separa rutas y lógica** | No (monolítico en raíz) | Sí (app/main.py y app/servicios.py) |
| **Dependencias no indicadas** | Intentó sugerir passlib | Ninguna |
| **Decisiones del agente por su cuenta** | Usó random en vez de secrets, nombres en inglés, sin validación 422 | Respetó estrictamente el contrato y AGENTS.md |

**Conclusión:**
La mayor diferencia fue producida por el contexto y el contrato de pruebas en el Prompt B, ya que delimitó las rutas, los nombres exactos de los campos y las restricciones de seguridad. En la Versión A, el agente decidió por su cuenta usar el módulo inseguro `random`, nombres en inglés y colocar el código en un único archivo en la raíz, violando la arquitectura esperada. Para futuras tareas, siempre se debe entregar el contrato de la API y las pruebas de aceptación antes de pedir código, garantizando un criterio de éxito determinista y verificable.
