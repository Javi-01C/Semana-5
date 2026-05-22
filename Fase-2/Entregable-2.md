
**¿Por qué es esencial usar una Pila y no simples contadores en el verificador?**

  Porque en los paréntesis anidados no solo nos importa la cantidad de símbolos, sino el orden exacto en que se abren y se cierran (lo último en abrirse debe ser lo primero en cerrarse).
  Si usáramos contadores, un caso cruzado como {[}] nos daría como resultado "válido" (ya que hay un cierre por cada apertura), 
  ignorando por completo que la estructura interna está mal construida. La pila, al usar LIFO, nos obliga a validar y cerrar la pareja correcta en el orden adecuado.




---


#include <iostream>
#include <string>
#include <utility>

using namespace std;

class Nodo {
public:
    string dato;
    Nodo* siguiente;

    Nodo(string d) {
        dato = d;
        siguiente = nullptr;
    }
};

class Pila {
private:
    Nodo* tope;
    int contador;

public:
    Pila() {
        tope = nullptr;
        contador = 0;
    }

    void push(string valor) {
        Nodo* nuevoNodo = new Nodo(valor);
        nuevoNodo->siguiente = tope;
        tope = nuevoNodo;
        contador++;
    }

    string pop() {
        if (estaVacia()) return "";
        Nodo* nodoAborrar = tope;
        string valor = tope->dato;
        tope = tope->siguiente;
        delete nodoAborrar;
        contador--;
        return valor;
    }

    bool estaVacia() {
        return tope == nullptr;
    }

    void vaciar() {
        while (!estaVacia()) pop();
    }

    void mostrar() {
        Nodo* actual = tope;
        if (actual == nullptr) {
            cout << "  [Vacia]" << endl;
            return;
        }
        while (actual != nullptr) {
            cout << "  -> " << actual->dato << endl;
            actual = actual->siguiente;
        }
    }
};

class EditorCodeLeap {
private:
    Pila pilaDeshacer;
    Pila pilaRehacer;

public:
    // Métodos del editor (Reto 1)
    void escribir(string cambio) {
        pilaDeshacer.push(cambio);
        pilaRehacer.vaciar();
    }

    void deshacer() {
        if (!pilaDeshacer.estaVacia()) {
            pilaRehacer.push(pilaDeshacer.pop());
        }
    }

    void rehacer() {
        if (!pilaRehacer.estaVacia()) {
            pilaDeshacer.push(pilaRehacer.pop());
        }
    }

    // Linter: Verificador de balanceo de brackets (Reto 2)
    pair<bool, string> verificarBalanceo(string codigoFuente) {
        Pila pilaBrackets;

        for (char c : codigoFuente) {
            // Si es apertura, entra a la pila
            if (c == '(' || c == '[' || c == '{') {
                pilaBrackets.push(string(1, c));
            } 
            // Si es cierre, se compara con el tope de la pila
            else if (c == ')' || c == ']' || c == '}') {
                if (pilaBrackets.estaVacia()) {
                    return {false, "INVÁLIDO (cierre sin apertura)"};
                }

                string apertura = pilaBrackets.pop();

                // Validamos que sea la pareja correcta
                if ((c == ')' && apertura != "(") ||
                    (c == ']' && apertura != "[") ||
                    (c == '}' && apertura != "{")) {
                    return {false, "INVÁLIDO (bracket incorrecto al cerrar)"};
                }
            }
        }

        // Si al final quedaron aperturas sin cerrar
        if (!pilaBrackets.estaVacia()) {
            return {false, "INVÁLIDO (faltan cierres)"};
        }

        return {true, "VÁLIDO"};
    }
};

int main() {
    EditorCodeLeap editor;

    string pruebas[] = {
        "({}[])",
        "({[]})",
        "({[}])",
        "(((",
        ")))"
    };

    cout << "=== RESULTADOS DEL LINTER ===" << endl;
    for (int i = 0; i < 5; i++) {
        string codigo = pruebas[i];
        pair<bool, string> resultado = editor.verificarBalanceo(codigo);
        
        cout << "Prueba " << i + 1 << " -> Texto: " << codigo << endl;
        cout << "Resultado: " << resultado.second << "\n" << endl;
    }

    return 0;
}
