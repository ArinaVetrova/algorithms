https://academy.yandex.ru/posts/osnovnye-vidy-sortirovok-i-primery-ikh-realizatsii

Где можно потренировать
https://leetcode.com/problems/sort-an-array/

# Пузырек:
```
void BubbleSort(vector<int>& values) {
  for (size_t idx_i = 0; idx_i + 1 < values.size(); ++idx_i) {
    for (size_t idx_j = 0; idx_j + 1 < values.size() - idx_i; ++idx_j) {
      if (values[idx_j + 1] < values[idx_j]) {
        swap(values[idx_j], values[idx_j + 1]);
      }
    }
  }
}
```
# Выбором
```
void SelectionSort(vector<int>& values) {
  for (auto i = values.begin(); i != values.end(); ++i) {
    auto j = std::min_element(i, values.end());
    swap(*i, *j);
  }
}
```
 # Вставками 
 ```
 void InsertionSort(vector<int>& values) {
  for (size_t i = 1; i < values.size(); ++i) {
    int x = values[i];
    size_t j = i;
    while (j > 0 && values[j - 1] > x) {
      values[j] = values[j - 1];
      --j;
    }
    values[j] = x;
  }
}
 ```
 
 # Быстрая
Статья с норм реализацией (у яндекса что-то непонятное)  https://acmp.ru/asp/do/index.asp?main=topic&id_course=1&id_section=7&id_topic=119
 ```
class Solution {
public:
    //merge sort
    
    void QuickSort_In(vector<int>& val, int l, int r)
    {
        if(l<r){
        int pivot_idx = (l+r)/2;
        int x = val[pivot_idx];
        int i = l, j = r;
        while(i <= j){
            while(val[i] < x) 
                i++;
            while(val[j] > x) 
                j--;
            if(i<=j) 
                swap(val[i++],val[j--]);
        }
            QuickSort_In(val, l, j);
            QuickSort_In(val, i, r);
     }
    }
    
    
    void quickSort(vector<int>& val)
    {
        if (val.size()<=1)
            return;
        QuickSort_In(val, 0, val.size()-1);
    }
    
    vector<int> sortArray(vector<int>& values) {
        quickSort(values);
        return values;
    }
    
};
 ```
 
 # Слиянием
 ```
 void MergeSortImpl(vector<int>& values, vector<int>& buffer, int l, int r) {
  if (l < r) {
    int m = (l + r) / 2;
    MergeSortImpl(values, buffer, l, m);
    MergeSortImpl(values, buffer, m + 1, r);

    int k = l;
    for (int i = l, j = m + 1; i <= m || j <= r; ) {
      if (j > r || (i <= m && values[i] < values[j])) 
        buffer[k++] = values[i++];
      else 
        buffer[k++] = values[j++];
      }
    for (int i = l; i <= r; ++i) {
      values[i] = buffer[i];
    }
  }
}

void MergeSort(vector<int>& values) {
  if (!values.empty()) {
    vector<int> buffer(values.size());
    MergeSortImpl(values, buffer, 0, values.size() - 1);
  }
}
 ```
