# parcial-programacion

package biblioteca;



import java.util.*;



// Clase base: Material bibliográfico

class Material {

    protected String titulo;

    protected String autor;

    protected boolean disponible;



    public Material(String titulo, String autor) {

        this.titulo = titulo;

        this.autor = autor;

        this.disponible = true;

    }



    public void mostrarInfo() {

        System.out.println("Título: " + titulo + " | Autor: " + autor + 

                           " | Disponible: " + (disponible ? "Sí" : "No"));

    }

}



// Clase hija: Libro (hereda de Material)

class Libro extends Material {

    private String codigo;



    public Libro(String titulo, String autor, String codigo) {

        super(titulo, autor);

        this.codigo = codigo;

    }



    @Override

    public void mostrarInfo() {

        System.out.println("Código: " + codigo + " | Título: " + titulo + 

                           " | Autor: " + autor + 

                           " | Disponible: " + (disponible ? "Sí" : "No"));

    }



    public String getCodigo() {

        return codigo;

    }

}



// Clase principal de la Biblioteca

class Biblioteca {

    private List<Libro> inventario = new ArrayList<>();

    private Map<String, String> prestamos = new HashMap<>();

    private Scanner sc = new Scanner(System.in);



    public void menu() {

        int opcion;

        do {

            System.out.println("\n==== MENÚ BIBLIOTECA ====");

            System.out.println("1. Inventario");

            System.out.println("2. Préstamo");

            System.out.println("3. Devolución");

            System.out.println("4. Multas");

            System.out.println("5. Salir");

            System.out.print("Seleccione una opción: ");

            opcion = sc.nextInt();

            sc.nextLine(); // limpiar buffer



            switch (opcion) {

                case 1 -> mostrarInventario();

                case 2 -> prestarLibro();

                case 3 -> devolverLibro();

                case 4 -> calcularMulta();

                case 5 -> System.out.println("Saliendo del sistema...");

                default -> System.out.println("Opción inválida. Intente nuevamente.");

            }

        } while (opcion != 5);

    }



    public void agregarLibro(Libro libro) {

        inventario.add(libro);

    }



    private void mostrarInventario() {

        System.out.println("\n--- INVENTARIO ---");

        for (Libro l : inventario) {

            l.mostrarInfo();

        }

    }



    private void prestarLibro() {

        System.out.print("Ingrese el código del libro a prestar: ");

        String codigo = sc.nextLine();

        for (Libro l : inventario) {

            if (l.getCodigo().equalsIgnoreCase(codigo)) {

                if (l.disponible) {

                    System.out.print("Ingrese el nombre del usuario: ");

                    String usuario = sc.nextLine();

                    prestamos.put(codigo, usuario);

                    l.disponible = false;

                    System.out.println("✅ Préstamo realizado con éxito a " + usuario);

                    return;

                } else {

                    System.out.println("❌ El libro no está disponible.");

                    return;

                }

            }

        }

        System.out.println("❌ No se encontró el libro con ese código.");

    }



    private void devolverLibro() {

        System.out.print("Ingrese el código del libro a devolver: ");

        String codigo = sc.nextLine();

        for (Libro l : inventario) {

            if (l.getCodigo().equalsIgnoreCase(codigo)) {

                if (!l.disponible) {

                    l.disponible = true;

                    prestamos.remove(codigo);

                    System.out.println("✅ Devolución registrada con éxito.");

                    return;

                } else {

                    System.out.println("⚠️ El libro ya estaba disponible.");

                    return;

                }

            }

        }

        System.out.println("❌ No se encontró el libro con ese código.");

    }



    private void calcularMulta() {

        System.out.print("Ingrese la cantidad de días de retraso: ");

        int dias = sc.nextInt();

        double multa = dias * 500; // valor fijo por día

        System.out.println("💰 La multa a pagar es: $" + multa + " COP");

    }

}



// Clase principal (Main)

public class MainBiblioteca {

    public static void main(String[] args) {

        Biblioteca biblioteca = new Biblioteca();



        // Se agregan algunos libros al inventario inicial

        biblioteca.agregarLibro(new Libro("Cien años de soledad", "Gabriel García Márquez", "L001"));

        biblioteca.agregarLibro(new Libro("El Quijote", "Miguel de Cervantes", "L002"));

        biblioteca.agregarLibro(new Libro("La Odisea", "Homero", "L003"));



        // Iniciar menú

        biblioteca.menu();

    }

}

