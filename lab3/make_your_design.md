# Controla el flujo, crea tu propio chip

En este laboratorio, se vera como manipular las variables del flujo en librelane

Como referencia, estas son las variables que podemos modificar:

https://librelane.readthedocs.io/en/stable/reference/step_config_vars.html


## Establece un ```die_area```

Por defecto, Librelane establece tamano automatico del dado basado en tu diseno.

Si quieres definir un tamano del dado, necesitamos establecer la siguiente variable ```FP_SIZING: absolute```

Una vez que ya se haya establecido la variable anterior, podemos establecer un area, definela de la siguiente manera:

```DIE_AREA: [0, 0, 150, 150]```

Esto va a establecer un area de 0 a 150um de ancho, y de 0 a 150um de alto

Podemos establecer la densidad de nuestro diseno, con la variable ```PL_TARGET_DENSITY``` la cual especifica la densidad del diseno. Si tenemos una area grande con una densidad alta, posiblemente tengamos grupos de celdas estandar solos. (o muy al centro, o muy cerca de una zona)

Tu ```config.json``` debe de contener lo siguiente

```json
"FP_SIZING: "absolute"
"DIE_AREA: "[0, 0, 150, 150]"
"PL_TARGET_DENSITY": 0.8"
```

Si la densidad es muy alta, OpenROAD te lo va a decir, esto se soluciona cambiando a un valor mas bajo :)

## Ejercicio

Ahora cambia el die area a 202.08 de ancho, por 154.98 de alto!
