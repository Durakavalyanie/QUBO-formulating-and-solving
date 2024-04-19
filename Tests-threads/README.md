# Polynom тесты на разных семплерах

## Stepest Decent Sampler

Результат с 30 вершинами, 100000 случайных начальных состояний

![](polynom/stepest_test_1.png)

Результат с 30 вершинами, 1000000 случайных начальных состояний
Время работы - ~15 минут

![](polynom/stepest_test_2.png)

Результат с 30 вершинами, 10000 случайных начальных состояний, 20 потоков
Время работы - ~1,5 минут

![](polynom/steepest_test_threads_1.png)

Результат с 30 вершинами, 20000 случайных начальных состояний, 20 потоков
Время работы - ~2 минуты

![](polynom/steepest_test_threads_2.png)

Результат с 30 вершинами, 10000 случайных начальных состояний, 40 потоков
Время работы - ~2,3 минуты

![](polynom/steepest_test_threads_3.png)

[//]: # (![]&#40;polynom/sticker.webp&#41;)

Результат с 30 вершинами, 30000 случайных начальных состояний, 50 потоков
Время работы - ~??? минуты

![](polynom/steepest_test_threads_.png)

## Simulated Annealing Sampler

Не знаю что происходит, все ужасно...
Не получилось подобрать параметры для более менее нормальных результатов

![annealing_parameters_1.png](polynom%2Fannealing_parameters_1.png)
![simulated_annealing_1.png](polynom%2Fsimulated_annealing_1.png)![simulated_1.png](random%2Fsimulated_1.png)

# Random тесты на разных семплерах

## Simulated Annealing

![annealing_parameters_1.png](polynom/annealing_parameters_1.png)
![simulated_1.png](random/simulated_1.png)

## Steepest Decent Solver

Кол-во вершин - 30, кол-во начальных состояний - 20000, потоков - 10
Время выполнения - ~4 мин

![steepest_1.png](random/steepest_1.png)

Кол-во вершин - 30, кол-во начальных состояний - 20000, потоков - 10
Время выполнения - ~1.5 мин

![steepest_2.png](random/steepest_2.png)


Кол-во вершин - 20, кол-во начальных состояний - 10000, потоков - 10
Время выполнения - ~30 сек

![steepest_random_2.png](polynom%2Fsteepest_random_2.png)![steepest_2.png](random/steepest_.png)
