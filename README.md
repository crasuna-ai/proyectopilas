// una tienda de ropa requiere un sistema que le permita lo siguiente

//menu 
//1. registrar prenda
// 2. consultar prenda
// 3. modificar prenda
// 4. vender prenda
// 5. consultar stock

// 2: cada prenda tiene los siguientes atributos 
// Marca, Referencia, Precio, Cantidad

// 3 cuando voy a ingresar una prenda nueva el sistema debe validar si existe 
// si existe le debe sumar las cantidades si no debe crear una nueva

//4: cuando  se vende una prenda se debe restar la cantidad 
// si la prenda a vender no existe debe mostrar mensaje "la  prenda no existe y permitir ingresar ya sea por marca o por referencia de lo contrario debe mostrar el stock con lacantidad de la penda reducida"

import java.util.Stack;

public class Inventario {
    private Stack<Prenda> stock;

    public Inventario() {
        stock = new Stack<>();
    }

    // Registrar una nueva prenda o actualizar cantidad si ya existe
    public void registrarPrenda(String marca, String referencia, double precio, int cantidad) {
        for (Prenda p : stock) {
            if (p.referencia.equals(referencia)) {
                // Si la prenda existe, sumamos las cantidades
                p.cantidad += cantidad;
                System.out.println("Prenda existente. Se ha sumado la cantidad.");
                return;
            }
        }
        // Si no existe, se agrega una nueva prenda
        stock.push(new Prenda(marca, referencia, precio, cantidad));
        System.out.println("Prenda registrada con éxito.");
    }

    // Consultar prenda por referencia o marca
    public void consultarPrenda(String buscar) {
        for (Prenda p : stock) {
            if (p.referencia.equals(buscar) || p.marca.equalsIgnoreCase(buscar)) {
                System.out.println("Prenda encontrada: " + p);
                return;
            }
        }
        System.out.println("La prenda no existe.");
    }

    // Modificar cantidad de una prenda
    public void modificarPrenda(String referencia, int nuevaCantidad) {
        for (Prenda p : stock) {
            if (p.referencia.equals(referencia)) {
                p.cantidad = nuevaCantidad;
                System.out.println("Prenda modificada con éxito.");
                return;
            }
        }
        System.out.println("La prenda no existe.");
    }

    // Vender prenda
    public void venderPrenda(String buscar, int unidades) {
        for (Prenda p : stock) {
            if (p.referencia.equals(buscar) || p.marca.equalsIgnoreCase(buscar)) {
                if (p.cantidad >= unidades) {
                    p.cantidad -= unidades;
                    System.out.println("Venta realizada con éxito. Cantidad restante: " + p.cantidad);
                } else {
                    System.out.println("No hay suficiente stock.");
                }
                return;
            }
        }
        System.out.println("La prenda no existe.");
    }

    // Consultar todo el stock
    public void consultarStock() {
        if (stock.isEmpty()) {
            System.out.println("El stock está vacío.");
        } else {
            System.out.println("Stock de prendas:");
            for (Prenda p : stock) {
                System.out.println(p);
            }
        }
    }
}
import java.util.Scanner;

public class TiendaRopa {
    private static Inventario inventario = new Inventario();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int opcion;

        do {
            mostrarMenu();
            opcion = scanner.nextInt();
            scanner.nextLine();  // Limpiar buffer de scanner

            switch (opcion) {
                case 1:
                    registrarPrenda();
                    break;
                case 2:
                    consultarPrenda();
                    break;
                case 3:
                    modificarPrenda();
                    break;
                case 4:
                    venderPrenda();
                    break;
                case 5:
                    consultarStock();
                    break;
                case 0:
                    System.out.println("Saliendo...");
                    break;
                default:
                    System.out.println("Opción no válida. Intente de nuevo.");
            }
        } while (opcion != 0);
    }

    // Mostrar el menú
    private static void mostrarMenu() {
        System.out.println("Menu:");
        System.out.println("1. Registrar prenda");
        System.out.println("2. Consultar prenda");
        System.out.println("3. Modificar prenda");
        System.out.println("4. Vender prenda");
        System.out.println("5. Consultar stock");
        System.out.println("0. Salir");
        System.out.print("Ingrese una opción: ");
    }

    // Registrar una prenda
    private static void registrarPrenda() {
        System.out.print("Ingrese la marca: ");
        String marca = scanner.nextLine();
        System.out.print("Ingrese la referencia: ");
        String referencia = scanner.nextLine();
        System.out.print("Ingrese el precio: ");
        double precio = scanner.nextDouble();
        System.out.print("Ingrese la cantidad: ");
        int cantidad = scanner.nextInt();
        scanner.nextLine();  // Limpiar buffer de scanner

        inventario.registrarPrenda(marca, referencia, precio, cantidad);
    }

    // Consultar una prenda
    private static void consultarPrenda() {
        System.out.print("Ingrese la referencia o marca de la prenda: ");
        String buscar = scanner.nextLine();

        inventario.consultarPrenda(buscar);
    }

    // Modificar una prenda
    private static void modificarPrenda() {
        System.out.print("Ingrese la referencia de la prenda a modificar: ");
        String referencia = scanner.nextLine();
        System.out.print("Ingrese la nueva cantidad: ");
        int nuevaCantidad = scanner.nextInt();
        scanner.nextLine();  // Limpiar buffer de scanner

        inventario.modificarPrenda(referencia, nuevaCantidad);
    }

    // Vender una prenda
    private static void venderPrenda() {
        System.out.print("Ingrese la referencia o marca de la prenda a vender: ");
        String buscar = scanner.nextLine();
        System.out.print("¿Cuántas unidades desea vender? ");
        int unidades = scanner.nextInt();
        scanner.nextLine();  // Limpiar buffer de scanner

        inventario.venderPrenda(buscar, unidades);
    }

    // Consultar todo el stock
    private static void consultarStock() {
        inventario.consultarStock();
    }
}

