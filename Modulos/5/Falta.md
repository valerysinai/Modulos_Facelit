# Módulo 5: Reconocimiento Facial

## Descripción General

El Módulo de Reconocimiento Facial constituye el núcleo funcional de FaceLit, ya que permite identificar a los usuarios mediante biometría facial para automatizar procesos de autenticación y control de asistencia dentro de los ambientes de formación del SENA.

Este módulo se encarga de:

* Registrar el rostro de los usuarios autorizados.
* Identificar usuarios mediante comparación biométrica.
* Registrar automáticamente eventos de asistencia.
* Enviar la información al servidor para su almacenamiento y trazabilidad.

La funcionalidad se basa en el uso de tecnologías de reconocimiento facial y procesamiento de imágenes, garantizando la validación de identidades mediante características biométricas únicas.

---

# RF-5 Reconocimiento Facial

## Objetivo

Permitir que el sistema capture, almacene y compare información biométrica facial para identificar usuarios y registrar eventos asociados a la asistencia.

Según el SRS, el sistema debe permitir que el usuario capture y registre su rostro para asociarlo con sus datos personales. Posteriormente, la imagen capturada será comparada con los registros almacenados para validar la identidad del usuario.

---

# RF-5.1 Registrar rostro del usuario

## Descripción

Permite capturar y almacenar la información biométrica facial de un usuario durante el proceso de registro.

El objetivo es generar una plantilla biométrica que quede asociada de manera única al perfil del usuario dentro del sistema.

## Entradas

* Imagen capturada desde la cámara.
* Identificador del usuario.
* Datos personales previamente registrados.

## Proceso

1. El usuario selecciona la opción Registrar Rostro.
2. El sistema activa la cámara.
3. Se detecta la presencia de un rostro humano.
4. Se captura la imagen facial.
5. Se validan condiciones mínimas de calidad.
6. Se genera la plantilla biométrica.
7. Se almacena la información facial asociada al usuario.

## Salidas

* Confirmación de registro facial exitoso.
* Asociación del rostro al perfil del usuario.

## Reglas de Negocio

* Solo se permiten rostros humanos.
* No se aceptan imágenes de objetos o animales.
* La captura debe realizarse en menos de un minuto.
* La imagen facial tendrá una vigencia máxima de 12 meses antes de requerir actualización.

## Criterios de Aceptación

* El sistema detecta correctamente un rostro humano.
* La imagen es almacenada correctamente.
* El rostro queda asociado al usuario.
* El sistema rechaza imágenes inválidas o no humanas.

---

# RF-5.2 Identificar usuario por reconocimiento facial

## Descripción

Permite verificar la identidad de un usuario comparando una imagen capturada en tiempo real contra los registros biométricos almacenados.

## Entradas

* Imagen facial capturada por la cámara.
* Base de datos de rostros registrados.

## Proceso

1. El usuario se posiciona frente a la cámara.
2. El sistema captura la imagen.
3. Se extraen características biométricas.
4. Se realiza la comparación contra los registros existentes.
5. Se calcula el porcentaje de coincidencia.
6. Se determina si la identidad es válida.

## Salidas

* Usuario identificado.
* Acceso concedido o denegado.
* Mensaje de reconocimiento exitoso.

## Reglas de Negocio

El sistema únicamente permitirá la identificación cuando la coincidencia biométrica sea igual o superior al 95 %.

## Criterios de Aceptación

* Coincidencia ≥ 95 % → Identidad validada.
* Coincidencia < 95 % → Identidad rechazada.
* El sistema debe funcionar en condiciones normales y de baja iluminación con una precisión mínima del 95 %.
* Debe detectar únicamente rostros humanos.

---

# RF-5.3 Enviar datos al servidor

## Descripción

Una vez identificado el usuario, el sistema debe registrar automáticamente el evento y enviar la información al servidor para su almacenamiento y posterior consulta.

Esta funcionalidad permite integrar el reconocimiento facial con el módulo de asistencias y reportes.

## Entradas

* Usuario identificado.
* Ambiente de formación.
* Fecha.
* Hora.
* Resultado del reconocimiento facial.

## Proceso

1. Se valida la identidad del usuario.
2. Se obtiene la fecha y hora del dispositivo o servidor.
3. Se identifica el ambiente asociado.
4. Se construye el registro de asistencia.
5. Se envían los datos al servidor mediante API.
6. El servidor almacena la información en la base de datos.

## Datos enviados

```json
{
  "usuarioId": "UUID",
  "ambienteId": "UUID",
  "fecha": "2026-06-24",
  "hora": "08:03:15",
  "estado": "ASISTENCIA"
}
```

## Salidas

* Registro almacenado exitosamente.
* Confirmación de asistencia.
* Actualización de historial.

## Relación con el SRS

El SRS establece que, después del reconocimiento facial, el sistema debe registrar automáticamente la fecha y hora del evento sin intervención manual.

Además, la información debe enviarse al historial de asistencias para su posterior visualización y generación de reportes.

## Criterios de Aceptación

* El usuario es identificado correctamente.
* La fecha y hora se registran automáticamente.
* El ambiente queda asociado al registro.
* La información se almacena en el servidor.
* El historial refleja inmediatamente el nuevo registro.

---

# Flujo General del Módulo

```text
Usuario
   ↓
Capturar Rostro
   ↓
Validar Rostro Humano
   ↓
Registrar Plantilla Biométrica
   ↓
Reconocimiento Facial
   ↓
Comparación Biométrica
   ↓
Coincidencia ≥ 95%
   ↓
Usuario Identificado
   ↓
Generar Registro de Asistencia
   ↓
Enviar Datos al Servidor
   ↓
Guardar en Base de Datos
   ↓
Actualizar Historial y Reportes
```

# Dependencias del Módulo

El módulo de Reconocimiento Facial depende directamente de:

* Módulo 1: Seguridad y Acceso.
* Módulo 3: Infraestructura - Ambientes e Inventario.
* Módulo 7: Instructores, Aprendices y Directivos.
* Módulo 8: Horarios, Observaciones e Incidencias.

Estas dependencias permiten asociar correctamente la identidad biométrica con el usuario, el ambiente y el horario correspondiente.
