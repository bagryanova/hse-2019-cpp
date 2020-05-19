Не было на лекции, а стоило.

# Тест t12
* Задания 2 и 3
  * `move` при возврате переменной при помощи `return` не надо (даже в C++11),
    а с C++17 там даже не нужен мув конструктор: https://stackoverflow.com/questions/4316727/returning-unique-ptr-from-functions/4316948#4316948

# Подробности про SFINAE
* SFINAE Работает только в сигнатуре, не работает в параметрах по умолчанию и теле функции (там "hard error").
* Есть разница между `foo_v<X>` и `foo<X>::value`: в первом случае hard error, во втором SFINAE из-за отсутствия value.
* SFINAE: можно пихнуть void_t<>* = nullptr в параметр функции, а не шаблона.
* ОШИБКА НА ЛЕКЦИИ: `&U::Foo` работает, даже если `foo` объявлен в родителе.
* `static_assert` можно делать вообще в любом месте файла, даже вне функций и внутри класса.
* ОШИБКА НА ЛЕКЦИИ: `auto foo(T a) -> decltype(a.foo()) { return a.foo(); }` работает.
  * Можно в макрос обернуть.
* SFINAE: трюк с `decltype(a + b, std::declval<int>())` или `void_t<decltype(a + b)>`.
* Проверка существования типа через `void_t<typename T::iterator>`
* Member detection можно делать без внешней структуры-обёртки, так проще.

## Включение-выключение типов (специализация по условию) [00:05]
* Задача: специализация по условию. Например, для всех `is_integral_v<T>`.
* Идея: в специализациях и в параметрах шаблона по умолчанию тоже работает SFINAE.
* Давайте добавим в шаблон фиктивный параметр (к сожалению, это надо сделать: https://github.com/emscripten-core/emscripten/pull/9089/files )
* А теперь пишем специализацию для `Foo<T, enable_if_T<cond<T>>`: мы второй параметр делаем либо `void`, либо SFINAE.

### Выбор полей и родителей класса через наследование.
* Например, когда у нас тип — `void`. Можно ещё свою обёртку сделать, которая занимает один байт, и её специализировать.

### tag dispatching (уже был)
Полезно применять вместо SFINAE и метапрограммирования, если есть возможность.