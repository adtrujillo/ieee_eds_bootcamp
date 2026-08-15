# Lab 1: Implementa un diseno

- Paso 1: Crea una carpeta para ejecutar un flujo y cambiate a ella

```shell
mkdir librelane_flow
cd librelane_flow
```

Dentro de la carpeta *librelane_flow*, crea una carpeta llamada *designs* y cambiate a ella

```shell
mkdir designs
cd designs
mkdir counter_example
cd counter_example
```

- Paso 2: Copia el siguiente directorio:

```shell
cp -rL /foss/examples/demo_sky130A/dig/* .
```

Dentro de este directorio podremos encontrar diversos archivos, el archivo de RTL *counter.v*, la cama de pruebas de ese contador *counter_tb.v*, el archivo de configuracion de librelane *counter.json*, un archivo de configuracion de orden de puertos *pin_order.cfg* y un script de shell que limpia todo lo generado por librelane *_clean.sh*

Abrimos con un editor de texto el archivo *counter.json*

Dentro de este archivo podremos encontrar las variables de control del flujo de diseno en librelane, para mayor informacion de variables, consulta:
 https://librelane.readthedocs.io/en/latest/reference/common_flow_vars.html

LibreLane admite una variedad de formatos de archivos de configuración como .tcl, .json y .yaml. En este directorio de ejercicios puedes encontrar el archivo de configuración config.yaml con una configuración mínima:

```shell
DESIGN_NAME: counter
VERILOG_FILES: dir::counter.sv
CLOCK_PORT: clk_i
CLOCK_PERIOD: 10 # 10ns = 100MHz
```

Aquí tienes la traducción al español:

```DESIGN_NAME``` es el módulo de nivel superior (top-level) de tu diseño; 
en este caso, **counter**. ```VERILOG_FILES``` especifica todos los archivos fuente de tu diseño. Esto puede ser una lista de archivos o incluso un comodín (wildcard) como ```dir::path/to/my/files/*.sv```. 
```CLOCK_PORT``` es, el puerto de reloj de tu diseño, y ```CLOCK_PERIOD``` especifica el período de reloj al que debe operar el diseño. 

LibreLane utiliza esta información para configurar el archivo SDC (Synopsys Design Constraint) por defecto y ejecutar el STA (Static Timing Analysis o Análisis Estático de Tiempos). 
Para diseños más grandes o complejos, podría ocurrir que no se puedan alcanzar los 100 MHz, en cuyo caso LibreLane reportará un error.


# Ejecutando librelane

Revisamos que tengamos listo el pdk de sky130

```shell
sak-pdk sky130A 
```

Para comenzar a ejecutar la herramienta, necesitamos usar el siguiente comando:

```shell
librelane --run-tag primer_flujo  counter.json
```

Donde: --run-tag *nombre* es el nombre de la carpeta donde se ejecutara y guardara todo el flujo,
y el archivo *json* es el archivo de configuracion para ejecutar nuestro diseno acrorde a diferentes variables 



Empezaremos a ver que se van a ejecutar bastantes lineas y bastante informacion, por defecto, se creara
una carpera llamada *runs*, dentro de esa carpeta existe nuestra ejecucion llamada
*primer_flujo*

```shell
cd runs/primer_flujo
```

Encontraremos un promedio de casi 80 pasos.

Nos regresamos al directorio original

```shell
cd ../..
```

## Visualizando nuestro diseno en OpenROAD

LibreLane utiliza OpenROAD para realizar los pasos del diseño físico. También cuenta con una GUI (interfaz gráfica de usuario) con la que puedes visualizar y depurar un diseño.

Para abrir la GUI de OpenROAD, simplemente vuelve a ejecutar el mismo comando con algunos argumentos adicionales:

```shell
librelane counter.json --last-run --flow OpenInOpenROAD 
```

Como podremos observar, tenemos nuestro diseno visualizado en OpenROAD, a la derecha del diseno, podremos observar las entradas y salidas.

# Ejercicio

- Que pasa si cambiamos el *pin_order.cfg* de la siguiente manera:


```shell
#N

#S
@min_distance=0.5
o_out.*

#E

#W
i_clk
i_reset
```

**NOTA* Cambia el *pin_order.cfg* y vuelve a ejecutar

# Observando el layout

Para observar el layout, podremos hacer lo siguiente:

Abrimos con Klayout

```shell
librelane counter.json --last-run --flow OpenInKLayout
```

# Observando el Layout en 3D (Demostrativo)

Para hacer este paso, necesitamos una maquina con mas de 16 GB de memoria RAM

Consultamos y descargamos el siguiente archivo:

https://github.com/trilomix/GDS3D/blob/dc6d965225c9f5ed2a6faefb7ea30665429060fe/techfiles/sky130.txt


Y usamos el siguiente comando:

```shell
GDS3D -p sky130.txt -i ../runs/primer_flujo/final/gds/counter.gds
```




