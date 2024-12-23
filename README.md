# ОТЧЁТ
## Постановка задачи - разбор алгебраического выражения при помощи стека
> [!NOTE]
> Множества
> 1. символов 'a'  - 'z', '0' - '9';
> 2. операций '+','-','*','/'  '(',')';

### РЕШЕНИЕ И АЛГОРИТМ
При проверке правильности выражения, мы не учитываем приоритет операций.
Мы помещаем выражение в стек, пока не встретим закрывающуюся скобку ")".
Далее мы проверяем корректность того, что находится в скобках, заменяя на новый символ из 'a' - 'z' или '0' - '9'.
По итогу, вы проверяем итоговое выражение, которое поулчилось псоле прохождения всего выражения и замен.

## СТЕК
> [!NOTE]
> Стек (от англ. stack — стопка) — структура данных, представляющая собой упорядоченный набор элементов, в которой добавление новых элементов и удаление существующих производится с одного конца, называемого вершиной стека.

```C
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <string.h>
```
При выполнении этого проекта я использовал библиотеки, представленные выше.


Эти определения структур нужны для реализации стека как связного списка.
```C
typedef struct node {
    char data; // Данные узла, в данном случае символ (char)
    struct node *next;  // Указатель на следующий узел в стеке
}
```

```C
typedef struct stack {
    struct node *top; // Указатель на вершину стека (последний добавленный элемент)
    int size; // Текущий размер стека (количество элементов)
}
```
Также для реализации данного проекта необходимы следующие функции:

> [!NOTE]
> 1. Push - положить в вершину
> 2. Pop - вытащить из верщины
> 3. ShowTop - показать вершину стека
> 4. PrintStack - вывод стека
> 5. IsEmpty - проверка на пустоту
> 6. DeleteStack - удаление стека полностью

Давайте рассмотрим каждую из них более подробно.

> [!NOTE]
> Функция Push необходима для добавления нового элемента в стек. Это одна из базовых операций для работы со стеком, реализующим принцип LIFO (Last In, First Out — последний вошёл, первый вышел).

1. Push - положить в вершину
```C
bool push(stek *st, char data)
{
    if (st)  // Проверяем, что указатель на стек не равен NULL
    {
        node *new_el = (node*) malloc(sizeof(node));  // Выделяем память для нового элемента стека
        if (new_el)  // Если память успешно выделена
        {
            new_el->data = data;  // Присваиваем данные новому элементу
            new_el->next = NULL;  // Новый элемент будет последним, поэтому его указатель на следующий элемент равен NULL

            if (st->top == NULL)  // Если стек пуст
            {
                st->top = new_el;  // Новый элемент становится верхним элементом стека
            }
            else  // Если стек не пуст
            {
                new_el->next = st->top;  // Новый элемент указывает на текущий верхний элемент стека
                st->top = new_el;  // Новый элемент становится верхним элементом стека
            }

            st->size++;  // Увеличиваем размер стека
            return true;  // Успешное добавление элемента
        }
    }
    return false;  // Если указатель на стек NULL или не удалось выделить память, возвращаем false
}
```

> [!NOTE]
> Функция Pop необходима для удаления верхнего элемента из стека и возврата его данных. Также является одной из ключевых функций для работы со стеком.

2. Pop - вытащить из верщины
```C
char pop(stek *st)
{
    if (st)  // Проверяем, что указатель на стек не равен NULL
    {
        if (st->top)  // Если стек не пуст (есть хотя бы один элемент)
        {
            char element = st->top->data;  // Сохраняем данные верхнего элемента стека

            node *ptrIx = st->top;  // Создаем временный указатель на текущий верхний элемент
            st->top = st->top->next;  // Перемещаем указатель на верхний элемент на следующий элемент стека
            st->size--;  // Уменьшаем размер стека

            free(ptrIx);  // Освобождаем память, занятую удаленным элементом стека
            return element;  // Возвращаем данные удаленного элемента
        }
    }
    return 0;  // Если стек пуст или указатель на стек равен NULL, возвращаем 0
}
```


> [!NOTE]
> Функция ShowTop используется для получения данных верхнего элемента стека, не удаляя его. Это значит, что она позволяет заглянуть в вершину стека и извлечь её данные, но не изменяет структуру стека.

3. ShowTop - показать вершину стека
```C
bool ShowTop(stek *st, char *data)
{
    bool full = false;  // Переменная для проверки, был ли найден верхний элемент

    if (st && data)  // Проверяем, что указатели на стек и данные не равны NULL
    {
        if (st->top)  // Если стек не пуст
        {
            *data = st->top->data;  // Сохраняем данные верхнего элемента в переменную, на которую указывает data
            full = true;  // Отмечаем, что верхний элемент найден
        }
    }

    return full;  // Возвращаем true, если данные верхнего элемента успешно получены, иначе false
}
```


> [!NOTE]
> Функция PrintStack нужна для вывода всех элементов стека на экран. Она позволяет визуализировать содержимое стека, что может быть полезно для отладки или для отображения данных в программе.

4. PrintStack - вывод стека
```C
void PrintStack(stek st)
{
    if (st.top == NULL)  // Если стек пуст (указатель на верхний элемент равен NULL)
    {
        printf("Stack is empty!\n");  // Выводим сообщение о пустом стеке
        return;  // Завершаем выполнение функции, так как нечего выводить
    }

    node *current = st.top;  // Устанавливаем указатель на первый элемент стека

    while (current)  // Пока в стеке есть элементы
    {
        printf("%c\n", current->data);  // Выводим данные текущего элемента стека
        current = current->next;  // Переходим к следующему элементу стека
    }
}
```


> [!NOTE]
> Функция IsEmpty предназначена для проверки, пуст ли стек. Это важная операция, которая позволяет безопасно работать со стеком, особенно перед выполнением операций, таких как удаление элементов, чтобы избежать ошибок, связанных с попыткой работы с пустой структурой данных.

5. IsEmpty - проверка на пустоту
```C
bool isEmpty(stek st)
{
    if (st.top == NULL)  // Если указатель на верхний элемент стека равен NULL (стек пуст)
        return true;  // Возвращаем true, указывая, что стек пуст

    return false;  // Если стек не пуст, возвращаем false
}
```


> [!NOTE]
> Функция DeleteStack предназначена для очистки стека, т.е. для удаления всех элементов стека и освобождения памяти, занятой этими элементами. После её выполнения стек становится пустым, а вся память, которая была выделена под элементы стека, освобождается.

6. DeleteStack - удаление стека полностью
```C
void DeleteStack(stek *st)
{
    if (st)  // Проверяем, что указатель на стек не равен NULL
    {
        if (st->top)  // Если стек не пуст (есть хотя бы один элемент)
        {
            node *ptr = st->top;  // Указатель на текущий элемент стека
            node *ptrIx = NULL;  // Временный указатель для освобождения памяти

            while (ptr)  // Пока в стеке есть элементы
            {
                ptrIx = ptr;  // Сохраняем текущий элемент в временную переменную
                ptr = ptr->next;  // Переходим к следующему элементу стека
                st->size--;  // Уменьшаем размер стека
                free(ptrIx);  // Освобождаем память, занятую текущим элементом
            }

            st->top = NULL;  // После очистки стека устанавливаем указатель на верхний элемент в NULL
        }
    }
}
```

## ГЛАВНАЯ ФУНКЦИЯ ПРОВЕРКИ ВЫРАЖЕНИЯ

> [!NOTE]
> 1. Проверка данных
> 2. Помещение символов в стек до символа ")"
> 3. Проверка выражения в скобках и замена на один элемент
> 4. Проверка выражения которое осталось по итогу (без скобок)
> 5. В конце проверка размера стека, если ноль то выражение верно, иначе не верно

 Функция CheckLexeme предназначена для проверки корректности последовательности символов в стеке, который представляет собой выражение, состоящее из операндов (например, цифр или букв) и операторов (например, +, -, *, /).

```C
bool CheckLexeme(stek *st)
{
    // Проверка, инициализирован ли стек (не равен NULL)
    if (st == NULL)
        return false;  // Если стек не инициализирован, возвращаем false

    int pos = 0;  // Переменная для отслеживания позиции элемента в последовательности

    // Пока стек не пуст
    while (isEmpty(*st) == false)
    {
        char tmp = pop(st);  // Извлекаем верхний элемент из стека

        // Если элемент является буквой (от a до z) или цифрой (от 0 до 9)
        if (('a' <= tmp) && (tmp <= 'z') || (('0' <= tmp) && (tmp <= '9')))
        {
            // Если позиция нечетная, это ошибка, так как по правилам лексемы
            // на нечетных позициях должны быть операторы
            if (pos % 2 != 0)
                return false;  // Возвращаем false, если лексема на нечетной позиции
        }

        // Если элемент является оператором (+, -, *, /)
        if ((tmp == '+') || (tmp == '-') || (tmp == '*') || (tmp == '/'))
        {
            // Если позиция четная, это ошибка, так как по правилам лексемы
            // на четных позициях должны быть операнды
            if (pos % 2 == 0)
                return false;  // Возвращаем false, если оператор на четной позиции
        }

        pos++;  // Увеличиваем позицию на 1 для следующего элемента
    }

    return true;  // Если последовательность символов в стеке корректна, возвращаем true
}
```
Итоговая функция, выполняющая проверку всего выражения

```C
bool check_expression(char *str)
{
    // Проверка, что указатель на строку не равен NULL
    if (str == NULL)
        return false;  // Если строка не инициализирована, возвращаем false

    // Проверка, что строка не пустая
    if (strlen(str) == 0)
        return false;  // Если строка пуста, возвращаем false

    // Проверка, что первый символ не является оператором
    if ((str[0] == '+') || (str[0] == '-') || (str[0] == '*') || (str[0] == '/'))
        return false;  // Если первый символ — оператор, возвращаем false

    // Инициализация стека для анализа выражения
    stek st = {NULL, 0};
    int i = 0;

    // Основной цикл для прохода по каждому символу строки
    while (str[i])
    {
        // Проверка на пустые скобки ()
        if (str[i] == '(')
            if (str[i + 1] && str[i + 1] == ')')
                return false;  // Если скобки пустые, возвращаем false

        // Проверка на закрывающую скобку
        if (str[i] == ')')
        {
            // Если после закрывающей скобки идет открывающая, это ошибка
            if (str[i + 1] == '(')
                return false;  // Неправильное расположение скобок


            char tmp;
            int pos = 0;
            bool flag = true;

            // Цикл для обработки элементов до открывающей скобки
            while (flag)
            {
                // Если стек пуст, то выражение некорректно (закрывающая скобка без соответствующей открывающей)
                if (isEmpty(st))
                    return false;

                tmp = pop(&st);  // Извлекаем элемент из стека
                pos++;  // Увеличиваем позицию

                char tmp_2; 
                ShowTop(&st, &tmp_2);  // Получаем верхний элемент стека

                // Если нашли открывающую скобку, завершаем цикл
                if (tmp_2 == '(')
                    flag = false;

                // Проверка, что лексема на четной позиции — операнд (буква или цифра)
                if (('a' <= tmp) && (tmp <= 'z') || (('0' <= tmp) && (tmp <= '9')))
                    if (pos % 2 == 0)  // Нечетная позиция для операнда недопустима
                        return false;

                // Проверка, что лексема на нечетной позиции — оператор
                if ((tmp == '+') || (tmp == '-') || (tmp == '*') || (tmp == '/'))
                    if (pos % 2 == 1)  // Четная позиция для оператора недопустима
                        return false;
            }

            pop(&st);  // Убираем открывающую скобку из стека
            push(&st, 'L');  // Добавляем временный элемент (для обозначения конца скобочного выражения)

        }

        // Пропуск пробелов
        else if (str[i] != ' ')
        {
            // Проверка на операнды после открывающей скобки
            if (i > 0 && str[i - 1] == '(' && (str[i] == '+' || str[i] == '-') && str[i + 2] && str[i + 2] == ')')
                push(&st, '0');  // Добавляем '0' для обозначения отсутствующего операнда

            push(&st, str[i]);  // Добавляем текущий символ в стек

        }    
        i++;  // Переход к следующему символу
    }

    // Проверка итоговой лексемы
    if (CheckLexeme(&st) == false)
        return false;

    // Если после обработки всех символов в стеке нет элементов, выражение корректно
    return st.size == 0;
}
```
## ТЕСТИРОВКА ПРОГРАММЫ НА РАЗЛИЧНЫХ ВЫРАЖЕНИЯХ
 
Я рассмотрел всевозможные случаи выражений, которые могут встретиться, они могут быть как коректны, так и нет
```C
    //char str[size_str] = "";//uncorrect
    //char str[size_str] = "()";//uncorrect
    //char str[size_str] = "(+8)";// correct
    //char str[size_str] = "(+f)";// correct
    //char str[size_str] = "(+k)";// correct
    //char str[size_str] = "(-d)";//correct
    //char str[size_str] = "(-8)";//correc
    //char str[size_str] = " (     +8)";// correct
    //char str[size_str] = "(-1) * ()";//uuncorrect
    //char str[size_str] = "(-1)(-2)";//uncorrect
    //char str[size_str] = "1* (-5)*(-5)";//correct
    //char str[size_str] = "(2 + (8 * a))";//correct
    //char str[size_str] = "(2 (+ (8 )* a))";//uncorrect
    //char str[size_str] = "(2) - (2)/(4(*3) ))";//uncorrect
    //char str[size_str] = "  (+2 - ((-2) / h ) + (a /  (-2)))";//correct
    //char str[size_str] = "((c-d)*h+1)*(a+b)";//correct
    //char str[size_str] = "b * a * 8 + 1 + a * ";//uncorrect
    //char str[size_str] = "b * a ++ a";//uncorrect
    //char str[size_str] = "-2 + 1";//uncorrect
    //char str[size_str] = "2 + (1)";//correct
    //char str[size_str] = "(2 - 1 / (8/2))";//correct
    //char str[size_str] = "(((-2)-2)-2)";//correct
    //char str[size_str] = "((c + d) * e + 1) * (a + b))))";//uncorrect
    //char str[size_str] = "(a+(b**c) - 8)";//uncorrect
```

Выше приведены различные ситуации, которые могут быть найдены и проверены при помощи нашего проекта
