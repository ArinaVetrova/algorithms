# Linked list

# Reverse linked list
https://leetcode.com/problems/reverse-linked-list/

- Что сделать: реверсировать односвязный список

- Как: создаем 2 доп указателя - предыдущий и следующий. Запоминаем следующий --> у head перенаправляем указатель next на предыдущий 
--> предыдущий == head --> head == следущий, который запомнили ранее. 

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

Какие делала ошибки при попытке самостоят решения:
- получались закольцованные указатели ( не использовала доп. указатели для хранения временных величин)
- использовала head++, была ошибка

