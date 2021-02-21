# Linked list

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
 ```
 
# Reverse linked list
https://leetcode.com/problems/reverse-linked-list/

- Что сделать: реверсировать односвязный список

- Как: создаем 2 доп указателя - предыдущий и следующий. Запоминаем следующий --> у head перенаправляем указатель next на предыдущий 
--> предыдущий == head --> head == следущий, который запомнили ранее. 
```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        while (head){
            ListNode* tmp_next = head->next;
            head->next = prev;
            prev = head;
            head = tmp_next;
        }
           
        return prev;
    }
};
```

Какие делала ошибки при попытке самостоят решения:
- получались закольцованные указатели ( не использовала доп. указатели для хранения временных величин)
- использовала head++, это ошибка, ведь linked list - не последовательный контейнер и арифметика указателей тут не применима


# Intersection linked lists
https://leetcode.com/problems/intersection-of-two-linked-lists/
Решила сама ^.^
 - Что: найти узел, начиная с которого листы сливаются
 - Как: пройтись по двум спискам и сравнить каждый узел 
 
 Это плохое решение в лоб  
 
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* tmpA = headA; ListNode* tmpB = headB;
        while (headB)
        {
            tmpA=headA;
            while (tmpA)
            {
                if (tmpA == headB)
                    return tmpA;
                tmpA = tmpA->next;
            }
            headB = headB->next;
        }

        return nullptr;
    }
};

- Time: O(NxM)
- Memory: O(1)


Решение поизящнее 
- Как: проходимся по одному из списков и переписываем его в хэш-таблицу(set) --> проходимся по второму списку и если есть такой элемент в hash table, то возвращаем его
```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        std::unordered_set<ListNode*> hashB;
       while (headB)
       {
           hashB.insert(headB);
           headB = headB->next;
       }
    
        while (headA)
        {
            if (hashB.find(headA) != hashB.end()) 
                return headA;
            headA = headA->next;
        }
        
    return nullptr;
    }
};
```
- Time: O(N+M)
- Memory: O(N)

# Лист с закольцованным участком
https://leetcode.com/problems/linked-list-cycle-ii/
Решение в лоб
```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        std::unordered_set<ListNode*> hash;
        ListNode* curr = head;
        while (curr){
            if (hash.find(curr) != hash.end())
                return curr;
            hash.insert(curr);
            curr = curr->next;
        }
           
        return nullptr;
    }
};
```
Time: O(N)
Space: o(N)

Решение "Кролик и черепаха"
Как: один узел шагает медленно -  смотрит на след узел, а второй бежит вперед и смотрит через узел, если в списке есть цикл, черепаха и кролик сравняются в одной из точек

class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        
        while(fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
            if (fast == slow)
                return slow->next;
        }
           
        return nullptr;
    }
};

Time: O(N)
Space: O(1)
