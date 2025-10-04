# Задача 3: Функция. 2048.
## Условие задачи:
Дан код:

```cpp
#include <iostream>
#include <vector>

// Ваш код будет вставлен сюда

void read(std::vector<std::vector<int>>& board){
    for (auto& row: board)
        for (auto& cell: row)
            std::cin >> cell;
}

void print(const std::vector<std::vector<int>>& board){
    for (auto& row: board){
        for (auto& cell: row) std::cout << cell << ' ';
        std::cout << std::endl;
    }
}

int main() {
    char direction;
    std::cin >> direction;
    
    std::vector<std::vector<int>> board(4, std::vector<int>(4));
    read(board);
    move(board, direction);
    print(board);
}
```
Этот код будет прогоняться по тестам. Ваша задача реализовать функцию move таким образом, чтобы этот код стал рабочим.

Аргумент board - игровое поле размером 4 на 4 клетки. Каждая клетка содержит число от 0 до 2^30, кратное 2. Число 0 означает, что клетка пустая;

Аргумент direction - направление сдвига чисел. Одно из 4-х значений: L - влево, R - вправо, T - вверх, B - вниз. Числа сдвигаются по правилам игры [2048](https://2048game.com/ru/).

В результате работы программы, в терминале должно быть выведено состояние доски после сдвига.
### Формат ввода
В первой строке входных данных указан один из символов: [L, R, T, B]. В последующих 4-х строках указаны по 4 целых числа aᵢ разделённые пробелом. Все числа кратные 2 и находятся в диапазоне aᵢ ∈ \[0..2³⁰].
### Формат вывода
Значения в ячейках доски должны быть выведены в виде 4-x строк, по 4 числа в каждой. В строке, числа должны быть разделены пробелом.
## Решение:
```cpp
#include <iostream>
#include <vector>
using namespace std;
void move(vector<vector<int>>& board, char dir) {
    if (dir == 'L') {
        for (int row = 0; row < 4; row++) {
            int write = 0;
            for (int col = 0; col < 4; col++) {
                if (board[row][col] != 0) {
                    if (write != col) {
                        board[row][write] = board[row][col];
                        board[row][col] = 0;
                    }
                    write++;
                }
            }
            for (int col = 0; col < 3; col++) {
                if (board[row][col] == board[row][col+1] && board[row][col] != 0) {
                    board[row][col] *= 2;
                    board[row][col+1] = 0;
                }
            }
            write = 0;
            for (int col = 0; col < 4; col++) {
                if (board[row][col] != 0) {
                    if (write != col) {
                        board[row][write] = board[row][col];
                        board[row][col] = 0;
                    }
                    write++;
                }
            }
        }
    } else if (dir == 'R') {
        for (int row = 0; row < 4; row++) {
            int write = 3;
            for (int col = 3; col >= 0; col--) {
                if (board[row][col] != 0) {
                    if (write != col) {
                        board[row][write] = board[row][col];
                        board[row][col] = 0;
                    }
                    write--;
                }
            }
            for (int col = 3; col > 0; col--) {
                if (board[row][col] == board[row][col-1] && board[row][col] != 0) {
                    board[row][col] *= 2;
                    board[row][col-1] = 0;
                }
            }
            write = 3;
            for (int col = 3; col >= 0; col--) {
                if (board[row][col] != 0) {
                    if (write != col) {
                        board[row][write] = board[row][col];
                        board[row][col] = 0;
                    }
                    write--;
                }
            }
        }
    } else if (dir == 'T') {
        for (int col = 0; col < 4; col++) {
            int write = 0;
            for (int row = 0; row < 4; row++) {
                if (board[row][col] != 0) {
                    if (write != row) {
                        board[write][col] = board[row][col];
                        board[row][col] = 0;
                    }
                    write++;
                }
            }
            for (int row = 0; row < 3; row++) {
                if (board[row][col] == board[row+1][col] && board[row][col] != 0) {
                    board[row][col] *= 2;
                    board[row+1][col] = 0;
                }
            }
            write = 0;
            for (int row = 0; row < 4; row++) {
                if (board[row][col] != 0) {
                    if (write != row) {
                        board[write][col] = board[row][col];
                        board[row][col] = 0;
                    }
                    write++;
                }
            }
        }
    } else if (dir == 'B') {
        for (int col = 0; col < 4; col++) {
            int write = 3;
            for (int row = 3; row >= 0; row--) {
                if (board[row][col] != 0) {
                    if (write != row) {
                        board[write][col] = board[row][col];
                        board[row][col] = 0;
                    }
                    write--;
                }
            }
            for (int row = 3; row > 0; row--) {
                if (board[row][col] == board[row-1][col] && board[row][col] != 0) {
                    board[row][col] *= 2;
                    board[row-1][col] = 0;
                }
            }
            write = 3;
            for (int row = 3; row >= 0; row--) {
                if (board[row][col] != 0) {
                    if (write != row) {
                        board[write][col] = board[row][col];
                        board[row][col] = 0;
                    }
                    write--;
                }
            }
        }
    }
}
```
## Пояснение решения:
Библеотеки:
 - \<iostream\> - Нужно для ввода/вывода.
 - \<vector\> — для хранения игрового поля в виде двумерного массива vector<vector\<int>>.
1. Определяем направление движения:
```cpp
if (dir == 'L') { 
    // движение влево
} else if (dir == 'R') {
    // движение вправо
} else if (dir == 'T') {
    // движение вверх
} else if (dir == 'B') {
    // движение вниз
}
```
Пояснение:
Пояснение: 
 - Здесь выбирается блок кода, который будет выполняться в зависимости от направления движения плиток. Это позволяет одной функцией обрабатывать все четыре направления.
 - Каждое направление работает по одной и той же логике, но меняются индексы: строки или столбцы, порядок обхода и начальная позиция write.
2. Сдвиг плиток:
Пример на направлении 'L':
```cpp
int write = 0; // или 3 в зависимости от направления
for (int col = 0; col < 4; col++) { // или row
    if (board[row][col] != 0) {
        if (write != col) {
            board[row][write] = board[row][col];
            board[row][col] = 0;
        }
        write++;
    }
}
```
Пояснение:
Для каждой строки (если движение горизонтальное) или столбца (если вертикальное) выполняется цикл по клеткам.
 - Создаётся переменная write, которая указывает на позицию, куда должна попасть следующая непустая плитка.
 - Если текущая клетка не равна нулю, её значение перемещается на позицию write, а старая клетка обнуляется, если это не та же самая позиция.
 - После каждой такой операции write смещается: увеличивается при движении влево/вверх и уменьшается при движении вправо/вниз.

Смысл: все непустые плитки собираются в сторону движения, промежуточные нули остаются с другой стороны.
3. Объединение плиток:
```cpp
for (int col = 0; col < 3; col++) { // или row
    if (board[row][col] == board[row][col+1] && board[row][col] != 0) {
        board[row][col] *= 2;
        board[row][col+1] = 0;
    }
}
```
Пояснение:
После первого сдвига выполняется второй цикл, который проверяет соседние плитки вдоль направления движения:
 - Если две соседние плитки равны и не равны нулю, они объединяются в одну (умножаются на 2), а соседняя клетка обнуляется.
4. Повторный сдвиг после объединения:
```cpp
write = 0; // или 3 в зависимости от направления
for (int col = 0; col < 4; col++) { // или row
    if (board[row][col] != 0) {
        if (write != col) {
            board[row][write] = board[row][col];
            board[row][col] = 0;
        }
        write++;
    }
}
```
Пояснение:
 - После объединения плиток остаются промежуточные нули.
 - Второй сдвиг собирает все непустые плитки снова к стороне движения, чтобы поле было «сжато».