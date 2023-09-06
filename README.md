# SmartPointers 

## SharedPtr

- Конструкторы
  - SharedPtr() - по умолчанию 
  - SharedPtr(Y* ptr) - конструктор от c-style указателя. Может быть как указатель на T, так и указатель на наследника T
  - SharedPtr(const SharedPtr<Y>&) - конструктор копирования 
  - SharedPtr(SharedPtr<Y>&&) - move-конструктор 
  - SharedPtr(Y* ptr, Deleter), где Y - либо T либо наследник T. Deleter - класс в котором есть ```operator()(Y*)```. Этот оператор нужно вызывать вместо уничтожения объекта под шаредом (освобождение памяти делать не нужно).
  - SharedPtr(Y* ptr, Deleter, Alloc), где Alloc - пользовательский аллокатор. Этот аллокатор используется для создания сущностей в динамической памяти, которые нужны для работы шареда. Аллокатор не должен применяться к уничтожению и освобождению ресурса (за это отвечает Deleter).
- operator=(const SharedPtr<Y>&) - оператор присваивания копированием
- operator=(SharedPtr<Y>&&) - оператор присваивания перемещением
- Деструктор - если при смерти шаред обнаруживает, что пора уничтожать объект, но объект на самом деле типа Y, где Y наследник T, то объект должен быть уничтожен как Y. При этом если был передан кастомный Deleter то объект должен быть уничтожен с помощью него.
- use_count - возвращает количество шаредов владеющих объектом.
- Метод get для получения c-style указателя
- операторы * и ->
- Метод reset() 

## Методы вне класса

- MakeShared - создает SharedPtr из аргументов. Эта функция должна обращаться к new ровно 1 раз. Не забудьте про форвардинг аргументов
- AllocateShared - делает то же что и MakeShared но с кастомным аллокатором. Этот же аллокатор должен быть использован для уничтожения и освобождения памяти под объект и под сущности шареда.

## WeakPtr

- Конструкторы
  - WeakPtr()
  - WeakPtr(const WeakPtr<Y>&)
  - WeakPtr(const SharedPtr<Y>&)
  - WeakPtr(WeakPtr<Y>&&)
- operator=(const WeakPtr<Y>&) - оператор присваивания копированием
- operator=(WeakPtr<Y>&&) - оператор присваивания перемещением
- Деструктор
- expired - возвращает True если объект под виком все еще валиден (на него есть шаред)
- lock - возвращает SharedPtr на объект (если объект еще жив, иначе UB).
