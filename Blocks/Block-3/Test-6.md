# Задача 6: Баланс пользователя.
## Условие задачи:
Дана информация о пользователях:
 - login (логин) - строка состоящая из строчных латинских символов и цифр;
 - balance (сумма на счету) - целое число 10⁻⁹ ≤ balance ≤ 10⁹;
Напишите программу, которая по введённому логину сообщает сколько денег осталось на счету у пользователя.
### Формат ввода
В первой строке входных данных указано целое число 1 ≤ n ≤ 20000 - количество пользователей. В следующих n строках перечислены логины пользователей и состояние счёта, именно в таком порядке, через точку с запятой.
В третьей строке входных данных указано целое число 1 ≤ m ≤ 20000 - количество запросов о состоянии счёта пользователя. На следующей строке перечислены логины пользователей через пробел.
### Формат вывода
Остаток на счету у пользователей в порядке, в котором перечислены логины, через пробел.
## Решение:
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>

using namespace std;

int main() {
    int countUsers, countRequests;
    map<string, int> users;
    vector<string> requests, strAll;
    cin >> countUsers;
    while (countUsers > 0) {
        string s;
        cin >> s;
        strAll.push_back(s);
        countUsers--;
    }
    for (string str : strAll) {
        vector<char> numsA;
        string num;
        int x = str.size()-1;
        while (str[x] != ';') {
            numsA.push_back(str[x]);
            str.pop_back();
            x--;
        }
        str.pop_back();
        for (int y = numsA.size()-1; y >= 0; y--) {
            num.push_back(numsA[y]);
        }
        users[str] = stoi(num);
    }
    cin >> countRequests;
    for (int i = 0; i < countRequests; i++) {
        string req;
        cin >> req;
        requests.push_back(req);
    }
    for (string request : requests) {
        cout << users[request] << " ";
    }
    
}
```
## Пояснение решения:
Библеотеки:
 - \<iostream\> — для ввода и вывода (cin, cout).
 - \<vector\> — хранение результата с использованием динамических массивов (vector<string>).
 - \<string\> — работа со строками.
 - \<map\> — для хранения данных в виде “ключ → значение”, чтобы быстро находить баланс пользователя по логину.;

1. Объявления и ввод количества пользователей:
```cpp
int countUsers, countRequests;
map<string, int> users;
vector<string> requests, strAll;
cin >> countUsers;
```
Пояснение:
 - countUsers — количество пользователей (число строк с логином и балансом).
 - users — ассоциативный массив (ключ — логин, значение — баланс).
 - strAll — временный вектор для хранения всех строк "login;balance".
 - cin >> countUsers; — ввод количества пользователей.
2. Считывание строк с пользователями:
```cpp
while (countUsers > 0) {
    string s;
    cin >> s;
    strAll.push_back(s);
    countUsers--;
}
```
Пояснение:
 - В цикле считываются строки вида login;balance (например, alex;1200)
 - Каждая строка добавляется в strAll.
 - Цикл повторяется n раз.
3. Разделение строки на логин и баланс:
```cpp
for (string str : strAll) {
    vector<char> numsA;
    string num;
    int x = str.size()-1;
    while (str[x] != ';') {
        numsA.push_back(str[x]);
        str.pop_back();
        x--;
    }
    str.pop_back();
```
Пояснение:
 - x = str.size() - 1 — начинаем с последнего символа строки.
 - Цикл while (str[x] != ';') идёт с конца строки, пока не найдёт ";".
 - Каждый символ числа ('0', '0', '2', '1') записывается в numsA.
 - str.pop_back() — убирает эти символы из конца строки, чтобы оставить только логин.
 - После выхода из цикла str.pop_back() убирает сам символ ';'.
 - Теперь:
     - str содержит логин
     - numsA содержит символы числа в обратном порядке
4. Восстановление числа в правильном порядке:
```cpp
for (int y = numsA.size()-1; y >= 0; y--) {
    num.push_back(numsA[y]);
}
users[str] = stoi(num);
```
Пояснение:
 - Переворачиваем numsA, чтобы получить число в обычном порядке.
 - num теперь строка вида "1200".
 - stoi(num) преобразует строку в целое число.
 - users[str] = stoi(num); — добавляем в карту:
5. Ввод запросов:
```cpp
cin >> countRequests;
for (int i = 0; i < countRequests; i++) {
    string req;
    cin >> req;
    requests.push_back(req);
}
```
Пояснение:
 - countRequests — сколько логинов нужно проверить.
 - В цикле считываются логины и сохраняются в requests.
6. Вывод ответов:
```cpp
for (string request : requests) {
    cout << users[request] << " ";
}
```
Пояснение:
 - Для каждого логина из запросов программа находит баланс в map users.
 - users[request] возвращает число (баланс).
 - Балансы выводятся через пробел в том же порядке, в котором были запросы.
