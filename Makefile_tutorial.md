# Makefile tutorial 
[info source](https://www.youtube.com/playlist?list=PLTd5ehIj0goOrqKZPvq1Np-8PUFcQSSm-)

Un `Makefile` es un archivo en el que vamos a declarar una serie de **reglas** que nos van a decir cómo hacer algo.

Las reglas tienen este formato:
```(makefile)

# comentario a la regla 1
objetivo_1 : dependencias que tiene el objetivo_1
<Tab>instrucciones para lograr el objetivo_1

# comentario a la regla 2
objetivo_2 : dependencias que tiene el objetivo_2
<Tab>instrucciones para lograr el objetivo_2

# comentario a la regla 3
objetivo_3 : dependencias que tiene el objetivo_3
<Tab>instrucciones para lograr el objetivo_3

# comentario a la regla n
objetivo_n : dependencias que tiene el objetivo_n
<Tab>instrucciones para lograr el objetivo_n

```

<img width="606" alt="image" src="https://github.com/luismiguelcasadodiaz/42_cursus/assets/19540140/e090f2c0-8a85-4614-a71b-e4489824f1dc">

## Variables
``` Makefile
OBJS = main.o salida.o calculadora.o
BINARY = programa
CFLAGS = -g -Wall -Wextra -Werror 
```

El valor de una variable puede ser introducido en otra variable
```
x = hola
y = $(x) mundo
```

EL contenido de `y` es Hola mundo"

Con `@` se silencian las instrucciones

### Variables de expansión recursiva: se definen con `=`.
La expansión se produce en el momento del acceso a la variable o en la ejecución.

### Variables de expansión simple: Se definen con `:=` o `::=` .
La expansión se produce en el momento de la asignación

Para recuperar el contenido de una variable usamos el dólar y los paréntesis.

``` Makefile
gcc $(CFLAGS) -o $(BINARY) $(CFLAGS)
```

## Reglas implicitas
[manual](https://www.gnu.org/software/make/manual/make.html#Implicit-Rules)
En función de las extensiones de las dependencias del objetivo, `make` ya sabe que instrucciones tiene que hacer para lograr el objetivo.

Así, por ejemplo

<img width="324" alt="image" src="https://github.com/luismiguelcasadodiaz/42_cursus/assets/19540140/b01e0634-5ebb-44a5-99f2-e3d27360b4ab">


En el caso de c, la regla de compilación implicita es :  `‘$(CC) $(CPPFLAGS) $(CFLAGS) -c’` y se ejecutan las instrucciones sin haberlas definido.

<img width="424" alt="image" src="https://github.com/luismiguelcasadodiaz/42_cursus/assets/19540140/3572dfaf-1dba-48ae-97dc-81b5bf344b6a">

## Reecompilaciones
Quedan definidad por las fechas de modificación de los ficheros.
Cuando una dependencia moderna ha dejado obsoleto a un objetivo, entonces se aplican las instrucciones para reconstruir el objetivo.

## Patrones y variable 
Los patrones nos premiten usar las reglas de una forma más flexible.

operador `%`. Es un operador que se remplaza por uno o más caracteres.

```Makefile
%.o: %.c
```
quiere decir que a partir de cualquie archivo punto c, puedo generar su punto o.
cualquier objeto terminado en punto o depende de un archivo punto c.

Hay un conjunto de [variables automaticas](https://www.gnu.org/software/make/manual/make.html#Automatic-Variables) que se crean al definir los patrones

`$<` se refiere a la primera dependencia de la regla
`$?` se refiere a todas las dependencias de la regla
`$@` se refiere al objetivo

Combinando patrones y variables automáticas .....
```Makefile
%.o: %.c
  $(CC) $(CFLAGS) $< -o $@
```


Dina Zhuzhleva explained me this trick to avoid relinks when i have two project-parts that affect the same target.
In this case, in the second part, i have to add bonus functions to same library where i added first part functions.
i generate my own timestamp to avoid relink again once i added object to de library because library becomes more updted than oobjects. 
```Makefile
bonus: .bonus

.bonus: $(OBJS) $(BONUS_OBJS) 
	@echo "================ GATHERING BONUS OBJECTS ====================="
	ar rcs $(NAME) $(OBJS) $(BONUS_OBJS) 
	touch .bonus
```
