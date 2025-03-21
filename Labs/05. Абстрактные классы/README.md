**Лаборатораня работа 5**

*Задание:*

1) В код ЛР4 добавить абстрактный класс `AbstractReader` и наследовать `CsvReader` от него;

2) Добавить класс `JsonReader`, наследованный от `AbstractReader`, который будет считывать данные из фалов формата JSON;

   *Использовать библиотеку [nlohmann::json](https://github.com/nlohmann/json),
    скачав файл [json.hpp](https://github.com/nlohmann/json/blob/develop/single_include/nlohmann/json.hpp).

```cpp
std::string name;

// read a JSON file
std::ifstream fin("file.json");
json j;
fin >> j;
for (const auto& e: j) {
    e["name"].get_to(name);
    // ....
}
```

4) Добавить кнопку `Обзор`, при нажатии на которую будет появляться стандартное диалоговое окно выбора файлов,
   и можно будет выбрать файл из которого прочитать все данные;

   *Использовать `QFileDialog` для диалога открытия файла.
