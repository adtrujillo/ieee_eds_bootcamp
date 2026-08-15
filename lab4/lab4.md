# Exporta un diseno!

Para este laboratorio, exportemos un diseno de la siguiente pagina:

https://github.com/freecores

Usemos el i2c: 

https://github.com/freecores/i2c

Es un core simple, no tan grande y no requiere de alta capacidad de computo, para ello, clonemos el repositorio:

```shell
git clone https://github.com/freecores/i2c.git i2c
```

copiamos el archivo json de nuestro ejemplo del contador, y tambien el archivo de configuracion de puertos

Le cambiamos el nombre a nuestro archivo json

Y lo configuramos de la siguiente manera:

```json
{
    "DESIGN_NAME": "i2c_master_top",
    "VERILOG_FILES": "dir::i2c/rtl/verilog/*",
    "CLOCK_PORT": "wb_clk_i",
    "CLOCK_PERIOD": 20,
    "IO_PIN_ORDER_CFG": "dir::pin_order.cfg"
}
```

Este diseno contiene:

- 18 entradas, 13 salidas

por lo tanto, el archivo ```pin_order.cfg``` quedara de la siguiente forma

```
#N

#S

#E
@min_distance = 0.4
wb_clk_i
wb_rst_i
arst_i
wb_addr.*
wb_dat_i.*
wb_stb.*
wb_cyc.*
scl_pad_i*
sda_pad_i*

#W
wb_dat_o.*
wb_ack_o.*
wb_inta.*
scl_pad_o
scl_padoen_o
sda_pad_o
sda_padoen_o
```

## Ejercicios

1) Define un area, la que tu quieras
2) Ajusta la frecuencia de ser necesario, sube un 10 porciento de la frecuencia original.
3) Ajusta la frecuencia de ser necesario, baja un 10 porciento de la frecuencia original.
4) Cual es la maxima frecuencia que soporta este diseno
5) Cambia de tecnologia :)