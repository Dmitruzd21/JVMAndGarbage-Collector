# **Анализ кода**

```java
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```

## **1.** **`int i = 1;`**

Предварительно Application ClassLoader загружает класс JvmComprehension c последующей проверкой, подготовкой статических полей, разрешением символьных ссылок.

В Metaspace помещается мета-информация о классе JvmComprehension: имя, методы, поля, константы.

В Stack Memory помещается фрейм метода main.

Переменная i также попадает в Stack Memory (фрейм main).

## **2.** **`Object o = new Object();`**

Предварительно Bootstrap ClassLoader/Platform ClassLoader загружают класс Object c последующей проверкой, подготовкой статических полей, разрешением символьных ссылок.

В Metaspace помещается мета-информация о классе Object: имя, методы, поля, константы.

В heap попадает обект Object.

В Stack Memory (фрейм метода main) помещается ссылка на объект Object.

## **3.** **` Integer ii = 2;`**

Предварительно Bootstrap ClassLoader/Platform ClassLoader загружают класс Integer c последующей проверкой, подготовкой статических полей, разрешением символьных ссылок.

В Metaspace помещается мета-информация о классе Integer: имя, методы, поля, константы.

В heap попадает объект Integer.

В Stack Memory (фрейм метода main) помещается ссылка на объект Integer.

## **4.** **`printAll(o, i, ii);`**

В Stack Memory помещается фрейм метода printAll.

Метод обращается к heap для получения 1го параметра - объекта Object.

Метод обращается к Stack Memory для получения 2го аргумента - int.

Метод обращается к heap для получения 3го аргумента - объекта Integer.

## **5.** **`Integer uselessVar = 700;`**

Загрузка класса не происходит, поскольку уже загружен ранее.

В heap попадает новый объект Integer.

В фрейм метода printAll (Stack Memory) помещается ссылка на объект Integer.

## **6.** **`System.out.println(o.toString() + i + ii);`**

Предварительно Bootstrap ClassLoader/Platform ClassLoader загружают класс System.

В Metaspace помещаются данные о классе System.

В Stack Memory создаются фреймы методов out и println.

Метод println обращается к heap к объекту Object, чтобы вызвать его метод toString.

## **7.** **`System.out.println("finished");`**

В Stack Memory создаются фреймы методов out и println.

Далее автоматически создается обект String (аргумент), который помещается в heap.

В фрейм помещается ссылка на String.

## **Сборщик мусора**

Периодически происходит остановка программы и сборка мусора.

Недостижимые объекты удаляются.

Достижимые объекты группируются по времени жизни (поколения).
Чем дольше объект живёт, тем реже проверяют, нужно ли его удалить

## **Execution Engine**

**Интерпретатор**

● Байт-код в файле .class интерпретирует
строку за строкой, а затем выполняет

**Just In Time (JIT) компилятор**

● Используется для повышения эффективности
интерпретатора

● Чтобы ускорить интерпретатор, он проверяет, как часто
вызывается какой-то метод. Если слишком часто,
то компилирует его код и сохраняет в кеше.
При последующих вызовах метода он выполняет готовый
машинный код, взятый из кеша, что происходит
значительно быстрее
