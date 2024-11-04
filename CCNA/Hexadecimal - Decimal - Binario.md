   Usa 16 posibles dígitos
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F

A == 10
B == 11
C == 12
D == 13
E == 14
F == 15

**Importante**: 
- 1 Carácter hexadecimal == **4 bits**
- 2 Carácteres hexadecimales == 1 byte == 8 bits


#### Convertir binario a decimal

Teniendo un número binario "1000 1111"
Desde el último número ponemos un numero arriba, empezando por "1" que se irá multiplicando por 2 cada vez que saltemos al siguiente número, iría incrementando como: 128, 64, 32, 16, 8, 4, 2, 1
Ahora sumamos todos esos números que resultaron de la multiplicación, y que están emparejados con los "1" binarios, ignorar los 0, y nos da el resultado final.


#### Convertir decimal a binario
Se usará de ejemplo el numero "3476"
Pasos a seguir
1. Dividir "3476" en 2
	1. ¿Es una división entera? Poner un 0
	2. ¿Es una división inexacta? Poner un 1
2. Seguir dividiendo el numero resultante por 2
3. Dividir 1738 / 2
	1. Cabe entero, poner un 0
4. Dividir 869 / 2
	1. No cabe entero, poner un 1
5. dividir 434 / 2
	1. Cabe entero, poner un 0
6. Dividir 217 / 2
	1. no cabe entero, poner un 1
7. Dividir 108 / 2
	1. Cabe entero, poner un 0
8. Dividir 54 / 2
	1. Cabe entero, poner un 0
9. Dividir 27 / 2
	1. No cabe entero, poner un 1
10. Dividir 13 / 2
	1. No cabe entero, poner un 1
11. Dividir 6 / 2
	1. Cabe entero, poner un 0
12. Dividir 3 / 2
	1. No cabe entero, poner un 1
13. Dividir 1 / 2
	1. No cabe entero, poner un 1
14. Resultado es 0,75, no seguir dividiendo

Se debe dividir hasta que el numero restante llegue a **0**

128, 64, 32, 16, 8, 4, 2, 1

2
4
16
32
64