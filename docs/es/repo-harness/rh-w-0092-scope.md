# Alcance propuesto RH-W-0092

**Estado: `completed`; el propietario autorizó la regla simple de trabajo el 2026-10-11 en vez de otro diseño o mecanismo.** El alcance siguiente es histórico. Entregado mediante el PR #331 (issue #330). El [registro de la hoja de ruta](roadmap.md#rh-w-0092) posee el ciclo de vida.

## Base verificada

El `.gitattributes` generado vigente ya protege cada ruta generada comparada por bytes con reglas exactas `text eol=lf`, y la verificación cubre `core.autocrlf=true`. No se debe reabrir ni rediseñar ese comportamiento sin un defecto medido.

## Dirección propuesta

Detectar y sugerir guía solo cuando una condición real de longitud de ruta de Windows afecte una operación. La opción de remediación que ejecuta la persona es:

```text
git config --global core.longpaths true
```

Cambia la configuración global de Git de la persona, por lo que nunca se ejecuta automáticamente, nunca es un prerrequisito, no corresponde fuera de Windows ni se sugiere si no existe un fallo relevante. No concede autoridad para modificar la configuración global.

## Límites

- Preservar el trabajo local ordinario cuando la guía no sea necesaria.
- Sin cambio de registro ni política del sistema operativo, campo de manifest, preferencia, estado, advertencia repetida de inicio, dependencia ni abstracción de plataforma.
- Sin cambios de producto, contrato generado, proyección de runtime, `.gitattributes`, configuración Git, pruebas, red ni entrega bajo esta unidad de redacción.

## Decisión final

El propietario aprobó el alcance bilingüe corregido. La evidencia exacta de detección y la redacción esperan un diseño autorizado por separado para detección y sugerencia condicionales solamente; la implementación y la entrega siguen sin autorización separada.
