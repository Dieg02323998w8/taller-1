#include <iostream>
#include <malloc.h>
#include <cstring>

using namespace std;



struct Nodo {
    char nombre[50];
    int numero;
    int vueltas;
    double tiempo_total;
    Nodo* izq;
    Nodo* der;
};

Nodo* crearNodo(char nombre[], int numero, int vueltas, double tiempo_total) {
    Nodo* nuevoNodo = (Nodo*)malloc(sizeof(Nodo));
    strcpy(nuevoNodo->nombre, nombre);
    nuevoNodo->numero = numero;
    nuevoNodo->vueltas = vueltas;
    nuevoNodo->tiempo_total = tiempo_total;
    nuevoNodo->izq = NULL;
    nuevoNodo->der = NULL;
    return nuevoNodo;
}

Nodo* insertarNodo(Nodo* raiz, char nombre[], int numero, int vueltas, double tiempo_total) {
    if (raiz == NULL) {
        raiz = crearNodo(nombre, numero, vueltas, tiempo_total);
    }
    else if (tiempo_total < raiz->tiempo_total) {
        raiz->izq = insertarNodo(raiz->izq, nombre, numero, vueltas, tiempo_total);
    }
    else {
        raiz->der = insertarNodo(raiz->der, nombre, numero, vueltas, tiempo_total);
    }
    return raiz;
}

int obtenerAltura(Nodo* nodo) {
    if (nodo == NULL) {
        return 0;
    }
    int alturaIzquierda = obtenerAltura(nodo->izq);
    int alturaDerecha = obtenerAltura(nodo->der);
    return 1 + max(alturaIzquierda, alturaDerecha);
}

int obtenerFactorEquilibrio(Nodo* nodo) {
    if (nodo == NULL) {
        return 0;
    }
    int alturaIzquierda = obtenerAltura(nodo->izq);
    int alturaDerecha = obtenerAltura(nodo->der);
    return alturaIzquierda - alturaDerecha;
}

Nodo* rotacionIzquierda(Nodo* nodo) {
    Nodo* nodoDerecha = nodo->der;
    Nodo* subArbolIzquierdo = nodoDerecha->izq;
    nodoDerecha->izq = nodo;
    nodo->der = subArbolIzquierdo;
    return nodoDerecha;
}

Nodo* rotacionDerecha(Nodo* nodo) {
    Nodo* nodoIzquierda = nodo->izq;
    Nodo* subArbolDerecho = nodoIzquierda->der;
    nodoIzquierda->der = nodo;
    nodo->izq = subArbolDerecho;
    return nodoIzquierda;
}

Nodo* balancearNodo(Nodo* nodo) {
    int factorEquilibrio = obtenerFactorEquilibrio(nodo);
    if (factorEquilibrio > 1) {
        if (obtenerFactorEquilibrio(nodo->izq) < 0) {
            nodo->izq = rotacionIzquierda(nodo->izq);
        }
        return rotacionDerecha(nodo);
    }
    if (factorEquilibrio < -1) {
        if (obtenerFactorEquilibrio(nodo->der) > 0) {
            nodo->der = rotacionDerecha(nodo->der);
        }
        return rotacionIzquierda(nodo);
    }
    return nodo;
}

Nodo* balancearArbol(Nodo* raiz) {
    if (raiz == NULL) {
        return NULL;
    }
    raiz->izq = balancearArbol(raiz->izq);
    raiz->der = balancearArbol(raiz->der);
    return balancearNodo(raiz);
}

void mostrarArbolMenoraMayor(Nodo* raiz) {
    if (raiz != NULL) {
        mostrarArbolMenoraMayor(raiz->izq);
        cout << "Nombre: " << raiz->nombre << ", Numero: " << raiz->numero << ", Vueltas: " << raiz->vueltas << ", Tiempo promedio: " << raiz->tiempo_total << endl;
        mostrarArbolMenoraMayor(raiz->der);
    }
}

void mostrarArbolMayoraMenor(Nodo* raiz) {
    if (raiz != NULL) {
        mostrarArbolMayoraMenor(raiz->der);
        cout << "Nombre: " << raiz->nombre << ", Numero: " << raiz->numero << ", Vueltas: " << raiz->vueltas << ", Tiempo promedio: " << raiz->tiempo_total << endl;
        mostrarArbolMayoraMenor(raiz->izq);
    }
}

double calcularPromedioTiempo(double tiempo_total, int vueltas) {
    return tiempo_total / vueltas;
}

Nodo* buscarNodo(Nodo* raiz, int numero) {
    if (raiz == NULL || raiz->numero == numero) {
        return raiz;
    }
    if (numero < raiz->numero) {
        return buscarNodo(raiz->izq, numero);
    }
    return buscarNodo(raiz->der, numero);
}

void agregarCorredor(Nodo*& raiz) {
    char nombre[50];
    int numero;
    int vueltas;
    double tiempo_total;
    cout << "Ingrese el nombre del corredor: ";
    cin >> nombre;
    cout << "Ingrese el numero asignado al corredor: ";
    cin >> numero;

    Nodo* nodoExistente = buscarNodo(raiz, numero);
    if (nodoExistente != NULL) {
        cout << "El número " << numero << " ya ha sido utilizado por otro corredor." << endl;
        return;
    }

    cout << "Ingrese la cantidad de vueltas del corredor: ";
    cin >> vueltas;
    if (vueltas < 2) {
        cout << "El corredor debe de dar al menos dos vueltas." << endl;
    }
    else {
        cout << "Ingrese el tiempo total del corredor: ";
        cin >> tiempo_total;
        double promedio_tiempo = calcularPromedioTiempo(tiempo_total, vueltas);
        raiz = insertarNodo(raiz, nombre, numero, vueltas, promedio_tiempo);
        raiz = balancearArbol(raiz);
    }
}

int main() {
    Nodo* raiz = NULL;
    int opcion = 0;
    while (opcion != 5) {
        cout << endl << "Menu" << endl;
        cout << "1. Agregar corredor" << endl;
        cout << "2. Mostrar arbol de menor a mayor o ascendente" << endl;
        cout << "3. Mostrar arbol de mayor a menor o descendente" << endl;
        cout << "4. Balancear arbol" << endl;
        cout << "5. Salir" << endl;
        cout << "Seleccione una opcion: ";
        cin >> opcion;
        switch (opcion) {
        case 1:
            agregarCorredor(raiz);
            break;
        case 2:
            mostrarArbolMenoraMayor(raiz);
            break;
        case 3:
            mostrarArbolMayoraMenor(raiz);
            break;
        case 4:
            raiz = balancearArbol(raiz);
            break;
        default:
            break;
        }
    }

    return 0;
}
