
**1. Posición final: Lista Enlazada vs. Arreglo Dinámico para Pilas**

Aunque implementar la pila con una lista enlazada fue excelente ya que garantiza inserciones en tiempo O(1) constante y no tiene un límite de tamaño predefinido, mi posición final para un entorno de producción general es a favor del arreglo dinámico (como un vector en C++). La razón principal es la "localidad de caché": el procesador es mucho más rápido leyendo bloques de memoria contiguos que saltando por direcciones aleatorias en la RAM como ocurre con los nodos de la list. Además, la lista enlazada desperdicia memoria al tener que guardar un puntero extra por cada dato. Reservaría las listas enlazadas únicamente para sistemas de tiempo real estricto donde no nos podemos permitir el tiempo extra que toma un arreglo dinámico al redimensionarse

---

**2. Estrategia para el Historial del Editor: ¿Texto completo o Deltas?**

Para un editor de código profesional como CodeLeap, la decisión correcta es guardar solo los deltas (los cambios específicos) en las pilas de Deshacer/Rehacer, y no el texto completo. Si guardáramos el documento entero tras cada pulsación de tecla, el consumo de memoria crecería de forma insostenible en archivos de miles de líneas. Guardar únicamente el cambio, por ejeplo "se insertó la letra 'a' en la línea 10". Es muchísimo más eficiente en memoria y es el estándar en la industria, aunque requiera programar la lógica inversa al momento de presionar Ctrl + Z

---

**3. Criterio personal: ¿Cuándo usar una Pila vs. otra estructura?**

Mi criterio fundamental para decidir si una pila es la herramienta adecuada se basa en la regla de "acceso restringido y retroceso lógico". Solo usaré una pila si el problema cumple estrictamente con el principio LIFO "Last In, First Out", lo que significa que la lógica del negocio me obliga a resolver o evaluar siempre el evento más reciente antes de poder mirar los anteriores. Ejemplos claros son la navegación web, la evaluación de expresiones matemáticas o los laberintos. Si descubro que en algún momento necesito buscar un elemento en el medio de la colección, o si el orden de atención debe ser por llegada como una fila de banco o cola de impresió, descartaré la pila inmediatamente a favor de un Arreglo
