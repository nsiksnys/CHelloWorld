# Fases de la Traducción y Errores

## hello2.c
**Preprocesamiento**
```
gcc hello2.c -std=c11 -Wall -E -o hello2.i
```

**Resultado:**
El archivo hello2.i está compuesto por el contenido de stdio.h y de hello2.c sin comentarios

## hello3.c
### Preprocesamiento
```
gcc hello3.c -std=c11 -Wall -E -o hello3.i
```

**Resultado:**
* En la primera línea se declara el método `printf`, que recibe obligatoriamente un puntero constante a una variable char, y opcionalmente una cantidad indefinida de parámetros extra.
* El archivo hello3.i no contiene a stdio.h porque, a diferencia de hello2.c, no se lo incluye en el código fuente. Además del contenido de hello3.c, hello3.i contiene comentarios relativos a la versión del compilador que se usa.

### Compilación
```
gcc hello3.i -std=c11 -Wall -c -o hello3.s
```

**Resultado:**
```
hello3.c: In function ‘main’:
hello3.c:11:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
hello3.c:11:2: error: expected declaration or statement at end of input
hello3.c:10:6: warning: unused variable ‘i’ [-Wunused-variable]
  int i=42;
      ^
```

* Advertencias:
	* Línea 10: la variable `i` está declarada pero no se usa
	* Línea 11: declaración implícita. Se llama a una función no declarada.
* Error: 
	* Línea 11: Se espera una declaración o una sentencia al final de la línea.

## hello4.c
### Preprocesamiento y compilación
```
gcc hello4.c -std=c11 -Wall -c -S -o hello4.s
```

**Resultado:**
```
hello4.c: In function ‘main’:
hello4.c:11:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
  prontf("La respuesta es %d\n");
  ^
hello4.c:10:6: warning: unused variable ‘i’ [-Wunused-variable]
  int i=42;
      ^
```
* Advertencias:
	* Línea 10: la variable `i` está declarada pero no se usa
	* Línea 11: declaración implícita. Se llama a una función no declarada.
* hello4.s contiene el código fuente traducido a la versión de Assembler propia del equipo en el que se trabaja

### Ensamblado sin vinculación
```
gcc hello4.s -std=c11 -Wall -c -o hello4.o
```

**Resultado:**
hello4.o contiene la traducción a código máquina de hello4.s

### Vinclulación y generación del ejecutable
```
gcc hello4.o -std=c11 -Wall -o hello4.out
```

**Resultado:**
hello4.out es el ejecutable resultado de vincular el contenido de hello4.o con la biblioteca estándar.

## hello5.c
### Preprocesamiento, compilación, ensamblado, vinculación, y generación de ejecutable
```
gcc hello5.c -std=c11 -Wall -o hello5.out
```

**Resultado:**
```
hello5.c: In function ‘main’:
hello5.c:11:9: warning: format ‘%d’ expects a matching ‘int’ argument [-Wformat=]
  printf("La respuesta es %d\n");
         ^
hello5.c:10:6: warning: unused variable ‘i’ [-Wunused-variable]
  int i=42;
      ^
   ```

* Advertencias:
	* Línea 10: no se usa la variable i
	* Línea 11: falta un argumento  de tipo `int` en la llamada a `printf`
* hello5.out es el ejecutable resultado de realizar el preprocesamiento, compilación, ensamblado y vinculación con la biblioteca estándar.

### Ejecución
```
./hello5.out
```
**Resultado:**
+ `La respuesta es 740417800`
+ Al ejecutar hello5.out `printf` toma como segundo argumento un valor cualquiera del segmento de datos correspondiente al proceso, lo formatea como int y lo muestra por la salida estándar (stdout).

## hello6.c
### Preprocesamiento, compilación, ensamblado, vinculación, y generación de ejecutable
```
gcc hello6.c -std=c11 -Wall -o hello6.out
```

**Resultado:**
hello6.out es el ejecutable resultado de realizar el preprocesamiento, compilación, ensamblado y vinculación con la biblioteca estándar.

## hello7.c
### Preprocesamiento, compilación, ensamblado, vinculación, y generación de ejecutable
```
gcc hello7.c -std=c11 -Wall -o hello7.out
```

**Resultado:**
```
hello7.c: In function ‘main’:
hello7.c:9:2: warning: implicit declaration of function ‘printf’ [-Wimplicit-function-declaration]
  printf("La respuesta es %d\n", i);
  ^
hello7.c:9:2: warning: incompatible implicit declaration of built-in function ‘printf’
hello7.c:9:2: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’
```
Declaración implícita: se llama a la función `printf` sin declararla o incluir `stdio.h`. Igualmente se genera el ejecutable vinculando con la función printf de la biblioteca estándar porque se permite por una cuestión de compatibilidad con código antiguo (previo a C11).

### Ejecución
```
./hello7.out
```

**Resultado:**
`La respuesta es 42`