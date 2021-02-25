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
Как: We notice that the problem could be simply reduced to another one : Remove the (L - n + 1)(L−n+1) th node from the beginning in the list , where LL is the list length. This problem is easy to solve once we found list length L
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
        ListNode* dummy = new ListNode(0, head); // для случая [1] и надо удалить единственный узел
        ListNode* first = dummy;
        ListNode* second = dummy;
        int i = 0;
        while (second->next){
            if (i < n){
                i++;
                second = second->next;
            }
            else
            {
                first = first->next;
                second = second->next;
            }
        } 
        first->next = first->next->next;
        
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
        vector<ListNode*> A = {head};
        while (A.back()->next != NULL)
            A.push_back(A.back()->next);
        return A[A.size() / 2];
    }
};
```
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
		if ( ! head ) return;
		ListNode *slow = head, *fast = head;
		while ( fast->next && fast->next->next )
		{
			slow = slow->next;
			fast = fast->next->next;
		}
			
		
		ListNode *prev = NULL, *cur = slow->next, *save;
		while ( cur )
		{
			save = cur->next;
			cur->next = prev;
			prev = cur;
			cur = save;
		}
			
		slow->next = NULL;
		
		ListNode *head2 = prev;
		while ( head2 )
		{
			save = head->next;
			head->next = head2;
			head = head2;
			head2 = save;
		}      
	}
};
```
