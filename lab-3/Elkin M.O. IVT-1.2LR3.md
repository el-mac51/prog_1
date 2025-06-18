# Лабораторная работа №3
## Тема лабораторной работы: Структуры. Объединения. Перечисления.

# Комплект 1. Структуры

## Задание 1.1
### Постановка задачи:
Создать некоторую структуру с указателем на некоторую функцию в качестве поля. Вызвать эту функцию через имя переменной этой структуры и поле указателя на функцию.
### Список идентификаторов:

| **Переменная**  | **Тип**                     | **Суть**                                                                 |
|-----------------|-----------------------------|--------------------------------------------------------------------------|
| `n`             | `int`                       | Входное число, для которого вычисляется факториал                        |
| `result`        | `unsigned long long`        | Переменная для хранения результата вычисления факториала                |
| `i`             | `int`                       | Счетчик цикла для вычисления факториала                                 |
| `factorial`     | `void (int)`                | Функция, вычисляющая и выводящая факториал числа `n`                    |
| `Maths`         | `struct`                    | Структура, содержащая указатель на функцию                              |
| `function`      | `void (*)(int)`             | Указатель на функцию внутри структуры `Maths`                           |
| `calc`          | `struct Maths`              | Экземпляр структуры `Maths`, используемый для вызова функции `factorial` |

### Код программы:

```c
#include <stdio.h>

// функция для вычисления и вывода на экран факториала числа
void factorial(int n)
{
    if(n < 0)
    {
        printf("ERROR! Negative value.");
        return;
    }

    unsigned long long result = 1;

    for(int i = 1; i <= n; i++)

        result *= i;

    printf("Factorial %d = %llu\n", n, result);
}

// структура с указателем на некоторую функцию
struct Maths
{
    void (*function)(int);
    // void - так как функция ничего не возвращает

    // (*function) - указатель на функцию

    // (int) - тип принимаемого аргумента функции
};

// к данному указателю можно привязать любую подходящую по типу и аргументу функцию

int main()
{
    struct Maths calc; // создание экземпляра структуры

    calc.function = factorial; // назначение функции к указателю
    calc.function(5); // вызов функции через структуру

    return 0;
}
```
### Результаты работы программы:

![[рез31.png]]
## Задание 1.2
### Постановка задачи: 
Создать структуру для вектора в 3-х мерном пространстве. Реализовать и использховать в своей программе следующие операции над векторами: 
• скалярное умножение векторов; 
• векторное произведение; 
• модуль вектора; 
• распечатка вектора в консоли. 
В структуре вектора указать имя вектора в качестве отдельного поля этой структуры.
### Список идентификаторов:

| **Переменная**  | **Тип**                     | **Суть**                                                                 |
|-----------------|-----------------------------|--------------------------------------------------------------------------|
| `Vector3D`      | `struct`                    | Структура, описывающая вектор в 3D-пространстве                         |
| `name`          | `char[10]`                  | Имя вектора (максимум 10 символов)                                      |
| `x`             | `double`                    | Координата вектора по оси X                                             |
| `y`             | `double`                    | Координата вектора по оси Y                                             |
| `z`             | `double`                    | Координата вектора по оси Z                                             |
| `a`             | `Vector3D`                  | Первый вектор в примере (экземпляр `Vector3D`)                          |
| `b`             | `Vector3D`                  | Второй вектор в примере (экземпляр `Vector3D`)                          |
| `scalar_vec`    | `double(const Vector3D*, const Vector3D*)` | Функция, вычисляющая скалярное произведение двух векторов         |
| `product_vec`   | `Vector3D(const Vector3D*, const Vector3D*)` | Функция, вычисляющая векторное произведение двух векторов    |
| `result`        | `Vector3D`                  | Временный вектор, хранящий результат векторного произведения           |
| `module_vec`    | `double(const Vector3D*)`   | Функция, вычисляющая модуль (длину) вектора                             |
| `print_vec`     | `void(const Vector3D*)`     | Функция, выводящая вектор в формате `Имя: (x, y, z)`                   |
| `scal`          | `double`                    | Переменная для хранения скалярного произведения векторов `a` и `b`     |
| `prod`          | `Vector3D`                  | Переменная для хранения векторного произведения векторов `a` и `b`     |

### Код программы:

```c
#include <stdio.h>
#include <math.h>

// объявление структуры для векторов в трехмерном пространстве

typedef struct

{
    char name[10];
    double x;
    double y;
    double z;
} Vector3D;

// функция для скалярного произведение векторов, возвращает число

double scalar_vec(const Vector3D *a, const Vector3D *b) // на вход подаются указатели на структуры
{
    return a->x * b->x + a->y * b->y + a->z * b->z; // доступ к полям через указатели
}
// функция для произведение векторов, возвращает вектор
Vector3D product_vec(const Vector3D *a, const Vector3D *b)
{
    Vector3D result = { // создается новый экземпляр типа 'Vector3D'
        "C",
        a->y * b->z - a->z * b->y,
        a->z * b->x - a->x * b->z,
        a->x * b->y - a->y * b->x
    };

    return result;
} 

// функция для получения модуля вектора, возвращает число
double module_vec(const Vector3D *a)
{
    return sqrt(a->x * a->x + a->y * a->y + a->z * a->z);
} 

// функция для печати вектора

void print_vec(const Vector3D *a)
{
    printf("%s: (%.2f, %.2f, %.2f)\n", a->name, a->x, a->y, a->z);
}

int main()
{
    Vector3D a = {"A", 2.0, 3.0, -1.0};
    Vector3D b = {"B", -1.0, 4.0, 0.0};  
    printf("Initial vectors:\n");
    print_vec(&a);
    print_vec(&b);
    printf("\n");
  
    double scal = scalar_vec(&a, &b);
    printf("Scalar product of vectors %s and %s: %.2f\n", a.name, b.name, scal);
  
    Vector3D prod = product_vec(&a, &b);
    printf("Product of vectors %s and %s is vector ", a.name, b.name);
    print_vec(&prod);
  
    printf("Module of vector %s: %.2f\n", a.name, module_vec(&a));
    printf("Module of vector %s: %.2f", b.name, module_vec(&b));
    return 0;
}
```
### Результаты работы программы:

![[рез32.png]]

## Задание 1.3
### Постановка задачи: 
Вычислить, используя структуру комплексного числа, комплексную экспоненту $exp(z)$ некоторого $z ∈ C$:$$\exp(z) = 1 + z + \frac{1}{2!}z^2 + \frac{1}{3!}z^3 + \ldots + \frac{1}{n!}z^n$$
### Список идентификаторов:

| **Переменная**       | **Тип**                     | **Суть**                                                                 |
|-----------------------|-----------------------------|--------------------------------------------------------------------------|
| `Complex`             | `struct`                    | Структура, представляющая комплексное число                              |
| `r`                   | `double`                    | Действительная часть комплексного числа                                 |
| `i`                   | `double`                    | Мнимая часть комплексного числа                                          |
| `complexAdd`          | `void(const Complex*, const Complex*, Complex*)` | Функция сложения двух комплексных чисел                  |
| `complexMultiply`     | `void(const Complex*, const Complex*, Complex*)` | Функция умножения двух комплексных чисел                |
| `complexScalarMultiply` | `void(const Complex*, double, Complex*)` | Функция умножения комплексного числа на скаляр       |
| `complexScalarDivide` | `void(const Complex*, double, Complex*)` | Функция деления комплексного числа на скаляр         |
| `complexExp`          | `void(const Complex*, int, Complex*)` | Функция вычисления экспоненты комплексного числа (ряд Тейлора) |
| `z`                   | `Complex`                   | Входное комплексное число в функции main                                |
| `result`              | `Complex`                   | Результат вычислений (в main и временный в complexExp)                  |
| `n`                   | `int`                       | Количество членов ряда Тейлора для вычисления exp(z)                    |
| `sum`                 | `Complex`                   | Накопительная сумма ряда в функции complexExp                           |
| `term`                | `Complex`                   | Текущий член ряда в функции complexExp                                  |
| `temp`                | `Complex`                   | Временная переменная для хранения промежуточных вычислений              |
| `k`                   | `int`                       | Счетчик цикла в функции complexExp                                      |

### Код программы:

```c
#include <stdio.h>

typedef struct
{
    double r; // действительная часть числа

    double i; // мнимая часть числа

} Complex;

void complexAdd(const Complex *a, const Complex *b, Complex *result)
{
    result->r = a->r + b->r;

    result->i = a->i + b->i;
}

void complexMultiply(const Complex *a, const Complex *b, Complex *result)
{
    double r_part = a->r * b->r - a->i * b->i;
    double i_part = a->r * b->i + a->i * b->r;
    result->r = r_part;
    result->i = i_part;
} 

void complexScalarMultiply(const Complex *a, double scalar, Complex *result)
{
    result->r = a->r * scalar;
    result->i = a->i * scalar;
} 

void complexScalarDivide(const Complex *a, double scalar, Complex *result)
{
    result->r = a->r / scalar;
    result->i = a->i / scalar;
}

  
void complexExp(const Complex *z, int n, Complex *result)
{
    Complex sum = {1.0, 0.0}; // начальная сумма (1 + 0i)
    Complex term = {1.0, 0.0}; // текущий член ряда
  
    for (int k = 1; k <= n; k++) {
        Complex temp;
        // умножаем текущий член на z
        complexMultiply(&term, z, &temp);
        
        // делим на k и обновляем term
        complexScalarDivide(&temp, k, &term);
        
        // добавляем term к сумме
        complexAdd(&sum, &term, &sum);
    }

    *result = sum;
}

int main()
{
    Complex z = {1.0,1.0};
    Complex result;
    int n = 10; // количество членов ряда
  

    complexExp(&z, n, &result);


    printf("For a complex number z = %.1lf + %.1lfi, exp(z) = %.6lf + %.6lfi", z.r, z.i, result.r, result.i);
  
    return 0;
}
```
### Результаты работы программы:

![[рез33.png]]
## Задание 1.4
### Постановка задачи: 
Используя так называемые "битовые" поля в структуре C, создать экономную структуру в оперативной памяти для заполнения даты некоторого события, например даты рождения человека. 
### Список идентификаторов:

| **Переменная**  | **Тип**                     | **Суть**                                                                 |
|-----------------|-----------------------------|--------------------------------------------------------------------------|
| `Birthday`      | `struct`                    | Структура для хранения даты рождения с битовыми полями                   |
| `name`          | `char[20]`                  | Имя человека (максимум 20 символов)                                     |
| `day`           | `unsigned int` (5 бит)      | День рождения (0-31)                                                    |
| `month`         | `unsigned int` (4 бит)      | Месяц рождения (0-15, но используется 1-12)                             |
| `year`          | `unsigned int` (11 бит)     | Год рождения (0-2047)                                                   |
| `print_birthday`| `void(Birthday)`            | Функция для вывода даты рождения в формате "Имя: день.месяц.год"        |
| `John`          | `Birthday`                  | Экземпляр структуры `Birthday` с данными для "John"                      |
| `Ivan`          | `Birthday`                  | Экземпляр структуры `Birthday` с данными для "Ivan IV Grozniy"          |
| `Person`        | `Birthday`                  | Параметр функции `print_birthday`, принимающий структуру для вывода     |

### Код программы:

```c
#include <stdio.h>

typedef struct {
    char name[20];
    unsigned int day : 5; // 5 бит, чтобы хранить значения от 0 до 31 (так как 2^5 = 32)
    unsigned int month : 4; // аналогично, 2^4 = 16
    unsigned int year : 11; // 2^11 = 2048
} Birthday;

void print_birthday(Birthday Person)
{
        printf("%s`s birthday: %d.%d.%d\n", Person.name, Person.day, Person.month, Person.year);  
}

int main()
{
    Birthday John = {"John",23,2,2005};
    Birthday Ivan = {"Ivan IV Grozniy",25,8,1530};
    print_birthday(John);
    print_birthday(Ivan);  
    return 0;
}
```
### Результаты работы программы:

![[рез34.png]]

## Задание 1.5
### Постановка задачи: 
Реализовать в виде структур двунаправленный связный список и совершить отдельно его обход в прямом и обратном направлениях с распечаткой значений каждого элемента списка.

### Список идентификаторов:

| **Переменная**  | **Тип**                     | **Суть**                                                                 |
|-----------------|-----------------------------|--------------------------------------------------------------------------|
| `Node`          | `struct`                    | Структура узла двусвязного списка                                        |
| `data`          | `int`                       | Значение, хранящееся в узле                                              |
| `prev`          | `struct Node*`              | Указатель на предыдущий узел в списке                                    |
| `next`          | `struct Node*`              | Указатель на следующий узел в списке                                     |
| `createNode`    | `Node*(int)`                | Функция создания нового узла с заданным значением                        |
| `newNode`       | `Node*`                     | Временный указатель на создаваемый узел                                  |
| `value`         | `int`                       | Значение для инициализации нового узла                                   |
| `appendNode`    | `void(Node**, Node**, int)` | Функция добавления узла в конец списка                                   |
| `head`          | `Node*`                     | Указатель на первый узел списка (голову)                                 |
| `tail`          | `Node*`                     | Указатель на последний узел списка (хвост)                               |
| `printForward`  | `void(Node*)`               | Функция печати списка от головы к хвосту                                 |
| `current`       | `Node*`                     | Временный указатель для итерации по списку                               |
| `printBackward` | `void(Node*)`               | Функция печати списка от хвоста к голове                                 |
| `freeList`      | `void(Node*)`               | Функция освобождения памяти, занимаемой списком                          |
| `temp`          | `Node*`                     | Временный указатель для безопасного освобождения памяти                  |

### Код программы:

```c
#include <stdio.h>
#include <stdlib.h>
  

// структура узла списка
// struct Node определяется в самой структуре, чтобы внутри нее ссылаться на ее собственный тип
typedef struct Node{
    int data;          
    struct Node* prev;  
    struct Node* next;  
} Node;

// создание нового узла
Node* createNode(int value) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("Memoty allocation error!\n");
        abort();
    }
    newNode->data = value;
    newNode->prev = NULL;
    newNode->next = NULL;
    return newNode;
}

// добавление узла в конец списка
// функции подаются указатели на указатель для модификации оригинальных указателей head и tail
void appendNode(Node** head, Node** tail, int value) {
    Node* newNode = createNode(value);
    if (*head == NULL) {
        // список пустой, поэтому новый узел становится головой и хвостом
        *head = newNode;
        *tail = newNode;
    } else {
        // добавление в конец, так как список не пустой
        (*tail)->next = newNode;
        newNode->prev = *tail;
        *tail = newNode;
    }
}
  
// обход списка от головы к хвосту
void printForward(Node* head) {
    printf("List forward: ");
    Node* current = head;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next; // переход к следующему узлу
    }
    printf("\n");
}

// обход списка от хвоста к голове
void printBackward(Node* tail) {
    printf("List backward: ");
    Node* current = tail;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->prev;
    }
    printf("\n");
}

// освобождение памяти
void freeList(Node* head) {
    Node* current = head;
    while (current != NULL) {
        Node* temp = current;
        current = current->next;
        free(temp);
    }
}
 
int main() {
    Node* head = NULL;  
    Node* tail = NULL;  
  
    // добавление элементов в список
    appendNode(&head, &tail, 10);
    appendNode(&head, &tail, 20);
    appendNode(&head, &tail, 30);
    appendNode(&head, &tail, 40);

    // обход списка и вывод элементов
    printForward(head);  
    printBackward(tail);

    // освобождение памяти
    freeList(head);
  
    return 0;
}
```
### Результаты работы программы:

![[рез35.png]]

# Комплект 2. 

## Задание 2.1
### Постановка задачи: 
Напишите программу, которая использует указатель на некоторое объединение `union`. 
### Список идентификаторов:

| **Переменная** | **Тип**            | **Суть**                                                                 |
|----------------|--------------------|--------------------------------------------------------------------------|
| `Data`         | `union`            | Объединение, хранящее разные типы данных в одной области памяти         |
| `i`            | `int`              | Целочисленное значение в объединении                                    |
| `f`            | `float`            | Вещественное число в объединении                                        |
| `s`            | `char[20]`         | Строка (массив символов) в объединении                                  |
| `data`         | `union Data`       | Экземпляр объединения Data                                              |
| `ptr`          | `union Data*`      | Указатель на объединение Data                                           |

### Код программы:

```c
#include <stdio.h>
#include <string.h>

union Data {
    int i;      
    float f;  
    char s[20];
};
  
int main()
{
    union Data data;
    // указатель на объединение
    union Data *ptr = &data;
    ptr->i = 57;
    printf("Integer number: %d\n", ptr->i);
    ptr->f = 3.14;
    printf("Float number: %.2f\n", ptr->f);
    
    // перезапись памяти; число 1078523331 - это представление битов 3.14 в целочисленном типе
    printf("Integer after writing float: %d\n", ptr->i);
    strcpy(ptr->s, "Get things done!");
    printf("String: %s\n", ptr->s);
    printf("Union size: %zu bite\n", sizeof(union Data));
  
    return 0;
}
```
### Результаты работы программы:

![[рез321.png]]
## Задание 2.2
### Постановка задачи: 
Напишите программу, которая использует `union` для побайтовой распечатки типа `unsigned long`. 
### Список идентификаторов:

| **Переменная** | **Тип**               | **Суть**                                                                 |
|----------------|-----------------------|--------------------------------------------------------------------------|
| `Number`       | `union`               | Объединение для доступа к числу как к целому или как к массиву байтов   |
| `n`            | `unsigned long`       | Целочисленное значение (обычно 4 или 8 байт)                            |
| `bytes`        | `unsigned char[]`     | Массив байтов, размер равен sizeof(unsigned long)                        |
| `num`          | `union Number`        | Экземпляр объединения Number                                            |
| `i`            | `int`                 | Счетчик цикла для перебора байтов                                       |

### Код программы:

```c
#include <stdio.h>
#include <string.h>
  
union Number {
    unsigned long n;
    unsigned char bytes[sizeof(unsigned long)]; // массив байтов размера unsigned long
};  

int main()
{
    union Number num;
    num.n = 1234567890;
    for(int i = 0; i < sizeof(num.bytes)/sizeof(unsigned char); i++){
        printf("%02x ", num.bytes[i]);
    }
}
```
### Результаты работы программы:

![[рез322.png]]

## Задание 2.3
### Постановка задачи: 
Создайте перечислимый тип данных (`enum`) для семи дней недели и распечатайте на экране его значения, как целые числа.
### Список идентификаторов:

| **Переменная** | **Тип**       | **Суть**                          |
|----------------|---------------|-----------------------------------|
| `WeekDays`     | `enum`        | Перечисление дней недели          |
| `MONDAY`       | `int` (enum)  | Понедельник (значение 1)          |
| `TUESDAY`      | `int` (enum)  | Вторник (значение 2)              |
| `WEDNESDAY`    | `int` (enum)  | Среда (значение 3)                |
| `THURSDAY`     | `int` (enum)  | Четверг (значение 4)              |
| `FRIDAY`       | `int` (enum)  | Пятница (значение 5)              |
| `SATURDAY`     | `int` (enum)  | Суббота (значение 6)              |
| `SUNDAY`       | `int` (enum)  | Воскресенье (значение 7)          |
### Код программы:

```c
#include <stdio.h>
#include <string.h>

enum WeekDays{
    MONDAY = 1, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
};
  
int main(void)
{
    printf("Monday: %d\n", MONDAY);
    printf("Tuesday: %d\n", TUESDAY);
    printf("Wednesday: %d\n", WEDNESDAY);
    printf("Thursday: %d\n", THURSDAY);
    printf("Friday: %d\n", FRIDAY);
    printf("Saturday: %d\n", SATURDAY);
    printf("Sunday: %d", SUNDAY);
}
```
### Результаты работы программы:

![[рез323.png]]

## Задание 2.4
### Постановка задачи: 
Создайте так называемое размеченное объединение `union`, которое заключено в виде поля структуры `struct` вместе с ещё одним полем, которое является перечислением enum и служит индикатором того, что именно на текущий момент хранится в таком вложенном объединении. Создать и заполнить динамический массив таких структур с объединениями внутри, заполняя вспомогательное поле перечисления enum для сохранения информации о хранимом в каждом размеченном объединении типе данных. Реализовать распечатку данных массива таких структур в консоль.
### Список идентификаторов:

| **Переменная**    | **Тип**          | **Суть**                                                                 |
|-------------------|------------------|--------------------------------------------------------------------------|
| `DataType`        | `enum`           | Перечисление типов данных (целое, вещественное, строка)                 |
| `TYPE_INT`        | `int` (enum)     | Константа для целочисленного типа (значение 1)                          |
| `TYPE_FLOAT`      | `int` (enum)     | Константа для вещественного типа (значение 2)                           |
| `TYPE_STRING`     | `int` (enum)     | Константа для строкового типа (значение 3)                              |
| `DataUnion`       | `union`          | Объединение для хранения разных типов данных                            |
| `intValue`        | `int`            | Целочисленное значение в объединении                                    |
| `floatValue`      | `float`          | Вещественное значение в объединении                                     |
| `stringValue`     | `char[5]`        | Строковое значение (максимум 4 символа + '\0') в объединении           |
| `TaggedUnion`     | `struct`         | Структура с меткой типа и объединением данных                           |
| `type`            | `DataType`       | Поле типа данных в структуре TaggedUnion                                |
| `data`            | `DataUnion`      | Поле с данными в структуре TaggedUnion                                  |
| `n`               | `int`            | Размер массива                                                          |
| `t`               | `int`            | Временная переменная для хранения типа данных                           |
| `array`           | `TaggedUnion*`   | Указатель на массив структур TaggedUnion                                |
| `i`               | `int`            | Счетчик циклов                                                          |
### Код программы:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// перечисление для указания типа данных в объединении
typedef enum {
    TYPE_INT = 1,    
    TYPE_FLOAT,  
    TYPE_STRING  
} DataType;
  
// размеченное объединение
typedef union {
    int intValue;
    float floatValue;
    char stringValue[5];
} DataUnion;
  
// структура, содержащая метку типа и объединение
typedef struct {
    DataType type;    // метка типа данных
    DataUnion data;   // данные
} TaggedUnion;
  
int main()
{
    int n, t;
    printf("Enter array size: ");
    scanf("%d", &n);
    TaggedUnion* array = (TaggedUnion*)malloc(sizeof(TaggedUnion) * n);
    if (array == NULL)
    {
        printf("Memory allocation ERROR!");
        return 1;
    }
  
    for(int i = 0; i < n; i++)
    {
        printf("Enter data type for current (%d) element (1 - INT, 2 - FLOAT, 3 - STRING): ", i+1);
        scanf("%d", &t);
  
        switch(t)
        {
            case 1:
            (array+i)->type = TYPE_INT;
            printf("Enter an INTEGER number: ");
            scanf("%d", &(array+i)->data.intValue);
            break;
  
            case 2:
            (array+i)->type = TYPE_FLOAT;
            printf("Enter a FLOAT number: ");
            scanf("%f", &(array+i)->data.floatValue);
            break;
            case 3:
            (array+i)->type = TYPE_STRING;
            printf("Enter a STRING: ");
            scanf("%4s", (array+i)->data.stringValue);
            break;
  
            default:
            printf("Incorrect TYPE! Enter 1 for INT, 2 for FLOAT, 3 for STRING.\n");
            i--;
            break;
        }
    }
  
    printf("\nResulting ARRAY:\n");
    for (int i = 0; i < n; i++)
    {
        switch ((array+i)->type)
        {
        case 1:
            printf("#%d: %d, INTEGER type\n",i+1,(array+i)->data.intValue);
            break;
        case 2:
            printf("#%d: %.2f, FLOAT type\n",i+1,(array+i)->data.floatValue);
            break;
        case 3:
            printf("#%d: %s, STRING type\n",i+1,(array+i)->data.stringValue);
            break;
        }
    }
    free(array);

    return 0;
}
```
### Результаты работы программы:

![[рез324.png]]

