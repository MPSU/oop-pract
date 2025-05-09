# Лабораторная работа 9: Простое рисование

## Задания

Разработать приложение Qt, которое рисует в окне несколько простых геометрических фигур: треугольники, прямоугольники и круги;

Параметры фигур (координаты вершин, размеры, радиусы) должны задаваться в JSON-файле,
и считываться с помощью модифицированного `JsonReader` из ЛР5.

## Порядок выполнения работы

1) В первую очередь разработать Виджет, который будет заниматься отрисовкой фигур. Виджет наследовать от QWidget, как делалось в ЛР4 (наследование).
2) Виджет должен хранить внутри себя список фигур (`std::vector`). Так как фигуры разные и отрисовываются по-разному, то следует
   создать базовый класс (`Figure`), от которого все фигуры будут наследоваться (см. ЛР4 и ЛР5).
   В `std::vector` хранить `std::unique_ptr` на базовый класс фигур.
4) Базовый класс фигуры должен иметь метод `paint`, который будет переопределяться в каждом классе-наследнике и будет рисовать фигуру.
5) Виджет в конструкторе должен прочитать JSON-файл и сформировать `std::vector<std::unique_ptr<Figure>>`.
6) Затем, когда нужно будет нарисовать фигуры (в методе `paintEvent`), нужно просто вызвать метод `paint` для каждой фигуры в векторе.
   И фигура сама будет рисовать себя в соответствии со своим типом.

**Подсказка:** для хранения координат точек удобно использовать класс `QPoint`.

## Умный указатель `std::unique_ptr<>`

`std::unique_ptr` -- это контейнерный класс, который хранит объект другого класса и управляет его временем жизни.
Объект внутри `std::unique_ptr` создается с помощью оператора `new` или фабричного метода `std::make_unique`.
Когда `std::unique_ptr` уничтожается, он автоматически запустит деструктор объекта который хранит (`delete`),
таким образон он защищает ресурсы от утечки (забытые вызовы `delete`, незакрытые файлы и т.п.).

Особенностью `std::unique_ptr` является то что он запрещает копирование объекта который хранит, отсюда и название *unique*,
то есть объект уникальный, в единственном экземпляре. `std::unique_ptr` можно перемещать с помощью семантики перемещения и `std::move`.

Пример работы с `std::unique_ptr`
```cpp
// std::unique_ptr определён в файле memory, подключаем его
#include <memory>

// Объявляем умный указатель на Figure (пока пустой)
std::unique_ptr<Figure> f1;

// Создаем объект с помощью new, который будет храниться в std::unique_ptr
f1 = std::unique_ptr<Figure>(new Figure(/* параметры конструктора */));

// С помощью std::make_unique код выглядит проще:
auto f2 = std::make_unique<Figure>(/* параметры конструктора */);

// Для упрощения кода объявим псевдоним для FigurePtr:
using FigurePtr = std::unique_ptr<Figure>;

// Создадим и наполним вектор Фигур
std::vector<FigurePtr> figs;

for (....) {
   figs.push_back(std::make_unique<Figure>(/* параметры */);
}
```

## Рисование в Qt

Для того чтобы нарисовать что-то на поверхности виджета следует переопределить метод `paintEvent`, который есть у каждого `QWidget`.
Метод `paintEvent` автоматически вызывается каждый раз, когда окно требуется перерисовать, например когда оно только что появилось,
или развернулось из свернутого, или изменился его размер.

```cpp
// Прототип:
virtual void paintEvent(QPaintEvent *event) override;

// Реализация:
void MyWidget::paintEvent(QPaintEvent* event)
{
    // Создаем экземпляр класса QPainter
    // и передаем ему указатель на Виджет на котором надо рисовать
    QPainter painter(this);

    // Рисуем линию с началом в точке (50, 50) и концом в точке (100, 100)
    painter.drawLine(50, 50, 100, 100);
}
```

Рисование в Qt производится с помощью класса [`QPainter`](https://doc.qt.io/qt-6/qpainter.html).
В арсенале `QPainter` как у хорошего художника есть много инструментов рисования:
* класс `QPen` -- карандаш для рисования простых линий;
* класс `QBrush` -- кисть для закрашивания контуров;
* метод `drawLine` -- для рисования отрезков;
* метод `drawRect` -- для рисования прямоугольников;
* метод `drawEllipse` -- для рисования эллипсов и кругов;

Пример рисования круга:

```cpp
void MyWidget::paintEvent(QPaintEvent* event)
{
    // Создаем экземпляр класса QPainter
    // и передаем ему указатель на Виджет на котором надо рисовать
    QPainter painter(this);

    // Создаем карандаш красного цвета, толщиной 2 пикселя
    QPen pen(Qt::red, 2);

    // Даем карандаш художнику
    painter.setPen(pen);

    // Создаем кисть черного цвета
    QBrush brush(Qt::black);

    // Даем кисть художнику
    painter.setBrush(brush);

    // Рисуем эллипс с центром в точке (200, 200) и одинаковыми радиусами 50 пикселей:
    painter.drawEllipse(200, 200, 50, 50);
}
```
