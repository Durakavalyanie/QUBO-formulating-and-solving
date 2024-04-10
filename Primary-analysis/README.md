# Первичный анализ
## Постановка задачи
Дана карта городов, между любыми двумя есть двусторонняя дорога, движение по которой занимает фиксированное время. В одном определённом городе находится
коммивояжёр, который должен посетить все города за наименьшее время (возвращение в начальный город не учитывается). Нужно найти такой маршрут.


Для тестов использовалась генерация полных графов со случайными весами рёбер от 1 до 100 включительно.


<details>
<summary>
  def GenerateFullGraph
</summary>
  
```
def GenerateFullGraph(amount_of_vertexes) :
    graph = [[0] * amount_of_vertexes for i in range(amount_of_vertexes)]
    for i in range(amount_of_vertexes) :
        for j in range(i + 1, amount_of_vertexes) :
            weight = random.randint(1, kMaxWeight)
            graph[i][j] = weight
            graph[j][i] = weight
    return graph
```
</details>

В качестве эталонного ответа использовался результат динамического решения задачи из библиотеки [python_tsp](https://pypi.org/project/python_tsp/).
## Steepest Descent
Результаты по итогам 300 тестов с использованием сэмлера Steepest Descent.

![1 diagram](images/SD-def-unfixed-exact.jpg)

![2 diagram](images/SD-def-unfixed-coef.jpeg)

![3 diagram](images/SD-def-unfixed-time.jpg)

![4 diagram](images/SD-def-fixed-exact.jpeg)

![5 diagram](images/SD-def-fixed-coef.jpeg)

![6 diagram](images/SD-def-fixed-time.jpg)
