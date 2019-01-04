# Bad Smells

## O que são?

Bad smells - do inglês "mau cheiro" - são algumns hábitos que os programadores constumam desenvolver que acabam dificultando a compreensão e legibilidade dos seus códigos.

## Solução
Cada bad smells tem uma solução apropriada, sendo assim, a partir do momento que têm-se conhecimento de quais são - pelo menos - os principais bad smells, fazer um **refactoring** neste código para que a partir daí ele fique mais fácil de entender o que está sendo feito (ou seja, qual o comportamento daquele código).

## O que é um refactoring?
Fazer um refactoring é, basicamente, encontrar uma ocorrencia de bad smells, reescrever este bloco de código removendo a má prática anteriormente detectada sem mudar o comportamento do programa em questão.

## exemplos de bad smells

Nesta apresentação tratarei de dois casos de bad smells, são eles **duplicated code**, **long method** a frente veremos o que são e quais medidas devem ser tomadas ao detectar que seu código tem um destes.

### duplicated code

__Martin Fowler__ diz no livro _**Refactoring** Improving the design of existing code_: "se você vê as mesmas estruras de código em mais de um lugar, você pode ter certeza que vai ser melhor se você encontrar uma maneira de unificar isso".

O exemplo mais simples de duplicação de código é, em uma mesma classe, ter uma expressão em mais de um método a solução para isso é fazer uma extração do método e invocar o novo método no lugar do código duplicado, veja o exemplo abaixo:

```C++
#include <iostream>

using namespace std;

int main(int argc, char const *argv[]) {
	int arrayA[10], arrayB[15], sum_a, sum_b;

	for(int i = 0; i < 10; i++) {
		cin >> arrayA[i];
	}

	for(int i = 0; i < 15; i++) {
		cin >> arrayB[i];
	}

	sum_a = 0;
	for(int i = 0; i < 10; i++) {
		sum_a += arrayA[i];
	}
	cout << "The sum of elements in arrayA are " << sum_a << endl;

	sum_b = 0;
	for(int i = 0; i < 15; i++) {
		sum_b += arrayB[i];
	}
	cout << "The sum of elements in arrayB are " << sum_b << endl;

	return 0;
}
```

No código acima é possivel notar dois exemplos da duplicação de código, o primeiro é a mesma estrura para inicilizar o array com a leitura do usuário, para consertar esta parte aplicamos a extração do método, seguindo o passo-a-passo:
1. criar um método que mantenha tal comportamento
2. invocar este método no lugar dos trechos de código repetido
3. compile e teste garantindo que o código refatorado tenha o mesmo comportamento do código com bad smells
tendo isso em mente, o código ficaria da seguinte maneira:

```C++
#include <iostream>

using namespace std;

void read_array(int *array, int size_arr) {
	for(int i = 0; i < size_arr; i++) {
		cin >> array[i];
	}
}

int main(int argc, char const *argv[]) {
	int arrayA[10], arrayB[15];
	
	read_array(arrayA, 10);
	read_array(arrayB, 15);
	
	int sum_a = 0;
	for(int i = 0; i < 10; i++) {
		sum_a += arrayA[i];
	}
	cout << "The sum of elements in arrayA are " << sum_a << endl;
	
	int sum_b = 0;
	for(int i = 0; i < 15; i++) {
		sum_b += arrayB[i];
	}
	cout << "The sum of elements in arrayB are " << sum_b << endl;

	return 0;
}
```
Mesmo com o refatoramente do inicialização dos arrays, temos mais uma ocorrencia de **código duplicado** que é na soma dos elementos do array, usaremos a mesma solução, extrair um método para resolver isso, sendo assim, o código final refatorado será:

```C++
#include <iostream>

using namespace std;

void read_array(int *array, int size_arr) {
	for(int i = 0; i < size_arr; i++) {
		cin >> array[i];
	}
}

int sum_elements_array(int *array, int size_arr) {
	int sum_array = 0;
	for(int i = 0; i < size_arr; i++) {
		sum_array += array[i];
	}
	return sum_array;
}

int main(int argc, char const *argv[]) {
	int arrayA[10], arrayB[15];
	
	read_array(arrayA, 10);
	read_array(arrayB, 15);
	
	cout << "The sum of elements in arrayA are " << sum_elements_array(arrayA, 10) << endl;
	cout << "The sum of elements in arrayB are " << sum_elements_array(arrayB, 15) << endl;

	return 0;
}
```

Depois do refatoramento o mesmo comportamento foi apresentado, ou seja, para as mesmas entradas foi produzida a mesma saída.
