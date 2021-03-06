# **Численное интегрирование**

**Задача**: вычислить определенный интеграл, используя метод трапеции, двумя способами: 
 - используя прагму `for`;
 - без использования прагмы `for`, декомпозиция по номерам потоков.

В обеих программах необходимо порождать только одну параллельную секцию.

**Входные параметры**:
1. Число разбиений отрезка `N`.

Провести исследование ускорения и эффективности на количестве потоков `1`, `2`, `3`, `4`.

**Не забывайте об использовании обязательного флага `-fopenmp` для компиляции исполняемых файлов стандарта OMP и флага `-lm` для библиотеки `math.h`.**

## **Решение**

1. Используя прагму `for` — **[OmpFor](OmpFor)**
2. Без использования прагмы `for`, декомпозиция по номерам потоков
     - Используя механизм замков — **[OmpLock](./OmpLock)**;
     - Используя секцию `critical` — **[OmpCritical](./OmpCritical)**;

### **Значение интеграла**

Во всех реализациях исполняемый файл `solution.c` вычисляет и выводит значение 
интеграла. Необходимые входные параметры:
 - `N` — количество шагов по координате;
 - `size` — необходимое количество процессов;

**Компиляция и запуск**: 
```
gcc -fopenmp solution.c -lm && ./a.out N size
```

### **Время работы**

Во всех реализациях исполняемый файл `time.c` вычисляет и выводит время работы 
программы. Необходимые входные параметры:
 - `N` — количество шагов по координате;
 - `size` — необходимое количество процессов;
 - `numexp` — количество последовательных выполнений программы для усреднения 
времени работы;

**Компиляция и запуск**: 
```
gcc -fopenmp time.c -lm && ./a.out N size numexp
```

## **Результат**

Скрипт `build.sh` с параметрами `N numexp` во всех реализациях:
 - компилирует и запускает исполняемый файл `time.c` на запрошенном кол-ве процессов;
 - записывает результат в файл `./res/data.txt`, предварительно его очистив;
 - с помощью исполняемого файла `data.c` создает два дополнительных файла
`acceleration.txt` и `efficiency.txt`, в которых будут записаны ускорение и
эффективность соответственно;
 - с помощью `gnuplot` строит требуемые графики и сохранит их в папкe `res`.