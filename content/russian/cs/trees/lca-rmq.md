---
title: Сведение LCA к RMQ
weight: 2
authors:
- Сергей Слотин
---

Подойдём к задаче нахождения наименьшего общего предка с другой стороны.

Пройдёмся по дереву dfs-ом и выпишем два массива: глубины вершин и номера вершин. Записывать мы их будем как когда при входе в вершину, так и при выходе.

![](../img/tour.png)

Пусть теперь поступил запрос: найти LCA вершин $v$ и $u$. Для определенности предположим, что $v$ в обходе встретилась раньше: $tin_v < tin_u$. Посмотрим на часть выписанного пути между моментом, когда мы *вышли* из $v$ и моментом, когда мы первый раз *вошли* в $u$. Так как любой простой путь между двумя вершинами в дереве единственный, где-то на этом отрезке мы должны были прийти в наименьший общий предок. При этом мы на этом пути точно не поднимались выше LCA, а значит LCA — это самая высокая вершина на этом пути.

Получается, что чтобы найти LCA, можно найти позицию минимума на отрезке $[tout_v, tin_u]$ в массиве глубин (первый выписанный массив) и посмотреть, какой вершине она соответствует в эйлеровом обходе (второй выписанный массив). Таким образом, задачу LCA можно свести к задаче RMQ (нахождению минимума на отрезке), что можно сделать, например, [деревом отрезков](/cs/range-queries/segment-tree).

Однако, если использовать дерево отрезков или любые другие деревья, то асимптотику мы особо не улучшили — всё равно требуется $O(\log n)$ времени на запрос в дерево отрезков, хоть преподсчёт и будет уже линейным.

Асимптотику времени запроса можно улучшить, используя тот факт, что мы на самом деле решаем задачу *static RMQ*, то есть у нас нет изменений массива. Для этого есть более подходящая структура — [разреженная таблица](/cs/range-queries/sparse-table), которая позволяет отвечать на запрос минимума за $O(1)$, но, как и двоичные подсчеты, использует $O(n \log n)$ операций и памяти на препроцессинг с малой константой.