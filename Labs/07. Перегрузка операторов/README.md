# Лабораторная работа 7: Перегрузка операторов

## Теоретическая часть

Перегрузка операторов в C++ позволяет переопределить стандартное поведение операторов для пользовательских типов данных. Это улучшает читаемость кода и делает объекты более интуитивными в использовании.

### Основные принципы перегрузки
1. **Перегрузка выполняется с использованием методов класса или дружественных функций.**
2. **Не все операторы можно перегружать.** Например, нельзя перегружать `.` (точка), `::` (разрешение области видимости) и `sizeof`.
3. **Перегруженные операторы должны сохранять семантику оригинальных операторов.**
4. **Перегрузка может выполняться как методом класса или внешней функцией.** Операторы, изменяющие объект (`+=`, `-=`, `++`, `--`), должны быть методами класса.
5. **Операторы `<<` и `>>` должны быть дружественными функциями.** Это необходимо, так как левый операнд (`std::ostream` или `std::istream`) не является объектом пользовательского класса.

### Пример перегрузки операторов

Рассмотрим класс `Complex` для работы с комплексными числами.

```cpp
// complex.h
#ifndef COMPLEX_H
#define COMPLEX_H

#include <iostream>

class Complex {
private:
    double real, imag;

public:
    Complex(double real = 0, double imag = 0);
    
    friend Complex operator+(const Complex& a, const Complex& b);
    friend Complex operator-(const Complex& a, const Complex& b);
    friend Complex operator*(const Complex& a, const Complex& b);
    friend bool operator==(const Complex& a, const Complex& b);
    Complex& operator*=(const Complex& other);
    
    friend std::ostream& operator<<(std::ostream& os, const Complex& c);
};

#endif // COMPLEX_H
```

```cpp
// complex.cpp
#include "complex.h"

Complex::Complex(double real, double imag) : real(real), imag(imag) {}

Complex operator+(const Complex& a, const Complex& b) {
    return Complex(a.real + b.real, a.imag + b.imag);
}

Complex operator-(const Complex& a, const Complex& b) {
    return Complex(a.real - b.real, a.imag - b.imag);
}

Complex operator*(const Complex& a, const Complex& b) {
    return Complex(a.real * b.real - a.imag * b.imag, a.real * b.imag + a.imag * b.real);
}

bool operator==(const Complex& a, const Complex& b) {
    return a.real == b.real && a.imag == b.imag;
}

Complex& Complex::operator*=(const Complex& other) {
    double realPart = real * other.real - imag * other.imag;
    double imagPart = real * other.imag + imag * other.real;
    real = realPart;
    imag = imagPart;
    return *this;
}

std::ostream& operator<<(std::ostream& os, const Complex& c) {
    os << c.real << " + " << c.imag << "i";
    return os;
}
```

```cpp
// main.cpp
#include "complex.h"

int main() {
    Complex c1(2, 3), c2(1, 4);
    std::cout << "c1: " << c1 << "\nc2: " << c2 << "\n";
    std::cout << "c1 + c2: " << (c1 + c2) << "\n";
    std::cout << "c1 - c2: " << (c1 - c2) << "\n";
    std::cout << "c1 * c2: " << (c1 * c2) << "\n";
    std::cout << "c1 == c2: " << (c1 == c2) << "\n";

    c1 *= c2;
    std::cout << "c1 after *= c2: " << c1 << "\n";
}
```

## Задания

### Основное задание

Разработайте класс по вашему варианту и перегрузите указанные операторы.

1) **Двумерный вектор (`Vec2d`)**
   - Координаты `x`, `y`
   - Операторы: `+`, `-`, `<`, `>`, `*` (умножение на число), `<<`, `==`, `!=`, `+=`

2) **Прямоугольник (`Rect`)**
   - Ширина `w`, высота `h`
   - Операторы: `+`, `-`, `+=`, `++` (постфиксный), `/` (деление на число), `<<`, `==`, `!=`

3) **Вектор в полярных координатах (`VecPolar`)**
   - Длина `L`, угол `phi`
   - Операторы: `+`, `-`, `*` (умножение на число), `<`, `>`, `<<`, `==`, `!=`, `+=`

4) **Цвет в палитре RGB (`ColorRgb`)**
   - Компоненты `r`, `g`, `b`
   - Операторы: `+`, `-`, `-=`, `*` (умножение на число), `++`, `<<`, `==`, `!=`, `+=`

## Вывод
Перегрузка операторов позволяет работать с пользовательскими классами так же удобно, как со встроенными типами, улучшая читаемость кода. Важно соблюдать семантику операций и не перегружать операторы так, чтобы их поведение было неожиданным.

