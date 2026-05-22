

    **¿Por qué necesitamos DOS pilas para el sistema Deshacer / Rehacer?**
    
  Para construir este sistema, imagina que escribir código es como viajar en el tiempo:
  Pila 1: Historial (Deshacer): Guarda todos los pasos que vas dando. Cuando presionas "Deshacer", sacas la última acción de esta pila (un pop) para retroceder en el tiempo.
  Pila 2: Futuro Alternativo (Rehacer): La acción que acabas de deshacer no se borra, sino que se guarda (un push) en esta segunda pila.
          Así, si te arrepientes de haber deshecho, puedes sacar la acción de aquí y regresarla al historial original.




---


  #include <iostream>
  #include <string>

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
        if (estaVacia()) {
            return "";
        }
        Nodo* nodoAborrar = tope;
        string valor = tope->dato;
        tope = tope->siguiente;
        delete nodoAborrar;
        contador--;
        return valor;
    }

    string peek() {
        if (estaVacia()) return "";
        return tope->dato;
    }

    bool estaVacia() {
        return tope == nullptr;
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

    void vaciar() {
        while (!estaVacia()) {
            pop();
        }
    }
  };

  class EditorCodeLeap {
  private:
    Pila pilaDeshacer;
    Pila pilaRehacer;

  public:
    void escribir(string cambio) {
        cout << " Escribiendo: '" << cambio << "'" << endl;
        pilaDeshacer.push(cambio);
        pilaRehacer.vaciar();
    }

    void deshacer() {
        if (pilaDeshacer.estaVacia()) {
            cout << " Nada que deshacer." << endl;
            return;
        }
        string ultimoCambio = pilaDeshacer.pop();
        pilaRehacer.push(ultimoCambio);
        cout << " Deshecho: '" << ultimoCambio << "'" << endl;
    }

    void rehacer() {
        if (pilaRehacer.estaVacia()) {
            cout << " Nada que rehacer." << endl;
            return;
        }
        string cambioRecuperado = pilaRehacer.pop();
        pilaDeshacer.push(cambioRecuperado);
        cout << " Rehecho: '" << cambioRecuperado << "'" << endl;
    }

    void mostrarHistorial() {
        cout << "ESTADO INTERNO DEL EDITOR " << endl;
        cout << "Pila Deshacer (Historial Actual):" << endl;
        pilaDeshacer.mostrar();
        cout << "Pila Rehacer (Cambios Descartados):" << endl;
        pilaRehacer.mostrar();
    }
  };

  int main() {
    EditorCodeLeap editor;

    cout << " Paso 1: Escribir codigo " << endl;
    editor.escribir("def funcion():");
    editor.escribir("    x = 5");
    editor.escribir("    return x");
    editor.mostrarHistorial();

    cout << " Paso 2: Deshacer dos veces " << endl;
    editor.deshacer();
    editor.deshacer();
    editor.mostrarHistorial();

    cout << " Paso 3: Rehacer una vez " << endl;
    editor.rehacer();
    editor.mostrarHistorial();

    return 0;
  }


