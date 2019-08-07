# Manual basico de lua

En esta manual aprenderemos lo basico de lua.
Vease tambien el [manual](https://www.lua.org/manual/5.1/es/manual.html) oficial de lua para mas informacion.

* Acerca de
    * Versiones
    * Instalación
    * Recomendaciones
    * ¿Que podemos hacer con lua?
* Sintaxis
    * "Hola Mundo" en Lua.
    * Comentarios.
    * Variables.
    * Tipos de datos.
    * Operadores.
        * Operadores aritméticos.
        * Operadores logicos.
        * Operadores relacionales.
    * Estructuras de control.
        * Condicionales.
        * Bucles.
    * Funciónes.
* Copyright.

## Acerca de

En la actualidad hay muchos lenguajes de programación, siendo posiblemente Java y C++ los más conocidos. Sin embargo, dichos 
lenguajes de programación pueden ser demasiado difíciles para empezar, debido a la gran cantidad de elementos a tener en cuenta 
para que un programa pueda ejecutarse, haciéndolos posiblemente puntos de partida un tanto difíciles.

Lua, en cambio, es un lenguaje mucho más sencillo de aprender y que puede ser un mejor punto de partida, debido a que necesita 
menos líneas de código para poder funcionar. Para ello vamos a mostrar el código de una frase en tres lenguajes diferentes.

"Hola Mundo" en C++:

```c++
#include <iostream>
using namespace std;

int main() {
    cout << "Hola Mundo " << endl;
    return 0;
}
```

"Hola Mundo" en Java:

```java
public class HolaMundo {
    public static void HolaMundo(String[] args) {
        System.out.print("Hola Mundo");
    }
}
```

"Hola Mundo" en Lua:

```lua
print("Hola Mundo")
```

## Versiones

Como cualquier lenguaje de programación interpretado, Lua evoluciona constantemente, estos cambios se distribuyen en versiones,
actualmente las versiones mas populares son la 5.0, 5.1, 5.2 y 5.3, personalmente prefiero la versión 5.1 que es una de las mas 
extendidas.

## Instalación

Debido a su gran popularidad del lenguaje su interprete se encuentra en la mayoría de las distribuciones de Linux.

Por lo que para su instalación pueden instalarlo con alguno de los siguientes comandos acorde a su distribución que estén utilizando.

Para los que son usuarios de Debian, Ubuntu, Linux Mint o cualquier sistema derivado de estos, solamente debemos de abrir una terminal y ejecutar en ella el siguiente comando:

`sudo apt install lua5.1`

Si son usuarios de Arch Linux, Manjaro, Antergos o cualquier distribución derivada de Arch Linux, podemos instalar el interprete desde los repositorios de AUR, para ello solamente debemos de teclear:

`aurman -S lua`

Mientras que para los que son usuarios de CentOS, RHEL, Fedora o cualquier distribución derivada de estas, lo podemos instalar con:

`sudo dnf install lua`

Comprobamos si esta instalado ejecutando en la consola (terminal):

`lua`

la salida seria algo como esto:

```
Lua 5.1.5  Copyright (C) 1994-2012 Lua.org, PUC-Rio
>
```

tambien puede crear un archivo cuya extencion seria .lua
para ejecutar dicho archivo seria de esta manera:

`lua archivo.lua`

## Recomendaciones

Para empezar a estudiar lua, recomiendo usar uno de los siguientes editores :

* [Textadept](https://foicica.com/textadept/) esta escrito en lua, por lo que esta orientado a la programacion de dicho lenguaje, lo uso para hacer script rapidos
* [Geany IDE](geany.org) es un buen IDE rapido y con una interfaz amigable, se puede personalizar como quieras, con soporte a lua, hice unas configuraciones para lua y la verdad quedo de maravilla, lo uso mas que todo para projectos grandes.
* [ZeroBrane](http://example.net/),creo es el mejor de todos ya que es un IDE que tiene todo lo que nesecitas para programar en lua

## ¿Que podemos hacer con lua?

Con lua podemos hacer desde aplicaciones de escritorio y paginas web hasta juegos.

## Sintaxis

Lua es un lenguaje de programación imperativo, estructurado y
bastante ligero que fue diseñado como un lenguaje interpretado con una semántica extendible.

### "Hola mundo" en Lua.

```lua
print("Hola Mundo")
```

la función `print`, inprime en pantalla lo que pasemos como parametro.

### Comentarios

Los comentarios en lua son los siguientes:

```lua
-- Esto es un comentario de una linea

--[[
 Este es un comentario multi linea
]]--
```

### Variables

Una variable es una palabra que almacena un valor.

`variable = valor`

en `lua` hay variables locales, globales y tablas.

```lua
variable = "global" -- lua reconoce todas las variables como globales a no ser, que la variable este declarada como local de esta manera
local variable = "local o privada"
-- arreglo o tabla simple
numeros = {"uno","dos","tres"} -- valores
-- arreglo asosiativo
informacion = {
    username = "diazvictor",
    email = "diaz.victor@openmailbox.org"
}
```

para acceder a una tabla o arreglo es de la siguiente manera:

`print(informacion.username)`

busco en la tabla informacion la variable llamada username y imprimo su valor. igual con la tabla arreglo.
la tabla numeros, contiene tres valores, "uno", "dos" y "tres", para acceder a uno de sus valores,
`numeros[1]`, numeros es la tabla y entre "corchetes" la posicion del valor.

### Tipos de datos

Lua es un lenguaje dinámicamente tipado. Esto significa que las variables no tienen tipos; sólo tienen tipo
los valores. No existen definiciones de tipo en el lenguaje. Todos los valores almacenan su propio tipo.

Todos los valores en Lua son valores de primera clase. Esto significa que todos ellos pueden ser
almacenados en variables, pueden ser pasados como argumentos de funciones, y también ser devueltos
como resultados.

En lua hay 8 tipos, pero veremos 6:_nil_,_boolean_,_number_,_string_,_function_,_table_. Nil es el
tipo del valor nil, cuya principal propiedad es ser diferente de cualquier otro valor; normalmente representa
la ausencia de un valor útil.El valor Boolean es false (falso) y true (verdadero). Tanto nil como
false hacen una condición falsa; Number representa números reales (en coma flotante y doble precision).
String representa una tira de caracteres. Lua trabaja con 8 bits: los strings pueden contener
cualquier carácter de 8 bits, incluyendo el carácter cero (0).

veamos un ejemplo usando variables:

```lua
-- tipo string
nombre = "Victor Díaz"
-- tipo number
edad = 16
nombre_completo = nombre .. " tiene" .. edad .. " años" -- los dos puntos (..) sirven para unir cadenas de caracteres con variables
-- tipo boolean
masculino = true
-- tipo nil o nulo
alien = nil
-- tipo float
decimal = 1730.3
-- tipo table/array
estados = {
    "Bolivar", 
    "Carabobo", 
    "Miranda", 
    "Zulia", 
    "Trujillo", 
    "Apure"
}
```

para comprobar que las variables son del tipo especificado usamos la funcion interna de lua `type`, que retorna un string que 
describe el tipo de un valor dado.

veamos un ejemplo:

```lua
print(type(nombre))
```

si colocamos la variable nombre, la salida sera la siguiente:

```
string
```

y asi puedes probar con las de mas variables.

## Operadores.

### Operadores aritméticos.

Son aquellos que permiten realizar operaciones matemáticas básicas, Lua tiene, los operadores aritméticos comunes:

| Operador | Propósito |
| -------- | --------- |
| + | adición |
| - | substracción |
| * | multiplicación |
| / | división |
| // | división  de piso |
| % | modulo |
| - | Negación (Unario) |

### Operadores logicos.

 Los operadores lógicos en Lua son and, or y not. Como las estructuras de control todos los operadores lógicos consideran false
y nil como falso y todo lo demás como verdadero.

El operador negación not siempre retorna false o true. El operador conjunción and retorna su primer operando si su valor
es false o nil; en caso contrario and retorna su segundo operando. El operador disyunción or retorna su primer 
operando si su valor es diferente de nil y false; en caso contrario or retorna su segundo argumento. 
Tanto and como or usan evaluación de cortocircuito; esto es, su segundo operando se evalúa sólo si es necesario.

| Operadores |
| ---------- |
| and        |
| or         |
| not        |

vease tambien [tabla de la verdad de operadores logicos](https://es.wikipedia.org/wiki/Tabla_de_verdad)

### Operadores relacionales.

Los operadores relacionales son usados para comparar dos valores, si la exprecion evaluada es correcta dará 
un resultado true (verdadero) de lo contrario sera false (falsa). 
Los operadores relacionales en Lua son:

| Operadores |       Uso     |     Nombre     | Descripción |
| ---------- | ------------- | -------------- | ----------- |
| ==         |     1 == "texto"    | Igualdad       | 1 es igual a "texto", no por que 1 es un valor de tipo number y "texto" es un valor de tipo "string", por lo tanto retorna un valor booleano en este caso false. |
| >          |         3 > 2       | Mayor que      | 3 es mayor que 2, si por lo tanto retorna true. |
| <          |         5 < 9       | Menor que      | 5 es menor que 9, por lo tanto retorna true. |
| >=         |        7 >= 7       | Mayor  o igual | 7 es mayor que 7, no pero si es igual, por lo tanto retorna true. |
| <=         |        9 <= 6       | Menor  o igual | 9 es menor que 6 o igual, no por lo tanto retorna false. |
| ~=         |    1 ~= "texto"     | Diferente      | 1 es diferente a "texto" que es un valor de tipo "string", si por lo tanto retorna true. | 

## Estructuras de control

Las estructuras de control if, while y repeat tienen el significado habitual.

Lua tiene también una sentencia for, en dos formatos.

### Condicionales

Para crear una condicional se utiliza la palabra reservada `if`.

ejemplo de sintaxis:

```lua
if ( condicion ) then
    ...
end
```

Si la condición se cumple (es decir, si su valor es true) se ejecutan todas las instrucciones que se encuentran 
dentro del bloque {...}. Si la condición no se cumple (es decir, si su valor es false) no se ejecuta ninguna instrucción
contenida en {...} y el programa continúa ejecutando el resto de instrucciones del script.

Ejercicio:

```lua
edad = 16

if edad == 16 then
    print "Tengo 16 años de edad."
else
    print "No tengo 16 años"
end

edad = 12

if (edad == 16) then
    print("Tengo " .. edad .. " años de edad.")
else
    print("No tengo " .. edad .. " años.")
end
```

El if comprueba si se cumple la condición (que puede estar entre paréntesis) e imprime en la pantalla (a través de una consola) 
lo que hay debajo del if (el cuerpo), en caso contrario imprime lo que hay debajo de else (el cuerpo), 
que viene a decir "si no". Como ves, los valores se asignan a las variables a través de un solo igual (=), mientras que 
para comprobar en las condicionantes hay que usar forzosamente doble igual (==). Además del doble igual, también se puede 
comparar utilizando mayor (>), menor (<), mayor o igual (>=), menor o igual (<=) y distinto (~=).

Se pueden establecer varias condiciones a la vez, pudiendo utilizar and, or y not.

Elaboraremos un script que me simule una clave de acceso. Si el usuario es: "admin" y la clave "123456" mostrara el mensaje "acceso permitido" caso contrario mostrara el mensaje "acceso denegado".

usaremos las funciones `print` y `io.read`, con `print` imprimimos en pantalla lo que se le pase como parametro y con `io.read`
escribiremos en patalla.

```lua
-- @Nota: la tilde no puede estar en una variable

print("Usuario:")
usuario = io.read() -- lo que insertemos cuando pida el nombre de usuario, lo guardamos en la variable usuario.
print("Contraseña:")
contrasena = io.read() -- lo que insertemos cuando pida la contraseña de usuario, lo guardamos en la variable contrasena.

if ( usuario == "admin" ) and (contrasena == "123456") then
    print("acceso permitido")
else
    print("acceso denegado")
end
```

ahora podemos hacer que cuando ingresen el usuario de "admin" pero la "contrasena" que imprima en pantalla "acceso denegado, su contraseña no es valida"
y al reves con "contrasena" y "admin" con otra condicion llamada elseif que ya la veremos:

```lua
print("Usuario:")
usuario = io.read() -- lo que insertemos cuando pida el nombre de usuario, lo guardamos en la variable usuario.
print("Contraseña:")
contrasena = io.read() -- lo que insertemos cuando pida la contraseña de usuario, lo guardamos en la variable contrasena.

if ( usuario == "admin" ) and (contrasena == "123456") then
    print("acceso permitido")
elseif (usuario == "admin") and (contrasena ~= "123456")
    print("acceso denegado, su contraseña no es valida")
elseif (usuario ~= "admin") and (contrasena == "123456")
    print("acceso denegado, su usuario no es valido")
else
    print("acceso denegado")
end
```

### Bucles

#### While

La estructura while ejecuta un simple bucle, mientras se cumpla la condición. Su definición formal es la siguiente:

Ejemplo de codigo:

```lua
while (condición) do
    -- cuerpo
end
```

digamos que tenemos una alarma y lo programamos para que comienze a las 0 horas, y cuando llegue a las 10 horas se detenga
y suene la alarma, bueno con este ejercicio lo hacemos.

la variable estado representa el estado de la alarma en este caso esta encendida, y la variable hora representa la hora en la que
inicia la alarma, en este caso a las 0 horas, bueno, el bucle se encargara de contar las horas hasta que finalice en este caso
a las 10 horas, dicho esto cuando el bucle llegue a dicha hora se detendra la alarma y mandara un mensaje.

Ejercicio:

```lua
estado = true
hora = 0

while (estado) do
	hora = (hora + 1)
	print("Son las" .. hora)
	if (hora >= 10) then
		estado = false
        print("Despierta")
	end
end
```

#### Repeat
La estructura repeat es parecida a la anterior con la diferencia que si la condición es verdadera el ciclo se interrumpe, 
en otros lenguajes se parece al do while.

Ejemplo:

```lua
repeat
    --cuerpo
until condición
```

bueno haremos el ejercicio anterior para que vean el parecido.

Ejercicio:

```lua
hora = 0

repeat
    hora = hora + 1
    print(hora)
    if (hora == 10) then
        print("Despierta")
    end
until hora == 10
```

#### For

La estructura for al igual que while repite un bloque de código siempre y cuando  sus condiciones se cumplan, en Lua se 
puede usar de dos formas.

##### Loop for numérico

Ejecuta el bloque con la variable igual a inicio, luego sigue incrementándola cantidad de pasos y ejecutando el bloque 
nuevamente hasta que es mayor que el valor de parada. la variable de paso se puede omitir y se establecerá de manera 
predeterminada en 1 (Se entenderá mejor en el ejemplo).

Ejemplo:

```lua
for variable = inicio, parada, paso do
    -- cuerpo
end
```

bueno haremos el ejercicio anterior para que vean el parecido.

Ejercicio:

```lua
for hora = 0 , 10, 1 do
    print("Son las " .. hora)
    if (hora == 10) then 
        print("Despierta")
    end
end
```

##### Loop for iterador

La versión del iterador `for` toma una función de iterador especial y puede tener cualquier cantidad de variables. 
se entendera mejor mas adelante.

Ejemplo:
```lua
for variable1, variable2, variable3 in iterador do
    -- cuerpo
end
```

En este ejercicio contaremos los habitantes de alguna ciudad, y la mostraremos en pantalla cual tiene mas y cual tiene menos, 
para ello crearemos una tabla bidimencional con el nombre de "ciudades", en esa tabla añadiremos cuatro variables
con el nombre de ciudad y su valor (el nombre, de la ciudad) y otras cuatro con el nombre de "habitantes" su valor es de tipo number(númerico)

en el bucle `for` la variable i seria el "indíce" y v el "valor", esas variables estaran asociadas
a la tabla ciudades donde i tomara el lugar de la "posicion de la ciudad" y v tomara el lugar de "ciudad o habitantes",
luego con una condicion verificamos cual variable tiene mas habitantes con la funcion `tonumber` le decimos al script que el valor
de "habitantes" debe ser un numero, luego el bucle dara vueltas hasta encontrar la ciudad con mas habitantes.

Ejercicio:

```lua
-- Arreglo Bidimensional

ciudades = {
    {ciudad = "Upata", habitantes = "1200"}, -- numero de indíce 1
    {ciudad = "Tumeremo", habitantes = "4000"}, -- numero de indíce 2
    {ciudad = "San felix", habitantes = "1500"}, -- numero de indíce 3
    {ciudad = "Puerto ordaz", habitantes = "1400"}  -- numero de indíce 4
}

-- i = indice de la ciudad
-- v = valor de la ciudad

for i, v in ipairs(ciudades) do
    if (tonumber(v.habitantes) > 1000 and tonumber(v.habitantes) < 1500) then
        print(i, v.ciudad," tiene ".. v.habitantes .. " habitantes")
    end
end
```

### Funciónes

Básicamente para cualquier lenguaje de programación, una estructura de control permite modificar el flujo de ejecución de instrucciones de un programa.

Lua cuenta con 4 estructuras de control, la estructura if, while, repeat y for.

Para crear una función en Lua, se utiliza la palabra reservada function , la estructura básica es la siguiente:

```lua
function nombreFuncion ()
    -- cuerpo
end
```

Pero también podemos declarar funciones como si se trataran de variables utilizando la siguiente estructura:

```lua
nombreFuncion = function ()
    -- cuerpo
end
```

#### Funciones con argumentos

Las funciones también pueden recibir argumentos, esto quiere decir que son parámetros o valores de 
entrada que son enviados hacia la función.

```lua
function nombreFuncion (argumento1, argumento2)
    -- cuerpo
end
```

Ejercicio basico:

```lua
function saludar ( nombre )
    print("Hola " .. nombre)
end

saludar("Victor")
```

Si ejecutamos nos devuelve el valor de:

`Hola Victor`

Como ejercicio complejo, haremos la famosa calculadora, estaremos usando la funcion `type` que retorna un string que 
describe el tipo de un valor dado, `io.read` escribiremos en patalla, `print` con la cual imprimimos en pantalla y condicionales.

nuestra funcion se llamara "calculadora" y recibira tres parametros, los cuales seran a que vendria siendo el primer valor,
op que seria el operador (+,-,*,/) y b que seria el segundo valor para hacer la operacion.

luego comprobaremos con la funcion `type` si el op (operador) es un string y el op es "+" sumamos a con a , si es "-" restamos 
a con b y asi con los demas,
si ninguna condicion se cumple, imprimimos en pantalla "el operador no es valido".

Ejercicio:

```lua
function calculadora(a, op, b)
    if (type(op) == 'string') then
        if (op == '+') then
            print(a + b)
        elseif (op == '-') then
            print(a - b)
        elseif (op == '/') then
            print(a / b)
        elseif (op == '*') then
            print(a * b)
        else
            print("el operador no es valido")
        end
    end
end

print("Introdusca primer valor")
a = io.read()
print("Introdusca operador")
op = io.read()
print("Introdusca tercer valor")
b = io.read()

print("El resultado es ")

calculadora(a, op, b)
```

## Copyright
- Email: diaz.victor@openmailbox.org
- Facebook: https://www.facebook.com/DiazUrbanejaVictor

## NOTA

Original en: https://github.com/diazvictor/manuales/blob/master/manual-basico-lua.md por si se pierde