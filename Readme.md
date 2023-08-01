# Lambdaworks Stark Platinum Prover

<img src="https://github.com/lambdaclass/lambdaworks_stark_platinum/assets/569014/ad8d7943-f011-49b5-a0c5-f07e5ef4133e" alt="drawing" width="300"/>

Este Prover todavía está en desarrollo y puede contener errores. No está destinado a ser utilizado en producción por el momento.

Por favor, revisa los problemas bajo la etiqueta de seguridad y espera a que se resuelvan si son relevantes para tu proyecto.

La salida integrada (output builtin) está completa y se admite la comprobación de rango (range check), pero aún no es sólida.

Esperamos tener algo funcional y en buen estado para mediados de agosto de 2023.

## Principales bloques de construcción

- [STARKS](https://github.com/lambdaclass/lambdaworks_cairo_prover/tree/main/src/starks): Todo lo relacionado con los bloques de construcción de STARKs, como el probador, el verificador y FRI.
- [Cairo](https://github.com/lambdaclass/lambdaworks_cairo_prover/tree/main/src/cairo): Implementación del Cairo AIR.

Por añadir:

- Compatibilidad con la API de Winterfell
- Añadir parámetros para probar y verificar en la CLI (Las entradas públicas deben ser serializadas y deserializadas)
- Compilar Cairo dentro de Rust para probar y verificar Cairo1/Cairo2 desde el archivo .cairo, en lugar del archivo .casm
- Añadir la última restricción de Range Check Built In
- Añadir más paralelización
- Benchmark y optimizaciones para Graviton
- Builtin Bitwise
- Verificador de Cairo
   - Verificador de lote (Batch verifier) / Para árboles y N pruebas
- Soporte de Chiplet
- Cuda con Icicle para FTT/NTT
- Diferentes disposiciones (layouts)
- DSL Plonk
- Campos de extensión en Starks
- Corregir el error de seguridad "enforce selector"
- Corregir pruebas frente a otras bibliotecas de campos para obtener resultados más estables
- HyperPlonk - Ultraplonk
- Mejorar el perfilado con multihilo
- Serialización JSON para pruebas
- Optimizaciones
   - Omitir capas
   - Detener FRI
   - Consultas de FRI en lote (mejora el tamaño de la prueba)
   - Otras
- Backend optimizado para mini goldilocks
- Builtin Pedersen
- Elegir la configuración de hash con ProofOptions
- Builtin Poseidon
- Hash de Poseidon
   - Árbol de Poseidon
   - Árbol en lote de Poseidon
- Prueba de concepto de aplicación Wasm ejecutando el verificador
- Funciones de calidad de vida (to_decimal_string, from_decimal_string)
- Builtin Sha256
- Compatibilidad con Sharp
- Verificador de Solidity
- Soporte FFTx para CUDA
- Herramientas de trazado
- Columnas virtuales
- Soporte Vulkan para FFT
- API compatible con Winterfell

## Requisitos

- Cargo 1.69+

## Cómo probarlo

Por el momento, solo se admiten programas en Cairo 0 sin argumentos y contratos en Cairo 1 sin argumentos.

### Probar y verificar

Para probar programas Cairo, puedes usar:

```bash
make prove PROGRAM_PATH=<ruta_del_programa_compilado> PROOF_PATH=<ruta_de_salida_de_la_prueba>
```

Para verificar una prueba, puedes usar:

```bash
make verify PROOF_PATH=<ruta_de_la_prueba>
```

Por ejemplo:

```bash
make prove PROGRAM_PATH=fibonacci.json PROOF_PATH=fibonacci_proof
make verify PROOF_PATH=fibonacci_proof
```

Para probar y verificar con un solo comando, puedes usar:

```bash
make run_all PROGRAM_PATH=<ruta_de_la_prueba>
```

### Uso del compilador Docker para programas Cairo 0

Construye la imagen del compilador con:

```bash
make docker_build_cairo_compiler
```

Luego, por ejemplo, si tienes un programa Cairo en la carpeta del proyecto, puedes usar:

```bash
make docker_compile_and_run_all PROGRAM=nombre_del_programa.cairo
```

O

```bash
make docker_compile_and_prove PROGRAM=nombre_del_programa.cairo PROOF_PATH=ruta_de_la_prueba
```

### Uso de cairo-compile para programas Cairo 0

Si tienes `cairo-lang` instalado, puedes usarlo en lugar del Dockerfile.

Luego, por ejemplo, si tienes algún programa Cairo en la carpeta del proyecto, puedes usar:

```bash
make compile_and_run_all PROGRAM=nombre_del_programa.cairo
```

O

```bash
make compile_and_prove PROGRAM=nombre_del_programa.cairo PROOF_PATH=ruta_de_la_prueba
```

### Compilación de contratos Cairo 1

Clona el repositorio `cairo`:

``` bash
git clone https://github.com/starkware-libs/cairo
```

Selecciona la versión 1.1.0 (correspondiente a esa etiqueta del repositorio). En la carpeta `cairo`, ejecuta:

``` bash
git checkout v1.1.0
```

- Para crear un archivo json a partir de un contrato Cairo:

  ``` bash
  cargo run --bin starknet-compile -- /ruta/hacia/entrada.cairo /ruta/hacia/salida.json
  ```

- Para crear un archivo casm a partir de un archivo json:

  ``` bash
  cargo run --bin starknet-sierra-compile -- /ruta/hacia/entrada.json /ruta/hacia/salida.casm
  ```

## Ejecución de tests

Para ejecutar los tests, simplemente usa:

```bash
make test
```

Si tienes la cadena de herramientas `cairo-lang` instalada, esto compilará los programas Cairo necesarios para las pruebas.

Si has construido la imagen de docker de cairo-compile, se utilizará para la compilación en su lugar.

Asegúrate de construir la imagen de docker si no deseas instalar la cadena de herramientas `cairo-lang`:

```bash
make docker_build_cairo_compiler
```

## Ejecución de fuzzers

Para ejecutar un fuzzer, simplemente usa:

```bash
make fuzzer <nombre del fuzzer>
```

Si no tienes las herramientas de fuzzing instaladas, utiliza:

```bash
make fuzzer_tools
```