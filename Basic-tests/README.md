# Корректность составления матрицы QUBO

## Построение матрицы QUBO
Введём булеву переменную $x_{i,v}$, которая равна 1, если коммивояжёр находится в вершине $v$ в момент времени $i$.

$$ H = A \cdot \sum_{i=1}^{N} \biggl( \sum_{v=1}^{N} x_{i,v} - 1 \biggr)^2 + B \cdot \sum_{v=1}^{N} \biggl( \sum_{i=1}^{N} x_{i,v} - 1 \biggr)^2 + C \cdot \sum_{i=1}^{N} \sum_{v=1}^{N} \sum_{u=1}^{N} x_{i,v} x_{i+1,u} \cdot w(v,u) $$

* Первое слагаемое отвечает за то, чтобы в каждый момент времени коммивояжёр находился только в одной вершине.
* Второе слагаемое отвечает за то, чтобы каждая вершина посещалась единожды.
* Последнее слагаемое отвечает за минимизацию длины пути.

На практике ограничения на непосещение дважды одной и той же вершины и ненахождение коммивояжёра в нескольких городах одновременно имеют одинаковый вес для удобства: $A=B$, а $C$ должна быть такой, что $0 < C \cdot max_{\forall u,v}(w(u,v)) < A$. Берём верхнюю границу весов $100$, $A=B=10^7$, $C=1$. Также для фиксации начала обхода в первой вершине добавим слагаемое $-2A \cdot x_{1,1}$.

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

Задача коммивояжёра с возвращением обратно и без возвращения сводятся друг к другу при помощи манипуляции с весами, поэтому здесь и далее считаем решение обеих задач равносильным.

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

Сведение замкнутой и незамкнутой постановок друг к другу. [ссылка](https://ru.wikipedia.org/wiki/%D0%97%D0%B0%D0%B4%D0%B0%D1%87%D0%B0_%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D0%B2%D0%BE%D1%8F%D0%B6%D1%91%D1%80%D0%B0#%D0%97%D0%B0%D0%BC%D0%BA%D0%BD%D1%83%D1%82%D1%8B%D0%B9_%D0%B8_%D0%BD%D0%B5%D0%B7%D0%B0%D0%BC%D0%BA%D0%BD%D1%83%D1%82%D1%8B%D0%B9_%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B_%D0%B7%D0%B0%D0%B4%D0%B0%D1%87%D0%B8).
