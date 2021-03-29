# algorithms - ссылка на репозиторий
https://github.com/ArinaVetrova/algorithms

- Хорошая статья с программой обучения алгоритмам и структурам данных
https://leetcode.com/discuss/general-discussion/1129503/powerful-studying-program-for-beginners-and-intermediate-levels-all-common-mistakes-analyzed

- Принцип построения рекурсивного решения
https://leetcode.com/explore/learn/card/recursion-i/250/principle-of-recursion/1680/

- BFS - https://leetcode.com/explore/learn/card/queue-stack/231/practical-application-queue/
Breadth-first search (BFS) is an algorithm to traverse or search in data structures like a tree or a graph.

https://cpp.mazurok.com/tag/bfs/

1. Заводим PLAN поиска — контейнер данных, где будем хранить вершины в которых мы планируем побывать. Изначально он пуст.
2. Добавляем в PLAN поиска исходную вершину с которой нам предписано начать.
3. Пока PLAN не пуст и цель поиска не достигнута делаем следующее
   3.1 GET: Извлекаем из PLAN какую-нибудь вершину v.
   3.2 Посещаем вершину v. Если мы не просто обходим вершины, а что-то ищем, то здесь самое время обыскать вершину v на предмет достижения цели поиска.
   3.3 Как-то отмечаем, что вершина v уже посещена.
   3.4 PUT: Добавляем в PLAN все соседние с v вершины, которые еще не были посещены.
Выводим результат поиска.
