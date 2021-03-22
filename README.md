# Грибной модуль

###  Метод «квадратичної вибірки»
```
#include <iostream>
#include <vector>

using namespace std;

vector<int> tetragonSort(vector<int> arr) {
    int min = 0, i = 0;
    int nGroups = static_cast<int>(sqrt(arr.size()));             // Размерность одного подмассива
    vector<int> bufferList, resultList;
    if (nGroups * nGroups < arr.size()) { nGroups++; }

    bufferList.assign(nGroups, INT_MAX);                          // Заполняем буферный массив максимальными значениями
    for (int i = nGroups * min; i < arr.size(); i += nGroups) {    // Цикл заполнения подмассивов буферного массива
        min = i;                                                  // Индекс минимального элемента
        for (int j = i + 1; j < i + nGroups && j < arr.size(); j++) {
            if (arr[j] < arr[min])
                min = j;                                          // Запоминаем индекс минимального элемента
        }

        bufferList[i / nGroups] = arr[min];                       // Добавление элемента в подсписок буферного списка
        arr[min] = INT_MAX;                                       // Выделение ячейки в основном массиве
    }

    while (true) {                                        // Цикл формирования ответа
        min = 0;                                          
        for (int k = 1; k < bufferList.size(); k++) {     
            if (bufferList[k] < bufferList[min]) { min = k; }
        }
        resultList.push_back(bufferList[min]);            // Заполнение результирующего массива
        if (resultList.size() == arr.size()) { break; }   // break-point - массив отсортирован

        i = nGroups * min;                                // Точка начального просмотра
        min = i;
        for (int j = i + 1; j < i + nGroups && j < arr.size(); j++) {
            if (arr[j] < arr[min]) { min = j; }
        }

        bufferList[i / nGroups] = arr[min];               // Добавление элемента в подсписок буферного списка
        arr[min] = INT_MAX;                               // Выделение ячейки в основном массиве
    }
    return resultList;
}

int main() {
        vector<int> varr = { 6, 5, 3, 7, 8, 1, 9 };  // test_1 {6, 5, 3, 7, 8, 1, 9}			   => {1, 3, 5, 6, 7, 8, 9}
                                                     // test_2 {4, 7, 1, 12, 89, 4, 6, 11, 44, 9 } => {1, 4, 4, 6, 7, 9, 11, 12, 44, 89}
        varr = tetragonSort(varr);
        for (const auto& i : varr) { cout << i << " "; } 
        cout << endl;
    return 0;
}
```

### Сортування «злиттям» не рекурсивним алгоритмом

```
void merge(int* arr, int start, int middle, int end) {
    int firstLength = middle - start + 1;
    int* leftArr = new int[firstLength];

    for (int j = 0, i = start; j < firstLength; ++j, ++i)
        leftArr[j] = arr[i];

    int secondLength = end - middle;
    int* rightArr = new int[secondLength];
    for (int j = 0, i = middle + 1; j < secondLength; ++j, ++i)
        rightArr[j] = arr[i];

    int lIndex = 0, rIndex = 0;
    while (lIndex < firstLength && rIndex < secondLength) {
        if (leftArr[lIndex] < rightArr[rIndex])
            arr[start] = leftArr[lIndex++];
        else
            arr[start] = rightArr[rIndex++];
        start++;
    }
    while (lIndex < firstLength)
        arr[start++] = leftArr[lIndex++];
    while (rIndex < secondLength)
        arr[start++] = rightArr[rIndex++];
    delete[] leftArr;
    delete[] rightArr;
}

void mergeSortHelper(int* arr, int sortFrom, int sortTo) {
    if (sortFrom < sortTo) {
        int middle = (sortFrom + sortTo) / 2;
        mergeSortHelper(arr, sortFrom, middle);
        mergeSortHelper(arr, middle + 1, sortTo);
        merge(arr, sortFrom, middle, sortTo);
    }
}

void mergeSort(int* arr, int size) {
    mergeSortHelper(arr, 0, size - 1);
}
```

### «Цифрове» сортування

```
#include <iostream>
using namespace std;
int getMax(int arr[], int n)
{ 
    int mx = arr[0]; 
    for (int i = 1; i < n; i++) 
        if (arr[i] > mx) 
            mx = arr[i]; 
    return mx; 
}
// A function to do counting sort of arr[] according to 
// the digit represented by exp. 
void countSort(int arr[], int n, int exp) 
{ 
    int output[n]; // output array 
    int i, count[10] = { 0 };
    // Store count of occurrences in count[] 
    for (i = 0; i < n; i++) 
        count[(arr[i] / exp) % 10]++;
    // Change count[i] so that count[i] now contains actual 
    //  position of this digit in output[] 
    for (i = 1; i < 10; i++) 
        count[i] += count[i - 1];
    // Build the output array 
    for (i = n - 1; i >= 0; i--) { 
        output[count[(arr[i] / exp) % 10] - 1] = arr[i]; 
        count[(arr[i] / exp) % 10]--; 
    }
    // Copy the output array to arr[], so that arr[] now 
    // contains sorted numbers according to current digit 
    for (i = 0; i < n; i++) 
        arr[i] = output[i]; 
}
void radixsort(int arr[], int n) 
{ 
    // Find the maximum number to know number of digits 
    int m = getMax(arr, n);
    // Do counting sort for every digit. Note that instead 
    // of passing digit number, exp is passed. exp is 10^i 
    // where i is current digit number 
    for (int exp = 1; m / exp > 0; exp *= 10) 
        countSort(arr, n, exp); 
}
// A utility function to print an array 
void print(int arr[], int n) 
{ 
    for (int i = 0; i < n; i++) 
        cout << arr[i] << " "; 
}
int main() 
{ 
    int arr[] = { 170, 45, 75, 90, 802, 24, 2, 66 }; 
    int n = sizeof(arr) / sizeof(arr[0]);
    radixsort(arr, n);
    print(arr, n); 
    return 0; 
}
```

### «Бітове» сортування
```
#include <iostream>
using namespace std;

void bitsort(int theArray[],int s) {

int* bucket[2];

int bSize[2];

bucket[0] = new int[s];
bucket[1] = new int[s];

int divisor = 1;

for (int i=1; i<32; i++) {

    bSize[0] = 0;
    bSize[1] = 0;

    for (int j = 0; j < s; j++) {

        int t = theArray[j] / divisor % 2;
        bucket[t][bSize[t]] = theArray[j];
        bSize[t]++;

    }

    int c = 0;

    for (int k = 0; k < bSize[0]; k++) theArray[c++] = bucket[0][k];
    for (int k = 0; k < bSize[1]; k++) theArray[c++] = bucket[1][k];

    divisor *= 2;
}

delete[] bucket[0];
delete[] bucket[1];

}

int main() {

    int array[] = {348756, 223756, 291834, 2389, 346, 8674, 373456, 42356, 23, 48635 };
    cout<<"List before bitsort: "<<endl;
    for (int i=0; i<10; i++) cout<<array[i]<<' ';
    cout<<endl;
    bitsort(array,10);
    cout<<"List after bitsort: "<<endl;
    for (int i=0; i<10; i++) cout<<array[i]<<' ';
    cout<<endl;
    return 0;
}
```

### «Лексикографічне» сортування к-кортежів
???

### «Лексикографічне» сортування ланцюжків різної довжини
```
#include <iostream>
using namespace std;

void LexicSort (string *str, int n) {
    string temp;
    for(int i = 0; i < n-1; ++i)
        for( int j = i+1; j < n; ++j)
        {
            if(str[i] > str[j])
            {
                temp = str[i];
                str[i] = str[j];
                str[j] = temp;
            }
        }
}

int main()
{
    int n;
    cout<<"How many Strings u want to Sort:  ";
    cin>>n;
    string str[n], temp;

    cout << "\nEnter [ "<<n<<" ] Strings Below: " << endl;

    for(int i = 0; i < n; i++)
    {
        cout<<"\nEnter [ "<<i+1<<" ] String: ";
        cin>>str[i];
    }

    LexicSort(str,n);

    cout << "\nAfter Sorting [ "<<n<<" ] Strings in lexicographical order: \n" << endl;

    for(int i = 0; i < n; ++i)
    {
        cout << str[i] << endl;
    }

    return 0;
}
```

### «Швидке» сортування не рекурсивним алгоритмом
```
void swap(int*, int, int);
int partition(int*, int, int);

// 1 2 3 4 5

void quickSort(int* arr, int start, int end) {
    int* stack = new int[end - start + 1];

    int stackTop = -1;

    // fill stack with initial value;
    stack[++stackTop] = end; // end index
    stack[++stackTop] = 0;  // start index

    while (stackTop >= 0) {
        start = stack[stackTop--];
        end = stack[stackTop--];

        int pivotIndex = partition(arr, start, end);

        // sort left part of an array
        if (start < pivotIndex - 1) {
            stack[++stackTop] = pivotIndex - 1;
            stack[++stackTop] = start;
        }

        // sort right part of an array
        if (end > pivotIndex + 1) {
            stack[++stackTop] = end;
            stack[++stackTop] = pivotIndex + 1;
        }
    }
}

int partition(int* arr, int low, int high) {
    int pivotElement = arr[high];

    int left = low - 1;

    for (int right = low; right <= high - 1; right++) {
        if (arr[right] <= pivotElement) {
            left++;
            swap(arr, left, right);
        }
    }

    swap(arr, left + 1, high);
    return left + 1;
}

void swap(int* arr, int i1, int i2) {
    int temp = arr[i1];
    arr[i1] = arr[i2];
    arr[i2] = temp;
}
```

### «Бінарне» сортування (різновид «швидкого»  v=(min + max)/2)
???

###  Сортування «бінарним деревом»
```
#include <vector>
#include <iostream>

using namespace std;

struct Tree {									// Структура бинарного дерева
	int data;
	Tree* left;
	Tree* right;
};

Tree* addTree(Tree* tree, const float& d) {		// Добавление элемента в дерево
	if (tree == nullptr) {
		tree = new Tree;
		tree->data = d;
		tree->left = tree->right = nullptr;
	}
	else if (d < tree->data) { tree->left = addTree(tree->left, d); }
	else tree->right = addTree(tree->right, d);
	return tree;
}

void deleteTree(Tree* tree) {					// Удаление дерева
	if (tree != nullptr) {
		deleteTree(tree->left);
		deleteTree(tree->right);
		delete tree;
	}
}

vector<int> sortTree(Tree* tree, vector<int> sorted_tree){		// Сортировка бинарным деревом
if (tree != nullptr) {
	sorted_tree = sortTree(tree->left, sorted_tree);
	sorted_tree.push_back(tree->data);							// Внесение элементов в результирующий массив
	sorted_tree = sortTree(tree->right, sorted_tree);
}
return sorted_tree;
}

int main() {
	int arr[] = { 6, 5, 3, 7, 8, 1, 9 };  // test_1 {6, 5, 3, 7, 8, 1, 9}				=> {1, 3, 5, 6, 7, 8, 9}
//                                        // test_2 {4, 7, 1, 12, 89, 4, 6, 11, 44, 9 } => {1, 4, 4, 6, 7, 9, 11, 12, 44, 89}
	Tree* tree = nullptr;
	vector<int> sv;

	for (int i = 0; i < size(arr); ++i) {
		tree = addTree(tree, arr[i]);
	}
	sv = sortTree(tree, sv);

	for (const int& i : sv) {
		cout << i << " ";
	}
	deleteTree(tree);
	return 0;
}
```

### Сортування «тернарним деревом»
```
#include <iostream>

#include <vector>

using namespace std;

struct Tree {                  // Структура бинарного дерева
        int data;
    Tree* left;
    Tree* middle;
    Tree* right;
};

Tree* addTree(Tree* tree, const float& d) {    // Добавление элемента в дерево
    if (tree == nullptr) {
        tree = new Tree;
        tree->data = d;
        tree->left = tree->middle = tree->right = nullptr;
    }
    else if (d < tree->data) { tree->left = addTree(tree->left, d); }
    else if (d == tree->data) { tree->middle = addTree(tree->middle, d); }
    else tree->right = addTree(tree->right, d);
    return tree;
}

void deleteTree(Tree* tree) {          // Удаление дерева
    if (tree != nullptr) {
        deleteTree(tree->left);
        deleteTree(tree->middle);
        deleteTree(tree->right);
        delete tree;
    }
}

vector<int> sortTree(Tree* tree, vector<int> sorted_tree){    // Сортировка бинарным деревом
    if (tree != nullptr) {
        sorted_tree = sortTree(tree->left, sorted_tree);
        sorted_tree.push_back(tree->data);
        sorted_tree = sortTree(tree->middle, sorted_tree);
        sorted_tree.push_back(tree->data);  // Внесение элементов в результирующий массив
        sorted_tree = sortTree(tree->right, sorted_tree);
    }
    return sorted_tree;
}

int main() {
    int arr[] = { 6, 5, 3, 7, 8, 1, 9 };  // test_1 {6, 5, 3, 7, 8, 1, 9}        => {1, 3, 5, 6, 7, 8, 9}
//                                        // test_2 {4, 7, 1, 12, 89, 4, 6, 11, 44, 9 } => {1, 4, 4, 6, 7, 9, 11, 12, 44, 89}
    Tree* tree = nullptr;
    vector<int> sv;

    for (int i = 0; i < size(arr); ++i) {
        tree = addTree(tree, arr[i]);
    }
    sv = sortTree(tree, sv);

    for (const int& i : sv) {
        cout << i << " ";
    }
    deleteTree(tree);
    return 0;
}
```

### Наступний метод: спочатку послідовність складається з одного елемента. В послідовність здійснюємо додавання, використовуючи «двійковий пошук» місця. Запропонувати відповідну структуру для швидкого бінарного пошуку та вставки. Бажана часова складність O(log n)
???

###  Методи «зовнішнього» сортування («сортування файлів»)
???

