好的，下面是用 Python 和 C 语言实现的快速排序代码。

**核心思想：**

快速排序是一种分治算法。它的基本思想是：

1.  **选择基准（Pivot）：** 从数组中选择一个元素作为“基准”。
2.  **分区（Partition）：** 将数组重新排列，使得所有小于基准的元素都排在基准的左边，所有大于基准的元素都排在基准的右边。等于基准的元素可以放在任何一边。这个过程称为分区。分区完成后，基准就处于它在排序后数组中的最终位置。
3.  **递归排序（Recursion）：** 递归地对基准左边的子数组和右边的子数组进行快速排序。

**Python 实现：**

```python
def quick_sort(arr):
    """
    使用快速排序算法对列表进行排序。

    Args:
        arr: 需要排序的列表。

    Returns:
        排序后的列表。
    """
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]  # 选择中间元素作为基准
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]

    return quick_sort(left) + middle + quick_sort(right)

# 示例用法
if __name__ == "__main__":
    my_list = [3, 6, 8, 10, 1, 2, 1]
    sorted_list = quick_sort(my_list)
    print(f"原始列表: {my_list}")
    print(f"排序后的列表: {sorted_list}")

    my_list_2 = [99, -5, 0, 100, 23, 45, -10, 88]
    sorted_list_2 = quick_sort(my_list_2)
    print(f"原始列表: {my_list_2}")
    print(f"排序后的列表: {sorted_list_2}")
```

**Python 实现说明：**

*   **`quick_sort(arr)` 函数：**
    *   **Base Case:** 如果列表长度小于等于 1，则列表已经排序，直接返回。
    *   **Pivot Selection:** 这里选择列表的中间元素作为基准。有多种选择基准的方法（第一个元素、最后一个元素、随机元素、中位数等），选择中间元素通常是一个不错的折中。
    *   **List Comprehensions:** 使用列表推导式 `[x for x in arr if condition]` 高效地创建三个子列表：`left` (小于基准的元素)、`middle` (等于基准的元素) 和 `right` (大于基准的元素)。
    *   **Recursive Calls:** 递归地调用 `quick_sort` 对 `left` 和 `right` 子列表进行排序。
    *   **Concatenation:** 将排序后的 `left` 子列表、`middle` 子列表和排序后的 `right` 子列表连接起来，形成最终的排序列表。

**C 语言实现：**

C 语言的实现通常是在原地进行排序，这意味着它直接修改原始数组，而不是创建新的数组。这通常更有效率。

```c
#include <stdio.h>

// 交换两个整数的辅助函数
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// 分区函数：选择一个基准元素，将数组分成两部分：
// 小于基准的在左边，大于基准的在右边。
// 返回基准元素的最终位置。
int partition(int arr[], int low, int high) {
    int pivot = arr[high]; // 选择最后一个元素作为基准
    int i = (low - 1);     // i 是小于基准的最后一个元素的索引

    for (int j = low; j <= high - 1; j++) {
        // 如果当前元素小于或等于基准
        if (arr[j] <= pivot) {
            i++; // 增加小于基准的元素的数量
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]); // 将基准放到正确的位置
    return (i + 1);
}

// 快速排序主函数
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        // pi 是分区索引，arr[pi] 已经在正确的位置
        int pi = partition(arr, low, high);

        // 分别对分区左右两边的子数组进行排序
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// 打印数组的辅助函数
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// 驱动程序
int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("原始数组: \n");
    printArray(arr, n);

    quickSort(arr, 0, n - 1);

    printf("排序后的数组: \n");
    printArray(arr, n);

    int arr2[] = {3, 6, 8, 10, 1, 2, 1};
    int n2 = sizeof(arr2) / sizeof(arr2[0]);
    printf("原始数组: \n");
    printArray(arr2, n2);
    quickSort(arr2, 0, n2 - 1);
    printf("排序后的数组: \n");
    printArray(arr2, n2);

    return 0;
}
```

**C 语言实现说明：**

*   **`swap(int* a, int* b)` 函数：** 一个简单的辅助函数，用于交换两个整数的内存地址指向的值。
*   **`partition(int arr[], int low, int high)` 函数：**
    *   **Pivot Selection:** 这里选择 `arr[high]` (数组的最后一个元素) 作为基准。
    *   **`i` 变量：** `i` 跟踪小于基准的元素的最后一个位置。
    *   **Iteration:** 循环遍历从 `low` 到 `high - 1` 的元素。
    *   **Comparison and Swap:** 如果当前元素 `arr[j]` 小于或等于基准，则 `i` 递增，然后 `arr[i]` 和 `arr[j]` 交换。这会将小于基准的元素移动到数组的左侧。
    *   **Final Pivot Placement:** 循环结束后，`arr[i + 1]` 和 `arr[high]` (即基准) 交换。这样，基准就被放置在了它在排序数组中的正确位置。
    *   **Return Pivot Index:** 返回基准的最终索引 `i + 1`。
*   **`quickSort(int arr[], int low, int high)` 函数：**
    *   **Base Case:** 如果 `low < high`，则表示子数组至少有两个元素，需要排序。否则，子数组已经排序。
    *   **Partitioning:** 调用 `partition` 函数来分区数组，并获取基准的索引 `pi`。
    *   **Recursive Calls:** 递归地调用 `quickSort` 分别对基准左边的子数组 (`low` 到 `pi - 1`) 和右边的子数组 (`pi + 1` 到 `high`) 进行排序。
*   **`printArray(int arr[], int size)` 函数：** 一个辅助函数，用于打印数组的内容。
*   **`main()` 函数：** 演示了如何使用 `quickSort` 函数对数组进行排序，并打印排序前后的结果。

**性能特点：**

*   **平均时间复杂度：** O(n log n)
*   **最坏时间复杂度：** O(n^2) (当数组已经有序或逆序，并且每次都选择第一个或最后一个元素作为基准时发生)
*   **空间复杂度：** O(log n) (平均情况下，由于递归调用栈的深度) 或 O(n) (最坏情况下)

**选择基准的策略对性能有很大影响。** 常见的优化包括：

*   **随机选择基准：** 降低了最坏情况发生的概率。
*   **三数取中法：** 选择第一个、中间和最后一个元素的中间值作为基准，这通常比选择第一个或最后一个元素效果更好。

希望这些代码和解释能帮助你理解快速排序！
