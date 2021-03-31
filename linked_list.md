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

# Intersection linked lists
https://leetcode.com/problems/intersection-of-two-linked-lists/

 - Что: найти узел, начиная с которого листы сливаются
 - Как: пройтись по двум спискам и сравнить каждый узел. Для вложенного цикла завести временный указатель
и при каждой итерации главного цикла направлять его на head заново.
 
 Это плохое решение в лоб  
``` 
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
```
- Time: O(NxM)
- Memory: O(1)


Решение поизящнее 
- Как: проходимся по одному из списков и переписываем его в хэш-таблицу(set) --> проходимся по второму списку и если есть такой элемент в hash table,
то возвращаем его
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
Как: шагаем по списку, если этот узел не встречался в хэш-таблице - заносим, если есть в таблице - его и возвращаем.
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
[Сcылка на вики c описанием алгоритма Флойда для нахождения цикла](https://ru.wikipedia.org/wiki/%D0%9D%D0%B0%D1%85%D0%BE%D0%B6%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5_%D1%86%D0%B8%D0%BA%D0%BB%D0%B0#:~:text=%D0%90%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC%20%D0%A4%D0%BB%D0%BE%D0%B9%D0%B4%D0%B0%20%D0%BF%D0%BE%D0%B8%D1%81%D0%BA%D0%B0%20%D1%86%D0%B8%D0%BA%D0%BB%D0%B0%20%E2%80%94%20%D1%8D%D1%82%D0%BE,%D0%AD%D0%B7%D0%BE%D0%BF%D0%B0%20%C2%AB%D0%A7%D0%B5%D1%80%D0%B5%D0%BF%D0%B0%D1%85%D0%B0%20%D0%B8%20%D0%B7%D0%B0%D1%8F%D1%86%C2%BB.)

Как: один узел шагает медленно -  смотрит на след узел, а второй бежит вперед и смотрит через узел, если в списке есть цикл, черепаха и кролик сравняются в одной из точек.

```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        // edge case - empty list
        if (!head || !head->next || !head->next->next) return NULL;
        // support animals
        ListNode *turtle = head, *hare = head;
        // checking if we loop or not
        while (hare->next && hare->next->next) {
            hare = hare->next->next;
            turtle = turtle->next;
            if (hare == turtle) break;
        }
        // exiting if we do not find a loop
        if (hare != turtle) return NULL;
        // finding the start of the loop
        turtle = head;
        while (turtle != hare) {
            hare = hare->next;
            turtle = turtle->next;
        }
        return turtle;
    }
};
```
Time: O(N)
Space: O(1)


# Палиндром односвязный список
Палиндром - слово или фраза, которая одинаково читается с конца и с начала.
https://leetcode.com/problems/palindrome-linked-list/

Что: Given a singly linked list, determine if it is a palindrome. Input: 1->2->2->1  Output: true

- Итеративный алгоритм
Как: переписываем лист в вектор, идем одновременно с начала и конца и сравниваем элементы. Если на какой-то итерации не равны - не палиндром.
```
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        std::vector<int> v;
        ListNode* curr = head;
        while (curr)
        {
            v.push_back(curr->val);
            curr = curr->next;
        }
        
        for (int i=0, j = v.size()-1 ; i<v.size()/2; i++,j--)
        {
            if (v[i] != v[j])
                return false;
        }
        return true;
    }
};
```
Time: O(N+N/2) --> O(N), т.к. при оценке сложности константы опускаются
Space: O(N)

- Рекурсивный алгоритм
Как: 
```
class Solution {
public:
    ListNode* p_front;
    bool recursiveCheck(ListNode* curr){
        if (curr){
            if (!recursiveCheck(curr->next))
                return false;
            if (curr->val != p_front->val)
                return false;
            
            p_front = p_front->next;
        }
        return true;
    }
    
    bool isPalindrome(ListNode* head) {
        p_front = head;
        return recursiveCheck(head);
    }
};
```
Time: O(N)
Space: O(N) (новую память не занимаем. но она занята из-за особенности вызова рекурсивных функций, т.к. пока алгоритм идет вглубь рекурсии, в программе зранится весь стек вызовов и данные вызвавших ф-ций

# Remove Nth node from end of list
https://leetcode.com/problems/remove-nth-node-from-end-of-list/
- Наивный алгоритм
Как: Считаем длину списка, потом второй раз шагаем до узла (L - n ), начиная с dummy, перенапраялем указатель на следующий за удаляемым. Возвращаем dummy->next. 
Завести узел-заглушку (dummy) dummy = new ListNode(); dummy->next = head;  этот узел нужен для случая когда на вход подается [1] и надо удалить единственный узел
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(); // этот узел нужен для случая когда на вход подается [1] и надо удалить единственный узел
        dummy->next = head;
        ListNode* curr = head;
        size_t size = 0;
        while (curr)
        {
            size++;
            curr = curr->next;     
        }
        curr = dummy;
        int i = 0;
        while (curr && i < size-n)
        { 
            curr = curr->next;
            i++;
        }
        curr->next = curr->next->next;
        
        return dummy->next;
    }
};
```
Time: O(N)
Space: O(1)

- Решить за 1 проход
Как: берем 2 указателя. Первый перемещаем на n+1 от начала, второй - на начале. Т.о. м-ж указателями разница в n узлов. Когда первый узел достигает конца, второй окажется на n-том узле от последнего. Перенаправляем указатели. Профит
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode();
        dummy->next = head;
        ListNode* slow = dummy; ListNode* fast = dummy;
        int i = 0;
        while(fast->next){
            if (i>=n)
                slow=slow->next; 
            fast=fast->next;
            i++; 
        }
        slow->next = slow->next->next;
        return dummy->next;
    }
};
```

# Middle of the Linked List
https://leetcode.com/problems/middle-of-the-linked-list/
1. Как: переписать в вектор и вернуть его средний эл-т
```
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        vector<ListNode*> vec;
        while(head){
            vec.push_back(head);
            head=head->next;
        }
        return vec[vec.size()/2];
    }
};
```
Time: O(N)
Space: O(N)

2. Как: по методу "кролика и черепахи"
Кролик бежит по узлам в 2 раза быстрее, поэтому, когда он достигнет конца списка, черепаха будет в середине.
```
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
```
Time: O(N)
Space: O(1)

# Merge Two Sorted Lists

https://leetcode.com/problems/merge-two-sorted-lists/

Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.

- Рекурсивный алгоритм
Как: 
```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
         if (!l1 )
             return l2;
        else if (!l2)
            returm l1;
        else if (l1->val < l2->val){
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        else
        {
            l2->next =  mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```
Time: O(N+M)
Space: O(n + m)O(n+m)
The first call to mergeTwoLists does not return until the ends of both l1 and l2 have been reached, so n + mn+m stack frames consume O(n + m)O(n+m) space.

- Итеративный алгоритм
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
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* prehead = new ListNode();
        ListNode* prev = prehead;
        
       if (!l1 && !l2)
            return nullptr;
        if(!l1)
            return l2;
        if(!l2)
            return l1;
        
        while (l1 && l2){
            if (l1->val <= l2->val)
            {
                prev->next = l1;
                l1=l1->next;
            }
            else
            {
                prev->next = l2;
                l2=l2->next;
            }
            prev = prev->next;
        }
        
        prev->next = l1 ? l1 : l2;
        return prehead->next;
    }
};
```
Time: O(N+M)
Space: O(1)

# Reorder list
https://leetcode.com/problems/reorder-list/

Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You may not modify the values in the list's nodes, only nodes itself may be changed.

Как: решение в 3 этапа. 
1. Сначала находим середину листа. Если колич-во узлов четное и в середине 2 узла - второй из них. Находим по методу "кролика и черепахи"
Кролик бежит по узлам в 2 раза быстрее, поэтому, когда он достигнет конца списка, черепаха будет в середине.
2. Разворачиваем вторую половину листа
3. Сливаем полученные листы в духе сортировки слиянием

```
class Solution {
public:
	void reorderList(ListNode* head) {
		if (!head)
            return;
        
        ListNode* slow = head; ListNode* fast = head;
        while (fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;
        }
        
        ListNode* cur = slow->next;
        ListNode* prev = nullptr;
        while (cur)
        {
            ListNode* tmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = tmp;
        }
        
        slow->next = nullptr;
        ListNode* head2 = prev;
        
        while(head2)
        {
            ListNode* tmp = head->next;
            head->next = head2;
            head = head2;
            head2 = tmp;
        }
	}
};
```

# Sort list
https://leetcode.com/problems/sort-list/
Как: 
Рекурсивно:
1) находим середину
2) сливаем 2 подмассива, проверяя  условие l1->val <= l2->val

```
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode *cur=new ListNode(0);
        ListNode *temp=cur;
        while(l1 && l2) {
            if(l1->val < l2->val) {
                cur->next=l1;
                l1=l1->next;
            }
            else {
                cur->next=l2;
                l2=l2->next;
            }
            cur=cur->next;
        }
        if(l1) cur->next=l1;
        if(l2) cur->next=l2;
        return temp->next;
    }
    
    ListNode* sortList(ListNode* head) {
        // Base Case if the list has 0 or 1 element it means it is already sorted
        if(!head || !head->next) return head;
        ListNode* slow=head;
        ListNode* fast=head->next;
        while(fast && fast->next) {
            slow=slow->next;
            fast=fast->next->next;
        }
        // Divide the list into two parts (one start with head and other with fast)
        fast=slow->next;
        slow->next=NULL;
        // Merge these List 
        return merge(sortList(head), sortList(fast));
    }
};
```
Time: O(NlogN)
Space: O(1)

