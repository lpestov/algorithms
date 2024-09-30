# Практическое занятие 1

## Практические советы по тестированию собственных реализаций

К тестированию собственных программ важно подходить очень ответственно и вдумчиво.

Вот категории тестов, которые стоит рассмотреть при тестировании:

**1. Тесты, описанные в условии задачи.**

Как правило, в условии задач даётся не менее одного примера с входными данными и верным ответом для них.
Эти тесты должны быть проверены в первую очередь, с высокой долей вероятности они есть в проверочном наборе.
Более того, на тестах из условия обычно можно проверить формат и вид выводимого ответа.
В данном случае нужно быть предельно внимательным - часто даже небольшое отличие (не хватает пробелов между элементами,
символы в разных регистрах) может привести к вердикту WRONG ANSWER, даже если по сути ответ правильный.

Простой пример: задача с обменом элементов

![image](https://github.com/il-bychkov/algorithms/assets/2277222/e8b7dc15-d03e-4a5d-9487-21999efe635e)


В данном случае мы видим что ответ предполагается одну единственну строку - нужно вывести элементы массива через пробел.
Это простой случай, однако в дальнейшем будут встречаться более сложные форматы вывода ответов, будьте внимательны.


**2. Тесты наименьшего размера/тривиальные тесты**
Самое простое что можно протестировать самостоятельно - тесты наименьшего размера.
Например, если речь идет о некоторой коллекции элементов, всегда нужно проверять случаи когда элементов всего 1 или даже 0 (конечно, если это предусмотрено интервалами входных параметров).
Часто на таких маленьких примерах программы даже ломаются, уходят в бесконечные циклы или даже получают ошибки при обращении к неразрешенному участку памяти.
В других случаях, для наименьших тестов может отличаться или не работать логика, корректная для остальных.

**Задание**: Придумать наименьшие тесты для всех задач контеста 0.

**Пример:** В задаче про обмены можно рассмотреть тесты с массивом из всего 1 элемента, в задаче про 2D массив - массив из одной строки, из одного столбца или совсем из 1 элемента.



**3. Тесты с наибольшими возможными данными**
Такие тесты обычно содержат граничные допустимые значения переменных задачи.
Например, максимально возможное число элементов в соответствии с условием. Или максимально большие значения элементов.
Тесты с наибольшими данными призваны проверить несколько вещей:
- укладывается ли предложенный Вами алгоритм в ограничение по времени
- укладывается ли предложенный Вами алгоритм в ограничение по памяти
- правильно ли выбраны типы данных внутри алгоритма, нет ли ошибок переполнения типов

Несмотря на то, что с опытом оценить время и память часто можно без использования дополнительных тестов, иногда бывает необходимо даже написать отдельную программу, которая генерирует большой тест.

Пример из задачи про суммы:

![image](https://github.com/il-bychkov/algorithms/assets/2277222/ef35bcb4-d89c-43d9-9d51-97ef6d1eefcd)

В задаче про суммы максимальными тестами можно считать:
- тесты с граничными значениями элементов $a = 10^9$, $a = -10^9$
- тесты массивов с максимальным числом элементов - например, массив из 500 строк и 500 столбцов
- оба пункта выше одновременно


**4. Тесты с различными типами ответов**
Иногда, корректному алгоритму необходимо концептуально по-разному реагировать на входные данные разных типов.
Например, для четного количества элементов массива логика может отличаться (из условия задачи) от логики для нечетного количества элементов.
Или, если речь идет о таком классе задач как игры, на часть примеров нужно будет ответить, например, "Выиграет первый игрок", а на другую "Выиграет второй игрок".
Важно придумать по несколько тестов для каждого возможного варианта ответов вашего алгоритма.



   
## Задачи на массивы из контеста 0

### Обмены

Общий алгоритм работы:
- идем циклом с начала до середины массива (или с конца до середины), пусть индекс текущего элемента = $i$
- берем равноудаленные элементы $a_i$, $a_{n-i}$ и обмениваем их значения

  Обмен возможен через два подхода:
  - с использованием переменной-буфера
  - с использованием различных бинарных операций

При использовании бинарных операций нужно быть аккуратным:
- вычитание, сложение, умножение/деление - работают лишь в специфических случаях, когда их результат умещается в тип данных. Более того, применимы лишь к целочисленным типам.
- Исключающее ИЛИ (XOR) горазде более гибкий способ, работает всегда. Применим к различным типам данных.

Далее LIVE CODING:

```c 
#include <stdio.h>

int main() 
{

    int a = 42;
    int b = 800;

    printf("initial state\n");
    printf("a = %d, b = %d\n", a, b);

    // works ONLY with integers and chars
    // POTENTIAL PROBLEM: OVERFLOW
    a = a + b;
    b = a - b;
    a = a - b;

    printf("after add-sub-sub swap\n");
    printf("a = %d, b = %d\n", a, b);

    // works ONLY with integers and chars
    // least efficient
    // POTENTIAL PROBLEM: OVERFLOW
    a = a * b;
    b = a / b;
    a = a / b;

    printf("after mul-div-div swap\n");
    printf("a = %d, b = %d\n", a, b);

    // works ONLY with integers and chars
    // POTENTIAL PROBLEM: OVERFLOW
    a = a - b;
    b = a + b;
    a = -a + b;

    printf("after sub-add-add swap\n");
    printf("a = %d, b = %d\n", a, b);

    // works with ANY data types
    // most efficient
    a = a ^ b;
    b = b ^ a;
    a = a ^ b;

    printf("after tripple xor swap\n");
    printf("a = %d, b = %d\n", a, b);

    return 0;
}
```
### Сумма двумерного массива


Общий алгоритм работы:
- выделяем место под хранение ответов - одномерные массивы размерами $n$ (для сумм строк) и $m$ (для сумм столбцов)
- организовываем проход по двумерному массиву, рассматривае элемент $a_{i,j}$
- каждый такой элемент участвует в сумме для $i$-й строки и $j$-го столбца
- увеличиваем накопленную сумму $i$-й строки на $a_{i,j}$ и накопленную сумму $j$-го столбца также на $a_{i,j}$

Далее LIVE CODING:
### Реализация 1 - тривиальная реализация
```c 
#include <stdio.h>
#include <stdint.h>

#define N_MAX 500
#define M_MAX 500

int main() 
{
    int n, m;
    scanf("%d", &n);
    scanf("%d", &m);

    // The most simple way is to allocate a big array, read all the number and then compute + print
    int64_t arr[N_MAX][M_MAX];

    // Two arrays to store our sums	
    int64_t sum_rows[N_MAX] = {0};
    int64_t sum_cols[M_MAX] = {0};		

    // Read the input array
    for(int i = 0; i < n; i++)	
    {
        for(int j = 0; j < m; j++)				
	{
            // lld - long long decimal IS IMPORTANT
	    scanf("%lld", &arr[i][j]);	
	}
    }

    // Now traverse the array and compute all the sums
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < m; j++)
        {	            
                sum_rows[i] += arr[i][j];
                sum_cols[j] += arr[i][j];            
        }
    }
	
    // Then just print the results

    printf("\n");
	
    for(int i = 0; i < n; i++)
    {
        // lld - long long decimal IS IMPORTANT	
        printf("%lld ", sum_rows[i]);
    }
    printf("\n");
    
    for(int i = 0; i < m; i++)
    {
        // lld - long long decimal IS IMPORTANT
        printf("%lld ", sum_cols[i]);
    }
    printf("\n");

    return 0;
}
```

### Реализация 2 - уменьшенное использование памяти
Изменения:
- основной алгоритм остается прежним
- убираем считывание исходных данных и хранение их в двумерном массиве
- сразу используем считываемый элемент для формирования сумм

Такой подход позволяет уменьшить затраты  времени и памяти программы.

Далее LIVE CODING:

```c 
#include <stdio.h>
#include <stdint.h>

#define N_MAX 500
#define M_MAX 500

int main() 
{
    int n, m;
    scanf("%d", &n);
    scanf("%d", &m);

    // Two arrays to store our sums
	
    int64_t sum_rows[N_MAX] = {0};
    int64_t sum_cols[M_MAX] = {0};

    // This algorithm does one single pass over all elements
    int64_t temp;
    for(int i = 0; i < n; i++)
    {
        for(int j = 0; j < m; j++)
        {		
            // Scanf current number and store it in int64_t variable. lld - long long decimal IS IMPORTANT	
            scanf("%lld", &temp);
            sum_rows[i] += temp;
            sum_cols[j] += temp;
        }
    }
	
    // Now just print all the sums computed

    printf("\n");
	
    for(int i = 0; i < n; i++)
    {
        // lld - long long decimal IS IMPORTANT	
        printf("%lld ", sum_rows[i]);
    }
    printf("\n");
    
    for(int i = 0; i < m; i++)
    {
        // lld - long long decimal IS IMPORTANT	
        printf("%lld ", sum_cols[i]);
    }
    printf("\n");

    return 0;
}
```

Пример сравнения работы двух реализаций

![image](https://github.com/il-bychkov/algorithms/assets/2277222/9c5f1bb3-7a08-481b-9ae7-ef0fcd594be5)


### Удаления и вставки

Общий алгоритм работы:
- реализовываем функциональность удаления и вставки элементов
- последовательно считываем текущие запросы, для каждого определяем тип запроса и вызываем соответствующую функциональность (удаление или вставку)
- выводим на экран текущий массив

Детали:
  - Лучший вариант реализации операций удаления, вставки и вывода на экран - реализация в виде отдельный функций
  - Также необходимо хранить актуальную длину массива и редактировать ее при выполнении модифицирующих операций

Обратите внимание:
- Для хранения текущей длины массива используется глобальная переменная.
При всём кажущемся удобстве глобальных переменных, в большинстве случаев их все-таки стоит избегать
Пример недостатков глобальных переменных:
    1. Сложно контролировать их изменение - глобальные переменные могут быть изменены из разных частей кода (из разных исходных файлов/заголовочных файлов).
    2. Создают проблемы с именами - нужно следить чтобы в других файлах не было таких  же имен. Если проект большой и людей много - это сложно.
    3. Ухудшают читаемость кода (приходится искать переменные в других файлах) и создают труднообнаруживаемые ошибки (следствие из пункта 1).
       
- При передаче массива в функции используется нотация с квадратными скобками и размером int arr[SIZE].
Такой способ передачи позволяет компилятору контролировать размер массива, который передается в функцию.
Если размер массива известен на стадии компиляции, этот способ помогает избежать некоторых ошибок.


**Реализация с глобальной переменной** (по мотивам решений Александра Ларькова и  Михаила Железина):

Далее LIVE CODING:

```c 
#include <stdio.h>

#define SIZE 350
int n;

// We will need to print an array several times, so lets define a function 
void print(int arr[SIZE]) 
{
    for (int i = 0; i < n; ++i) 
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// We will need to insert an element to the array several times, so lets define a function
void insert(int arr[SIZE], int index, int val) 
{
    for (int i = n - 1; i >= index; --i) 
    {
        arr[i + 1] = arr[i];
    }
    ++n;
    arr[index] = val;
}

// We will need to remove an element from the array several times, so lets define a function
void delete(int arr[SIZE], int index) 
{
    for (int i = index; i < n - 1; ++i) 
    {
        arr[i] = arr[i + 1];
    }
    --n;
}

int main() 
{
    int arr[SIZE] = {0};
    scanf("%d", &n);

    for (int i = 0; i < n; ++i) 
    {
        scanf("%d", &arr[i]);
    }

    int m;
    scanf("%d", &m);

    for (int i = 0; i < m; ++i) 
    {
        int t;
        scanf("%d", &t);
        if (t == 0) 
        {
            // If request == insert - call insertion function
            int index, val;
            scanf("%d%d", &index, &val);
            --index;
            insert(arr, index, val);
        } else 
        {
            // If request == delete - call deletion function
            int index;
            scanf("%d", &index);
            --index;
            delete(arr, index);
        }
        // Print current array
        print(arr);
    }
    return 0;
};
```

**Реализация без глобальной переменной**:

Обратите внимание:
- по сравнению с предыдущей реализацией для хранения текущей длины массива используется _локальная_ переменная (в данном случае - переменная внутри main).
Для того чтобы ей воспользоваться нам придется передавать её адрес в нужные функции чтобы они могли записывать/читать именно из этой переменной.
Изменять/читать данную переменную будем с помощью оператора разыменования.
       
- При передаче массива в функции используется нотация с обычным указателем, без размера (например, int* arr).
Такой способ тоже подойдет, еще он подойдет в ситуациях, когда размер массива становится известен лишь во время выполнения программы.

- объявление переменных для считывания запросов (index, t, val) лучше вынести вне циклов, чтобы переменная не объявлялась каждый раз заново

Далее LIVE CODING (но можно немного покопировать/изменить прошлый код):

```c 
#include <stdio.h>
 
#define SIZE 350

// Function recieves an array pointer and pointer to the size variable
void print(int* arr, int* size)
{
    for (int i = 0; i < *size; ++i)
    {
        printf("%d ", arr[i]);
    }
    printf("\n");
}


// Function recieves array pointer, insertion index, value to insert and a pointer to array size (we will need to modify it)
void insert(int* arr, int index, int val, int* size)
{
    for (int i = *size - 1; i >= index; --i)
    {
        arr[i + 1] = arr[i];
    }    
    arr[index] = val;
    *size += 1;
}

// Function recieves array pointer, index to delete and a pointer to array size (we will need to modify it)
void delete(int* arr, int index, int* size)
{
    for (int i = index; i < *size - 1; ++i)
    {
        arr[i] = arr[i + 1];
    }    
    *size -= 1;
}
 
int main() 
{
    int n;
    int arr[SIZE] = {0};
    
    // Read input data
    scanf("%d", &n); 
    for (int i = 0; i < n; ++i) 
    {
        scanf("%d", &arr[i]);
    }
 
    int m;
    scanf("%d", &m);
    
    // Define variables to store input values from requests
    int t;
    int index, val;
    
    for (int i = 0; i < m; ++i) {        
        scanf("%d", &t);
        if (t == 0) {    
            // If request == insert - call insertion function
            scanf("%d%d", &index, &val);
            --index;
            insert(arr, index, val, &n);
        } else {            
            // If request == delete - call deletion function
            scanf("%d", &index);
            --index;
            delete(arr, index, &n);
        }
        print(arr, &n);
    }
    return 0;
};
```

