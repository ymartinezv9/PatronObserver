# PatronObserver

# Índice

1. [¿Qué es el patrón Observer?](#1-¿qué-es-el-patrón-observer)
2. [Ventajas del patrón Observer](#2-ventajas-del-patrón-observer)
3. [Ejemplo del Patrón Observer: Notificaciones para estudiantes de la UMG](#3-ejemplo-del-patrón-observer-notificaciones-para-estudiantes-de-la-umg)
   - [Código en Java](#código-en-java)
   - [Resultado en consola](#resultado-en-consola)
4. [Explicación](#explicación)
5. [Conclusión](#conclusión)

---

# Patrón de Diseño Observer

## 1. ¿Qué es el patrón Observer?

El patrón **Observer** es un patrón de diseño comportamental que define una relación de dependencia uno-a-muchos entre objetos, de tal forma que cuando un objeto cambia de estado, todos sus dependientes son notificados y actualizados automáticamente. Este patrón es útil cuando un cambio en un objeto requiere actualizar a otros objetos, pero sin que estos objetos estén acoplados de manera rígida.

En lugar de que los objetos dependientes verifiquen constantemente si algo ha cambiado, el patrón **Observer** les permite "escuchar" cambios, notificándoles solo cuando sea necesario. Los objetos que notifican se llaman **Sujetos (Subjects)**, y los objetos que reciben las actualizaciones son llamados **Observadores (Observers)**.

## 2. Ventajas del patrón Observer

- **Desacoplamiento**: Permite que los objetos cambien de estado sin depender directamente unos de otros.
- **Escalabilidad**: Es fácil agregar más observadores sin alterar el código del sujeto.
- **Flexibilidad**: Diferentes observadores pueden reaccionar de manera distinta ante el mismo evento de cambio.
- **Facilita la actualización automática**: Los observadores se actualizan automáticamente cuando el sujeto cambia su estado.

## 3. Ejemplo del Patrón Observer: Notificaciones para estudiantes de la UMG

En este ejemplo, implementamos un sistema de notificaciones para estudiantes de la **UMG**. El sistema tiene un sujeto que es el **Departamento de Notificaciones**, y los observadores son los estudiantes. Cada vez que el departamento publica un aviso, los estudiantes suscritos reciben las actualizaciones.

### Código en Java

```java
// Interfaz Observer
interface Observer {
    void update(String mensaje);
}

// Interfaz Subject
interface Subject {
    void suscribir(Observer observer);
    void desuscribir(Observer observer);
    void notificar(String mensaje);
}

// Sujeto concreto: Departamento de Notificaciones
class DepartamentoNotificaciones implements Subject {
    private List<Observer> observadores = new ArrayList<>();

    @Override
    public void suscribir(Observer observer) {
        observadores.add(observer);
    }

    @Override
    public void desuscribir(Observer observer) {
        observadores.remove(observer);
    }

    @Override
    public void notificar(String mensaje) {
        for (Observer observer : observadores) {
            observer.update(mensaje);
        }
    }

    // Método para crear un nuevo aviso
    public void nuevoAviso(String aviso) {
        System.out.println("Departamento de Notificaciones: " + aviso);
        notificar(aviso);
    }
}

// Observador concreto: Estudiante UMG
class EstudianteUMG implements Observer {
    private String nombre;

    public EstudianteUMG(String nombre) {
        this.nombre = nombre;
    }

    @Override
    public void update(String mensaje) {
        System.out.println(nombre + " ha recibido una notificación: " + mensaje);
    }
}

// Uso del patrón Observer
public class Main {
    public static void main(String[] args) {
        DepartamentoNotificaciones departamento = new DepartamentoNotificaciones();

        // Estudiantes que se suscriben a las notificaciones
        EstudianteUMG estudiante1 = new EstudianteUMG("Juan");
        EstudianteUMG estudiante2 = new EstudianteUMG("Ana");

        departamento.suscribir(estudiante1);
        departamento.suscribir(estudiante2);

        // Nuevo aviso del departamento
        departamento.nuevoAviso("Clases suspendidas por mal tiempo.");

        // Estudiante desuscrito
        departamento.desuscribir(estudiante1);

        // Nuevo aviso para los estudiantes restantes
        departamento.nuevoAviso("Nueva fecha para el examen final.");
    }
}
```
### Resultado en consola
```bash
Departamento de Notificaciones: Clases suspendidas por mal tiempo.
Juan ha recibido una notificación: Clases suspendidas por mal tiempo.
Ana ha recibido una notificación: Clases suspendidas por mal tiempo.
Departamento de Notificaciones: Nueva fecha para el examen final.
Ana ha recibido una notificación: Nueva fecha para el examen final.
```

### Explicación:

- **Observer**: Es la interfaz que define el método `update()` que los observadores implementarán para recibir notificaciones.
- **Subject**: Es la interfaz del sujeto que permite a los observadores suscribirse, desuscribirse y recibir notificaciones.
- **DepartamentoNotificaciones**: Es el sujeto concreto que mantiene la lista de observadores y los notifica cuando ocurre un cambio (cuando se publica un nuevo aviso).
- **EstudianteUMG**: Es el observador concreto que implementa el método `update()` y reacciona cuando recibe una notificación.

En este ejemplo, los estudiantes se suscriben a las notificaciones del departamento. Cuando se publica un nuevo aviso, los estudiantes suscritos reciben el mensaje. Si un estudiante se desuscribe, deja de recibir notificaciones.

---

### Conclusión

El patrón **Observer** permite gestionar fácilmente la dependencia entre un sujeto y varios observadores. Es ideal para implementar sistemas de notificación o eventos, donde un cambio en el sujeto debe ser comunicado a varios observadores. En este ejemplo, los estudiantes de la **UMG** reciben avisos importantes del **Departamento de Notificaciones** de manera automática, dependiendo de si están suscritos o no.
