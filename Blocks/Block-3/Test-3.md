# Задача 1: Объединить отсортированные последовательности.
## Условие задачи:
Дано арифметическое выражение, которое может состоять из:
 - бинарных, левоассоциативных операторов: +, -, *, /, %
 - бинарного, правоассоциативного оператора: возведение в степень (^)
 - неотрицательных операндов целого типа
 - скобочек: ( и ).
Преобразуйте это выражение в [обратную польскую нотацию](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%80%D0%B0%D1%82%D0%BD%D0%B0%D1%8F_%D0%BF%D0%BE%D0%BB%D1%8C%D1%81%D0%BA%D0%B0%D1%8F_%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C#%D0%9F%D1%80%D0%B5%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%B8%D0%B7_%D0%B8%D0%BD%D1%84%D0%B8%D0%BA%D1%81%D0%BD%D0%BE%D0%B9_%D0%BD%D0%BE%D1%82%D0%B0%D1%86%D0%B8%D0%B8).
### Формат ввода
В одной строке задано арифметическое выражение (без пробелов). В конце строки символ \n.
### Формат вывода
Выражение преобразованное в обратную польскую нотацию. Между элементами должно быть по одному пробелу.
## Решение:
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int priority(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/' || op == '%') return 2;
    if (op == '^') return 3;
    return 0;
}

int main() {
    string str;
    getline(cin, str);
    vector<char> result, stack;
    for (int x = 0; x < str.size(); x++) {
        if (str[x] >= '0' && str[x] <= '9') {
            if (x > 0 && str[x-1] >= '0' && str[x-1] <= '9') {
                result.push_back('|');
            }
            result.push_back(str[x]);
        } else {
            if (stack.empty()) {
                stack.push_back(str[x]);
            } else {
                if (str[x] == '(') {
                    stack.push_back(str[x]);
                } else if (str[x] == ')') {
                    while (!stack.empty() && stack.back() != '(') {
                        result.push_back(stack.back());
                        stack.pop_back();
                    }
                    if (!stack.empty()) stack.pop_back();
                } else {
                    while (!stack.empty() && priority(stack.back()) >= priority(str[x]) && stack.back() != '(') {
                        if (str[x] == '^' && stack.back() == '^') {
                            break;
                        }
                        result.push_back(stack.back());
                        stack.pop_back();
                    }
                    stack.push_back(str[x]);
                }
            }
        }
    }
    while (!stack.empty()) {
        if (stack.back() != '(')
            result.push_back(stack.back());
        stack.pop_back();
    }
    int f = 0;
    for (int i = 0; i < result.size(); i++) {
        if (result[i] == '|') {
            continue;
        } else {
            if (i > 0 && result[i-1] != '|') {
                cout << " ";
            }
        }
        cout << result[i];
    }
}
```
## Пояснение решения:
Библеотеки:
 - \<iostream\> — для ввода и вывода (cin, cout).
 - \<vector\> — чтобы использовать динамические массивы (vector<int>).
 - \<algorithm\> — содержит функцию sort, которая нужна для сортировки.

1. Функция priority(char op):
```cpp
int priority(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/' || op == '%') return 2;
    if (op == '^') return 3;
    return 0;
}
```
Пояснение:
 - Возвращает приоритет оператора, чтобы знать, какие операции выполняются раньше. Это нужно, чтобы при преобразовании в обратную польскую нотацию (ОПН) правильно расставить порядок вычислений.
2. Считывание выражения и основные структуры:
```cpp
getline(cin, str);
vector<char> result, stack;
```
Пояснение:
 - Считывает строку с выражением (например: 3+4*2/(1-5)^2^3).
 - result — сюда записываем результат в ОПН.
 - stack — стек операторов (и скобок).
3. Основной цикл по символам:
```cpp
for (int x = 0; x < str.size(); x++) {
    if (str[x] >= '0' && str[x] <= '9') {
        ...
    } else {
        ...
    }
}
```
Пояснение:
 - Проходим по каждому символу выражения.
4.  Обработка чисел:
```cpp
if (str[x] >= '0' && str[x] <= '9') {
    if (x > 0 && str[x-1] >= '0' && str[x-1] <= '9') {
        result.push_back('|');
    }
    result.push_back(str[x]);
}
```
Пояснение:
 - Если число из 1 символа, то мы просто его добавляем в результат
 - Если число из нескольких символов то между символами мы ставим "|" для корректной обработки строки.
5. Обработка операторов и скобок:
```cpp
if (stack.empty()) {
    stack.push_back(str[x]);
} else {
    if (str[x] == '(') {
        stack.push_back(str[x]);
    } else if (str[x] == ')') {
        while (!stack.empty() && stack.back() != '(') {
            result.push_back(stack.back());
            stack.pop_back();
        }
        if (!stack.empty()) stack.pop_back();
    } else {
        while (!stack.empty() && priority(stack.back()) >= priority(str[x]) && stack.back() != '(') {
            if (str[x] == '^' && stack.back() == '^') {
                break;
            }
            result.push_back(stack.back());
            stack.pop_back();
        }
        stack.push_back(str[x]);
    }
}
```
Пояснение:
 - Если стек пуст — просто кладём оператор туда.

 - Если встретили '(' — кладём её в стек.

 - Если встретили ')' — достаём из стека всё до '(', добавляя в результат.

 - Если обычный оператор:
     - Смотрим, есть ли в стеке операторы с выше или равным приоритетом — если да, вытаскиваем их в result.
     - Особый случай: ^ (правоассоциативный), поэтому если оба ^, не выталкиваем.
     - После этого кладём текущий оператор в стек.
6. После цикла:
```cpp
while (!stack.empty()) {
    if (stack.back() != '(')
        result.push_back(stack.back());
    stack.pop_back();
}
```
Пояснение:
 - После прохода строки могут остаться операторы в стеке — переносим их в результат.
7. Вывод результата:
```cpp
int f = 0;
for (int i = 0; i < result.size(); i++) {
    if (result[i] == '|') {
        continue;
    } else {
        if (i > 0 && result[i-1] != '|') {
            cout << " ";
        }
    }
    cout << result[i];
}
```
Пояснение:
 - Пропускаем | (они были только разделителями внутри чисел).
 - Между элементами ставим один пробел.