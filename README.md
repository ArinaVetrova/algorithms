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
#include <iostream>
#include <queue>
using namespace std;
int main() {
    // чтение исходных данных
    int n, v;
    cin >> n >> v;
    int matrix[n][n];
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> matrix[i][j];
    queue <int> plan; // план посещения в виде очереди
    plan.push(--v);   // мы нумеруем с 0, а не с 1
    matrix[v][v] = 1; // отмечаем, что эта вершина уже заносилась в план 
    int counter = 1;  // начальную уже сосчитали
    while (!plan.empty()) {
        v = plan.front(); // посещаем следующую по плану вершину 
        plan.pop();       // удаляем ее из плана посещения
        for (int u = 0; u < n; u++) { // перебираем соседние с ней
            if (matrix[v][u] and !matrix[u][u]) { // если новая, то
                plan.push(u);     // добавляем ее в план
                matrix[u][u] = 1; // отмечаем, что уже не новая
                counter++;        // считаем, сколько было вершин
            }
        }
    }
    cout << counter << endl;
}
```
