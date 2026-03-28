# Reto2-Registro-Donaciones

## Proyecto de registro de donaciones de computadores en Java. Este proyecto se divide en 5 clases las cuales son:

---

## -Computadores.

En esta clase encontraremos los atributos base de cualquier computador como su precio base, peso y consumo energético. También contiene las constantes con los valores por defecto y el método para calcular el precio según el consumo y el peso del equipo.

## Codigo.

```java
public class Computadores {

    private final static char    CONSUMO_W_BASE = 'F';
    private final static Double  PRECIO_BASE    = 100.0;
    private final static Integer PESO_BASE      = 5;

    protected Double  precioBase;
    protected Integer peso;
    protected char    consumoW;

    public Computadores() {
        this.precioBase = PRECIO_BASE;
        this.peso       = PESO_BASE;
        this.consumoW   = CONSUMO_W_BASE;
    }

    public Computadores(Double precioBase, Integer peso) {
        this.precioBase = precioBase;
        this.peso       = peso;
        this.consumoW   = CONSUMO_W_BASE;
    }

    public Computadores(Double precioBase, Integer peso, char consumoW) {
        this.precioBase = precioBase;
        this.peso       = peso;
        this.consumoW   = consumoW;
    }

    public Double calcularPrecio() {
        Double adicion = 0.0;

        switch (this.consumoW) {
            case 'A': adicion += 100; break;
            case 'B': adicion += 80;  break;
            case 'C': adicion += 60;  break;
            case 'D': adicion += 50;  break;
            case 'E': adicion += 30;  break;
            case 'F': adicion += 10;  break;
            default:  adicion += 0;   break;
        }

        if      (this.peso >= 0  && this.peso < 19)  adicion += 10;
        else if (this.peso >= 20 && this.peso < 49)  adicion += 50;
        else if (this.peso >= 50 && this.peso <= 79) adicion += 80;
        else if (this.peso >= 80)                    adicion += 100;

        return this.precioBase + adicion;
    }

    public Double  getPrecioBase() { return precioBase; }
    public Integer getPeso()       { return peso; }
    public char    getConsumoW()   { return consumoW; }
}
```

---

## -ComputadoresMesa.

En esta clase tenemos la herencia de la clase Computadores usando extends. Agrega el atributo almacenamiento que suma un valor adicional al precio si supera los 100 GB. Sobreescribe el método calcularPrecio() usando @Override para incluir esta lógica extra.

## Codigo.

```java
public class ComputadoresMesa extends Computadores {

    private final static Integer ALMACENAMIENTO_BASE = 50;
    private Integer almacenamiento;

    public ComputadoresMesa() {
        super();
        this.almacenamiento = ALMACENAMIENTO_BASE;
    }

    public ComputadoresMesa(Double precioBase, Integer peso) {
        super(precioBase, peso);
        this.almacenamiento = ALMACENAMIENTO_BASE;
    }

    public ComputadoresMesa(Double precioBase, Integer peso, char consumoW, Integer almacenamiento) {
        super(precioBase, peso, consumoW);
        this.almacenamiento = almacenamiento;
    }

    @Override
    public Double calcularPrecio() {
        Double precio = super.calcularPrecio();
        if (this.almacenamiento > 100) precio += 50;
        return precio;
    }

    public Integer getCarga() { return almacenamiento; }
}
```

---

## -ComputadoresPortatiles.

En esta clase también heredamos de Computadores. Agrega los atributos pulgadas y camaraITG. Si las pulgadas superan 40 se agrega el 30% del precio base, y si tiene cámara integrada se suman $50 adicionales.

## Codigo.

```java
public class ComputadoresPortatiles extends Computadores {

    private final static Integer PULGADAS_BASE = 20;
    private Integer pulgadas;
    private boolean camaraITG;

    public ComputadoresPortatiles() {
        super();
        this.pulgadas  = PULGADAS_BASE;
        this.camaraITG = false;
    }

    public ComputadoresPortatiles(Double precioBase, Integer peso) {
        super(precioBase, peso);
        this.pulgadas  = PULGADAS_BASE;
        this.camaraITG = false;
    }

    public ComputadoresPortatiles(Double precioBase, Integer peso, char consumoW,
                                   Integer pulgadas, boolean camaraITG) {
        super(precioBase, peso, consumoW);
        this.pulgadas  = pulgadas;
        this.camaraITG = camaraITG;
    }

    @Override
    public Double calcularPrecio() {
        Double precio = super.calcularPrecio();
        if (this.pulgadas > 40) precio += this.precioBase * 0.30;
        if (this.camaraITG)     precio += 50;
        return precio;
    }

    public Integer getPulgadas()  { return pulgadas; }
    public boolean isCamaraITG()  { return camaraITG; }
}
```

---

## -PrecioTotal.

En nuestra clase PrecioTotal encontraremos el arreglo que guarda todos los computadores recibidos en donación y los métodos para calcular y mostrar los totales separados por tipo: computadores en general, computadores de mesa y computadores portátiles.

## Codigo.

```java
public class PrecioTotal {

    private Double         totalComputadores           = 0.0;
    private Double         totalComputadoresPortatiles = 0.0;
    private Double         totalComputadoresMesa       = 0.0;
    private Computadores[] listaComputadores;

    public PrecioTotal(Computadores[] pComputadores) {
        this.listaComputadores = pComputadores;
    }

    public void mostrarTotales() {
        totalComputadores           = 0.0;
        totalComputadoresMesa       = 0.0;
        totalComputadoresPortatiles = 0.0;

        for (Computadores c : listaComputadores) {
            Double precio = c.calcularPrecio();
            totalComputadores += precio;
            if      (c instanceof ComputadoresMesa)       totalComputadoresMesa      += precio;
            else if (c instanceof ComputadoresPortatiles) totalComputadoresPortatiles += precio;
        }

        System.out.println("La suma del precio de los computadores es de "            + totalComputadores);
        System.out.println("La suma del precio de los computadores de mesa es de "    + totalComputadoresMesa);
        System.out.print(  "La suma del precio de los computadores portátiles es de " + totalComputadoresPortatiles);
    }
}
```

---

## -Main.

Por último en nuestro Main tendremos la creación de los objetos de cada tipo de computador, los cuales se guardan en un arreglo y se pasan a la clase PrecioTotal para calcular y mostrar los resultados en consola.

## Codigo.

```java
public class Main {

    public static void main(String[] args) {

        Computadores[] computadores = new Computadores[6];
        computadores[0] = new Computadores(150.0, 70, 'A');
        computadores[1] = new ComputadoresMesa(70.0, 40);
        computadores[2] = new ComputadoresPortatiles(600.0, 70, 'D', 50, false);
        computadores[3] = new Computadores();
        computadores[4] = new Computadores(500.0, 60, 'A');
        computadores[5] = new Computadores(700.0, 50, 'D');

        PrecioTotal solucion1 = new PrecioTotal(computadores);
        solucion1.mostrarTotales();
        System.out.println();

        Computadores[] computadores2 = new Computadores[4];
        computadores2[0] = new Computadores(60.0, 10, 'D');
        computadores2[1] = new ComputadoresMesa(300.0, 40, 'Z', 40);
        computadores2[2] = new ComputadoresPortatiles(50.0, 10, 'A', 70, false);
        computadores2[3] = new Computadores(50.0, 10);

        PrecioTotal solucion2 = new PrecioTotal(computadores2);
        solucion2.mostrarTotales();
        System.out.println();
    }
}
```

---

En cada clase se establecen los atributos y métodos necesarios para la ejecución del código por medio de la terminal. En un futuro se busca generar también una interfaz gráfica amigable y funcional para el registro de las donaciones.

## Como compilar y ejecutar

```bash
# Compilar todos los archivos
javac *.java

# Ejecutar el programa
java Main
```

## Resultado esperado

```
La suma del precio de los computadores es de 3000.0
La suma del precio de los computadores de mesa es de 130.0
La suma del precio de los computadores portátiles es de 910.0

La suma del precio de los computadores es de 715.0
La suma del precio de los computadores de mesa es de 350.0
La suma del precio de los computadores portátiles es de 175.0
```

## Autor

**[Tu nombre completo]**
[Tu carrera / curso]
[Tu institución] — MisiónTIC 2022
