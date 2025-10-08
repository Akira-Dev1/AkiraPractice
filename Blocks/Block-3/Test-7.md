# Задача 7: Какое следующее слово.
## Условие задачи:
Дан файл data.txt с текстом. Текст состоит из слов разделенных пробелами. Слова состоят из строчных латинских символов. Текст заканчивается словом stopword. Посмотреть содержимое файла можно [тут](https://disk.yandex.ru/d/yfsccdCTBDNRvw).

Реализуйте алгоритм, который по введённому слову target предскажет следующее слово. А именно, выведет в терминал три возможных варианта следующего за target слова (без повторов) по следующим правилам:

Если есть слово X, которое, в тексте, встречалось после слова target чаще остальных, то выводим его.
Если слова X и Y встречались одинаковое количество раз после слова target, то выводим то, которое лексикографически меньше. Например, для hi и by, нужно вывести by.
Если все различные слова, которые встречались после target уже выведены (даже если их меньше 3х), то закончить вывод;
Если слова target в тексте нет, выведите один символ минус -.
Например:

Текст: текст состоит из слов разделенных пробелами слова состоят из строчных латинских символов stopword

Таргет: слов
Вывод: разделенных 

Таргет: из
Вывод: слов строчных

Таргет: бэтмен
Вывод: -
#### Как получить данные из файла

```cpp
#include "fstream"

int main() {
    std::ifstream file("data.txt"); // Открываем файл на чтение
    
    // Работаем с файлом так же как с cin
    // std::string word;
    // file >> word;
}
```
### Формат ввода
Файл data.txt, расположен в том же каталоге, что и программа. На вход программе подаётся одно слово target состоящее из строчных латинских символов.
### Формат вывода
Символ минус - или до 3х слов разделённых пробелами.
## Решение:
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <fstream>
#include <utility>
#include <algorithm>

using namespace std;
int main() {
    ifstream file("data.txt");
    vector<string> words;
    string word;
    while (file >> word) {
        words.push_back(word);
    }
    string name;
    cin >> name;
    map<string, int> cheked;
    for (int i = 0; i < words.size()-1; i++) {
        if (words[i] == name) {
            cheked[words[i+1]] += 1;
        }
    }
    vector<pair<string, int>> result(cheked.begin(), cheked.end());
    sort(result.begin(), result.end(), [](auto &a, auto &b) {
        if (a.second != b.second) {
            return a.second > b.second;
        } else {
            return a.first < b.first;
        }
    });
    if (result.size() >= 1) {
        for (int i = 0; i < 3 && i < result.size(); i++) {
            if (i != 0) {
                cout << " ";
            }
            cout << result[i].first;
        }
    } else {
        cout << "-";
    }

}
```
## Пояснение решения:
Библеотеки:
 - \<iostream\> — для ввода и вывода (cin, cout).
 - \<vector\> — динамический массив (vector<string> для слов).
 - \<string\> — работа со строками.
 - \<map\> — для подсчёта, сколько раз каждое слово встречается после target (map<string,int>).
 - \<fstream\> — работа с файлами (ifstream).
 - \<utility\> — пара pair, используется для преобразования map в vector<pair<>>.
 - \<algorithm\> — сортировка sort с пользовательским компаратором.

1. Чтение файла в вектор слов:
```cpp
ifstream file("data.txt");
vector<string> words;
string word;
while (file >> word) {
    words.push_back(word);
}
```
Пояснение:
 - Открываем файл data.txt на чтение.
 - Проходим по всем словам файла.
 - Каждое слово добавляем в words, чтобы дальше можно было анализировать соседние слова.
2. Чтение target слова:
```cpp
string name;
cin >> name;
```
Пояснение:
 - Считываем слово, после которого нужно предсказать следующие слова.
3. Подсчёт слов, следующих за target:
```cpp
map<string, int> cheked;
for (int i = 0; i < words.size()-1; i++) {
    if (words[i] == name) {
        cheked[words[i+1]] += 1;
    }
}
```
Пояснение:
 - Проходим по всем словам кроме последнего.
 - Если текущее слово равно target (name), то увеличиваем счётчик для следующего слова (words\[i+1]) в map.
 - В итоге cheked\[word] хранит количество раз, которое слово встречалось после target.

4. Преобразование map в вектор и сортировка:
```cpp
vector<pair<string, int>> result(cheked.begin(), cheked.end());
sort(result.begin(), result.end(), [](auto &a, auto &b) {
    if (a.second != b.second) {
        return a.second > b.second;
    } else {
        return a.first < b.first;
    }
});
```
Пояснение:
 - map нельзя сортировать напрямую по значениям, поэтому преобразуем в vector<pair<>>.
 - Сортировка:
     - Сначала по количеству встреч (частоте) по убыванию (a.second > b.second).
     - Если частоты равны, по лексикографическому порядку (a.first < b.first).
5. Вывод до трёх слов:
```cpp
if (result.size() >= 1) {
    for (int i = 0; i < 3 && i < result.size(); i++) {
        if (i != 0) {
            cout << " ";
        }
        cout << result[i].first;
    }
} else {
    cout << "-";
}
```
Пояснение:
 - Если хотя бы одно слово после target встречалось — выводим максимум три слова через пробел.
 - Если таких слов нет (target в тексте не встречается) — выводим "-".