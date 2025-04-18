# Лабораторная работа 8: Шаблоны

## Теоретическая часть

Шаблоны функций и классов в C++ позволяют писать обобщенный код для работы с разными типами данных.

Шаблон в C++ определяется с помощью ключевого слова `template`,
за которым идут угловые скобки `<>` в которых указываются параметры шаблона.

### Пример шаблонной функции
```cpp
template<typename T>
T add(T a, T b)
{
    return a + b;
}

int main(int argc, char* argv[])
{
    int x = add(5, 4);
    double y = add(5.3, 8.2);
}
```

### Пример шаблонного класса
```cpp
template<typename T>
class Awesome {
public:
    Awesome(T a, T b)
        : a_(a)
        , b_(b)
    {
    }

private:
    T a_{};
    T b_{};
};

int main(int argc, char* argv[])
{
    Awesome<int> x(12, 17);
    Awesome<double> y(4.3, 19.7);
}
```

## Задания

Разработайте шаблонную функцию по варианту.

1) **`avg`** -- вычисляет среднее значение вектора данных;
2) **`mult`** -- умножает каждый элемент вектора данных на значение;
3) **`invert`** -- инвертирует каждый элемент вектора данных;
4) **`add`** -- увеличивает каждый элемент вектора данных на значение;

Убедитесь что ваша функция работает с `int`, `float`, `std::complex`;

Класс из ЛР7 сделайте шаблонным, так чтобы тип аттрибутов класса задавался парметром;

Убедитесь что ваша шаблонная функция работает с новым шаблонным классом и при необходимовсти модифицируте класс;

Разработайте шаблонную функцию `print_vect`, которая выводит в консоль содержимое векторва (std::vector<>).
