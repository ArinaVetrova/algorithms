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
Как: один узел шагает медленно -  смотрит на след узел, а второй бежит вперед и смотрит через узел, если в списке есть цикл, черепаха и кролик сравняются в одной из точек.

```
class Solution {
public:
ListNode *detectCycle(ListNode *head) {
	ListNode* slow = head;
	ListNode* fast = head;
	while(fast && fast->next) {
		slow = slow->next;
		fast = fast->next->next;
		if (slow == fast) {
			slow = head;
			while (slow != fast) {
				slow = slow->next;
				fast = fast->next;
			}
			return slow;
		}
	}
	return nullptr;
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
```
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* mid = getMid(head);
        ListNode* left = sortList(head);
        ListNode* right = sortList(mid);
        return merge(left, right);
    }

    ListNode* merge(ListNode* list1, ListNode* list2) {
        ListNode dummyHead(0);
        ListNode* ptr = &dummyHead;
        while (list1 && list2) {
            if (list1->val < list2->val) {
                ptr->next = list1;
                list1 = list1->next;
            } else {
                ptr->next = list2;
                list2 = list2->next;
            }
            ptr = ptr->next;
        }
        if(list1) ptr->next = list1;
        else ptr->next = list2;

        return dummyHead.next;
    }

    ListNode* getMid(ListNode* head) {
        ListNode* midPrev = nullptr;
        while (head && head->next) {
            midPrev = (midPrev == nullptr) ? head : midPrev->next;
            head = head->next->next;
        }
        ListNode* mid = midPrev->next;
        midPrev->next = nullptr;
        return mid;
    }
};
```
Time: O(NlogN)
Space: O(1)

```
class Solution {
public:
    ListNode* tail = new ListNode();
    ListNode* nextSubList = new ListNode();

    ListNode* sortList(ListNode* head) {
        if (!head || !head -> next)
            return head;
        int n = getCount(head);
        ListNode* start = head;
        ListNode dummyHead(0);
        for (int size = 1; size < n; size = size * 2) {
            tail = &dummyHead;
            while (start) {
                if (!start->next) {
                    tail->next = start;
                    break;
                }
                ListNode* mid = split(start, size);
                merge(start, mid);
                start = nextSubList;
            }
            start = dummyHead.next;
        }
        return dummyHead.next;
    }

    ListNode* split(ListNode* start, int size) {
        ListNode* midPrev = start;
        ListNode* end = start->next;
        //use fast and slow approach to find middle and end of second linked list
        for (int index = 1; index < size && (midPrev->next || end->next); index++) {
            if (end->next) {
                end = (end->next->next) ? end->next->next : end->next;
            }
            if (midPrev->next) {
                midPrev = midPrev->next;
            }
        }
        ListNode* mid = midPrev->next;
        nextSubList = end->next;
        midPrev->next = nullptr;
        end->next = nullptr;
        // return the start of second linked list
        return mid;
    }

    void merge(ListNode* list1, ListNode* list2) {
        ListNode dummyHead(0);
        ListNode* newTail = &dummyHead;
        while (list1 && list2) {
            if (list1->val < list2->val) {
                newTail->next = list1;
                list1 = list1->next;
                newTail = newTail->next;
            } else {
                newTail->next = list2;
                list2 = list2->next;
                newTail = newTail->next;
            }
        }
        newTail->next = (list1) ? list1 : list2;
        // traverse till the end of merged list to get the newTail
        while (newTail->next) {
            newTail = newTail->next;
        }
        // link the old tail with the head of merged list
        tail->next = dummyHead.next;
        // update the old tail with the new tail of merged list
        tail = newTail;
    }

    int getCount(ListNode* head) {
        int cnt = 0;
        ListNode* ptr = head;
        while (ptr) {
            ptr = ptr->next;
            cnt++;
        }
        return cnt;
    }
};
```
Time: O(NlogN)
Space: O(1)
