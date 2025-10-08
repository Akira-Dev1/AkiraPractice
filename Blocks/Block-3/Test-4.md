# Задача 4: Общие слова в последовательностях.
## Условие задачи:
Даны две последовательности слов. Слова состоят из строчных латинских символов и разделены пробелами. Одно и тоже слово может встретиться в последовательности много раз.

Определите и выведите на экран слова, которые есть одновременно в 2х последовательностях. Слова должны быть отсортированы лексикографически. Если таких слов нет, выведите -1.
### Формат ввода
В первой строке входных данных указано целое число 1 ≤ n ≤ 10000. В следующей строке, через пробел, перечислены слова. В третьей строке входных данных указано целое число 1 ≤ m ≤ 10000. В следующей строке, через пробел, перечислены слова.

Слова состоят из 5ти строчных латинских символов [a-z].
### Формат вывода
Слова отсортированные в лексикографическом порядке и разделённые пробелом или -1, если общих слов нет.
## Решение:
```cpp
#include <iostream>
#include <vector>
#include <sstream>
#include <string>
#include <set>
#include <algorithm>
using namespace std;

int main() {
    int n1, n2;
    string str1, str2, word;
    vector<string> words1, words2;

    cin >> n1; cin.ignore();
    getline(cin, str1);
    cin >> n2; cin.ignore();
    getline(cin, str2);

    istringstream ss1(str1), ss2(str2);
    set<string> s1, s2;
    vector<string> result;

    while (ss1 >> word) s1.insert(word);
    while (ss2 >> word) s2.insert(word);

    for (string w : s1) {
        if (s2.count(w)) {
            result.push_back(w);
        }
    }
    if (result.empty()) {
        cout << -1;
    } else {
        sort(result.begin(), result.end());
        for (int i = 0; i < result.size(); i++)
            cout << (i ? " " : "") << result[i];
    }
}
```
## Пояснение решения:
Библеотеки:
 - \<iostream\> — для ввода и вывода (cin, cout).
 - \<vector\> — хранение результата с использованием динамических массивов (vector<string>).
 - \<string\> — работа со строками.
 - \<sstream\> – разбор строки на слова (istringstream).
 - \<set\> – хранение уникальных слов и быстрый поиск (set<string>).
 - \<algorithm\> – сортировка слов (sort).

1. Считывание данных:
```cpp
cin >> n1; cin.ignore();
getline(cin, str1);
cin >> n2; cin.ignore();
getline(cin, str2);
```
Пояснение:
 - Считываем количество слов n1 и n2.
 - cin.ignore() убирает оставшийся \n.
 - getline считывает строки слов целиком.
2. Разделение строк на слова и удаление дубликатов:
```cpp
istringstream ss1(str1), ss2(str2);
set<string> s1, s2;

while (ss1 >> word) s1.insert(word);
while (ss2 >> word) s2.insert(word);
```
Пояснение:
 - Потоки ss1 и ss2 разбивают строки на слова.
 - Множества s1 и s2 хранят уникальные слова.
3. Поиск общих слов:
```cpp
for (string w : s1) {
    if (s2.count(w)) {
        result.push_back(w);
    }
}
```
Пояснение:
 - Проходим по каждому слову из первого множества s1.
 - s2.count(w) проверяет, есть ли слово в s2.
 - Если есть → добавляем его в result.
4.  Вывод результата:
```cpp
if (result.empty()) {
    cout << -1;
} else {
    sort(result.begin(), result.end());
    for (int i = 0; i < result.size(); i++)
        cout << (i ? " " : "") << result[i];
}
```
Пояснение:
 - Если общих слов нет → выводим -1.
 - Иначе сортируем лексикографически и выводим через пробел.