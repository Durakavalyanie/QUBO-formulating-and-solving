# Корректность составления матрицы QUBO

## Построение матрицы QUBO

<details>

<summary>
Построение QUBO матрицы
</summary>

```
    for i in range(0, amount_of_vertexes):  # строим кубо матрицу
        for j in range(0, amount_of_vertexes):
            for v in range(0, amount_of_vertexes):
                for u in range(0, amount_of_vertexes):
                    row = i * amount_of_vertexes + v
                    column = j * amount_of_vertexes + u
                    if (row == column):
                        qubo[row][column] = -2 * kA
                    elif (i == j or u == v):
                        qubo[row][column] = 4 * kA
                    elif (abs(j - i) == 1):
                        qubo[row][column] = kWeight * graph[v][u]
```
  
</details>

Решателем в следующих тестах является Steepest Descent Solver by D-Wave

## Тест на многоульнике из 10 вершин

Параметр reads = 1000

![](images/test1.jpg)

Параметр reads = 1200

![](images/test2.jpg)

### Вывод

На малом кол-ве вершин можно добиться нужной точности ответа за приемлемое время выполнения

## Тест на многоульнике из 30 вершин

Параметр reads семплера=1000

![](images/test30_1.png)

Параметр reads семплера=5000

![](images/test30_2.png)

Как можно видеть мало что поменялось


Параметр reads семплера=30000

![](images/test30_3.png)

Как бы не повышали точность, семплер выдает неприемлемый ответ за непростительное время работы

