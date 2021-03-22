# Грибной модуль

###  Метод «квадратичної вибірки»
???

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
