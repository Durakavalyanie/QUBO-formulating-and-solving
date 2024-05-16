# Влияние характера распределения вершин на точность ответа
## Построение
Рассматриваются три типа распределения вершин в графе:
* В форме правильного многоугольника (вершины расположены в форме правильного многоугольника и вес ребра равен евклидовому расстоянию).
* Случайное геометрическое (случайное расположение вершин на плоскости и вес ребра равен евклидовому расстоянию).
* Абстрактное (каждому ребру присваивается случайный вес).

<details>
  
<summary>
   Построение правильного многоугольника
</summary>

```
def GeneratePolygon(amount_of_vertexes) :
    graph = [[0] * amount_of_vertexes for i in range(amount_of_vertexes)]
    vertexes = []
    for i in range(amount_of_vertexes):
        angle = 2 * i * 3.14159 / amount_of_vertexes  # Вычисление угла для каждой вершины
        x = round(100 * (0.5 + 0.5 * math.cos(angle)), 2)  # X координата
        y = round(100 * (0.5 + 0.5 * math.sin(angle)), 2)  # Y координата
        vertexes.append((x, y))

    for i in range(amount_of_vertexes) :
        for j in range(amount_of_vertexes) :
            if (i == j) :
                continue
            graph[i][j] = round(euclidean_distance(vertexes[i], vertexes[j]), 2)
    return graph
```

</details>

<details>

<summary>
  Построение случайного распределения.
</summary>

  ```
  def GenerateFullPlot(amount_of_vertexes) :
    graph = [[0] * amount_of_vertexes for i in range(amount_of_vertexes)]
    vertexes = []
    for i in range(amount_of_vertexes):
        x = random.randint(1, 100)  # X координата
        y = random.randint(1, 100)  # Y координата
        vertexes.append((x, y))    

    for i in range(amount_of_vertexes) :
        for j in range(amount_of_vertexes) :
            if (i == j) :
                continue
            graph[i][j] = round(euclidean_distance(vertexes[i], vertexes[j]), 2)
    return graph
  ```
  
</details>

<details>

<summary>
  Построение абстрактного графа.
</summary>

  ```
def GenerateFullGraph(amount_of_vertexes):
    graph = [[0] * amount_of_vertexes for i in range(amount_of_vertexes)]
    for i in range(amount_of_vertexes):
        for j in range(i + 1, amount_of_vertexes):
            weight = random.randint(1, kMaxWeight)
            graph[i][j] = weight
            graph[j][i] = weight
    return graph
  ```
</details>

## Результаты

### Steepest Descent, 20 vertexes

| |Polygon |Random|Abstract|
| ------- | ------- | ------- |------|
|gap| 44%| 25.5%|49.9%|
|time, sec.| 1.24 |1.22|1.21|

### Simulated Annealing, 10 vertexes

||Polygon|Random|Abstract|
| ------- | ------- | ------- |-----|
|gap| 27.0%| 14.5%| 33.1%|
|time, sec.| 6.16| 5.91|5.54|

### Tabu Sampler, 15 vertexes

||Polygon|Random|Abstract|
| ------- | ------- | ------- |-----|
|gap| 0.1%| 44.1%| 88.0%|
|time, sec.| 0.2141| 0.2142|0.2149|

Поведение Tabu Sampler'а на правильном многоугольнике очень сильно выделяется на фоне остальных результатов. Действительно, на таком графе вплоть до 60 вершин сэмплер выдаёт почти идеальный ответ за num_reads <= 10 и с параметрами по умолчанию, и с параметрами tenure = 0, num_restarts = 0. Ниже приведена таблица со временем достижения среднего gap <= 1% за 50 тестов.

|Vertexes|10|15|20|25|30|35|40|45|50|55|60|
|---|---|---|---|---|---|---|---|---|---|---|---|
|Time, sec.|0.148|0.1377|0.1441|0.1481|0.2243|0.2559|0.3271|0.4928|1.0741|1.2042|2.2069|

## Примеры

 | |Polygon|Random|
 |---|---|---|
 |SD| ![](images/Polygon_SD_20.png) | ![](images/Random_SD_20.png)|
 |Dynam| ![](images/Polygon_Dynam_20.png)| ![](images/Random_Dynam_20.png) |

 На правильном многоугольнике отклонение от эталонного ответа, полученного с помощью решения динамическим программированием, составило ~ 40%.

 На случайном распределении отклонение составило ~ 16%.

 Во всех тестах использовалось значение num_reads = 10 000.
