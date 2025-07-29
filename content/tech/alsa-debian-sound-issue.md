---
title: "Solución para problemas de sonido ALSA en Debian Wheezy"
author: "Vicente Manuel Muñoz Milchorena"
date: 2012-12-04
tags: ["linux", "debian", "alsa", "audio", "troubleshooting"]
categories: ["tutoriales", "linux"]
description: "Guía paso a paso para resolver problemas de sonido con ALSA y Alsamixer en Debian Wheezy cuando aparecen errores de configuración."
---

# Solución para problemas de sonido ALSA en Debian Wheezy

## Descripción del problema

Para obtener sonido en Debian Wheezy cuando se tienen problemas con ALSA y Alsamixer marca un error continuo al momento de encender, donde aparece algo como esto:

```bash
root@sleeper:/etc# sudo /etc/init.d/alsa-utils restart
[....] Shutting down ALSA...warning: 'alsactl store' failed with error message 'alsactl: get_control:250: Cannot read control info '2,0,0,Front Playback Volume,0': [FAILid argument'...failed.
[....] Setting up ALSA...amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
amixer: Mixer hw:0 load error: Invalid argument
```

## Solución paso a paso

### Paso 1: Identificar la tarjeta de sonido

Primero necesitamos identificar cuál es la tarjeta de sonido que estamos usando. Para esto, ejecutamos el siguiente comando:

```bash
cat /proc/asound/cards
```

Este comando nos mostrará las tarjetas de sonido disponibles y sus identificadores.

### Paso 2: Crear el archivo de configuración asound.conf

Ir al directorio `/etc` y crear un archivo con el nombre `asound.conf` con la siguiente información:

#### Opción A: Usando número de tarjeta

```bash
# /etc/asound.conf
pcm.!default {
    type hw
    card 2
}
ctl.!default {
    type hw          
    card 2
}
```

#### Opción B: Usando nombre de tarjeta (recomendado)

**Nota importante:** Debemos tener cuidado ya que usar números es peligroso si se tiene más de una tarjeta, porque este orden puede cambiar al momento de iniciar. Lo que se puede hacer es usar el nombre entre corchetes para que así se pueda identificar más fácil.

En mi caso, quedaría algo como esto:

```bash
# /etc/asound.conf
pcm.!default {
    type hw
    card NVidia
}
ctl.!default {
    type hw          
    card NVidia
}
```

De esta forma dejamos claro cuál es la tarjeta que queremos y esta quedará por defecto declarada para cuando reinicie, o inicie, el servicio de ALSA.

### Paso 3: Comandos adicionales de configuración

Para finalizar, están unos comandos más que se deben ejecutar desde la terminal. Estos comandos no garantizan que funcione, pero es una aproximación adicional:

```bash
echo "options snd-hda-intel model=generic" >> /etc/modprobe.d/alsa-base.conf
```

```bash
/etc/init.d/alsa-utils stop
```

```bash
alsa force-reload
```

```bash
/etc/init.d/alsa-utils start
```

### Verificación del resultado

Si todo sale bien, este mensaje debería ser visible:

```bash
[ ok ] Setting up ALSA...done.
```

## Resumen

Esta solución resuelve los problemas de configuración de ALSA en Debian Wheezy mediante:

1. **Identificación correcta** de la tarjeta de sonido
2. **Creación del archivo** `/etc/asound.conf` con la configuración apropiada
3. **Aplicación de comandos** adicionales para forzar la recarga de la configuración
4. **Verificación** de que el servicio funcione correctamente

La clave está en usar el nombre de la tarjeta en lugar del número para evitar problemas cuando el sistema detecte las tarjetas en diferente orden al reiniciar.
