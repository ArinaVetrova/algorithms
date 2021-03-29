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
3. Пока PLAN не пуст и цель поиска не достигнута делаем следующее:  
   3.1. GET: Извлекаем из PLAN какую-нибудь вершину v.  
   3.2. Посещаем вершину v. Если мы не просто обходим вершины, а что-то ищем, то здесь самое время обыскать вершину v на предмет достижения цели поиска.  
   3.3. Как-то отмечаем, что вершина v уже посещена.  
   3.4. PUT: Добавляем в PLAN все соседние с v вершины, которые еще не были посещены.  
Выводим результат поиска.  

От выбора контейера PLAN зависит, какой обход будет сделан. Стек- DFS (обход в глубину). Очередь - BFS (обход в ширину).

BFS:

```
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                add next to queue;
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

Шаблон BFS с проверкой, был посещен узел или нет
```
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> visited;  // store all the nodes that we've visited
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to visited;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to visited;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```
