# Корректность составления матрицы QUBO

## Построение матрицы QUBO
Введём булеву переменную $x_{i,v}$, которая равна 1, если коммивояжёр находится в вершине $v$ в момент времени $i$.

$$ H = A \cdot \sum_{i=1}^{N} \biggl( \sum_{v=1}^{N} x_{i,v} - 1 \biggr)^2 + B \cdot \sum_{v=1}^{N} \biggl( \sum_{i=1}^{N} x_{i,v} - 1 \biggr)^2 + C \cdot \sum_{i=1}^{N} \sum_{v=1}^{N} \sum_{u=1}^{N} x_{i,v} x_{i+1,u} \cdot w(v,u) $$

На практике ограничения на непосещение дважды одной и той же вершины и ненахождение коммивояжёра в нескольких городах одновременно имеют одинаковый вес для удобства: $A=B$, а $C$ должна быть такой, что $0 < C \cdot max_{\forall u,v}(w(u,v)) < A$. Берём верхнюю границу весов $100$, $A=B=10^7$, $C=1$. Также для фиксации начала обхода в первой вершинедобавим слагаемое $

<details>
  <summary>
    def MakeQuboSoloTSP(graph)
  </summary>
  
  ```
  def MakeQuboSoloTSP(graph) :
    amount_of_vertexes = len(graph)
    dimentions = amount_of_vertexes ** 2  # количество переменных x_i_v
    qubo = [[0] * dimentions for i in range(dimentions)]
    # x_i_v: i-время, v-вершина
    for i in range(0, amount_of_vertexes):
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
                        qubo[row][column] = graph[v][u]     
    qubo[0][0] -= 2 * kA #начало из нуля                                   
    return qubo
  ```
  
</details>

![Построение QUBO матрицы](images/code.jpg)

Задача: коммивояжёр с возвращением обратно.

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

## Полезные ссылки

Много Ising-сформулированных классических задач. [ссылка](https://arxiv.org/pdf/1302.5843.pdf).

Вариант Ising-формулировки TSP с раскрытием скобок. [ссылка](https://www.ece.ualberta.ca/~jhan8/publications/SB_TSP_ISOCC_Submitted.pdf).

