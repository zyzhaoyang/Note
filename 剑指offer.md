剑指Offer

## 1.1 数组

### 1.1.1 二维数组中的查找

- 题目描述
  - 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
- 问题思路
  - 首先选取数组中右上角的数字。该数字等于目标数字，则找到;该数字大于目标数字，则这个数字所在的列不存在目标数字，即列值--；该数字小于目标数字，则剔除这个数字所在的行，即行值++；要查找的数字不在数组的右上角，则每一次都在数组的查找范围中剔除一行或者一列，这样每一步都可以缩小查找的范围，直到找到要查找的数字，或者查找范围为空。
- 编程实现

```C++
    bool Find(int target, vector<vector<int> > array) {
        if(array.size() <= 0 || array[0].size() <= 0)
            return false;
        int arr_row = array.size() - 1;
        int arr_col = array[0].size() - 1;
        
        if(target < array[0][0] || target > array[arr_row][arr_col])
            return false;
        int i = 0;
        int j = arr_col;
        while(i <= arr_row && j >= 0)
        {
            int index = array[i][j];
            if(index == target)
            {
                return true;
            }
            else if(index > target)
            {
                j--;
            }
            else
            {
                i++;
            }
        }
        
        return false;
    }
```

### 1.1.2 旋转数组的最小数字

- 问题描述
  - 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
- 问题思路
  - 用二分查找法的思路来寻找这个最小的元素。
  - 找到数组中间的元素,如果中间元素位于前面的递增子数组，那么它应该大于或者等于第一个指针指向的元素。此时最小元素应该位于该中间元素之后，然后我们把第一个指针指向该中间元素，移动之后第一个指针仍然位于前面的递增子数组中。
  - 如果中间元素位于后面的递增子数组，那么它应该小于或者等于第二个指针指向的元素。此时最小元素应该位于该中间元素之前，然后我们把第二个指针指向该中间元素，移动之后第二个指针仍然位于后面的递增子数组中。
  - 第一个指针总是指向前面递增数组的元素，第二个指针总是指向后面递增数组的元素。最终它们会指向两个相邻的元素，而第二个指针指向的刚好是最小的元素，这就是循环结束的条件。
- 编程实现

```c++
    int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.size() <= 0)
            return 0;
        
        if(rotateArray.size() == 1)
            return rotateArray[0];
        
        if(rotateArray.size() == 2)
            return rotateArray[0] > rotateArray[1] ? rotateArray[1] : rotateArray[0];
        
        int left = 0;
        int right = rotateArray.size() - 1;
        int mid = left;
        while (rotateArray [left] >= rotateArray[right]){//左右指针相遇时停止循环
            if(right - left == 1){
                return rotateArray[right];
            }
            mid = left + (right - left) / 2;
            if(rotateArray[left] == rotateArray[right] && rotateArray[mid] == rotateArray[left]){//解决特殊情况
                int result = rotateArray[left];
                for(int i = left + 1; i < right; i++){
                    if (rotateArray[i] < result)
                        result = rotateArray[i];
                }
                return result;
            }
            if(rotateArray[mid] >= rotateArray[left])
                left = mid;
            else
                right = mid;
        }
        return rotateArray[mid];
    }
```

### 1.1.3 调整数组顺序使奇数位于偶数前面

- 问题描述
  - 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
- 问题思路
  - 因为要保证奇数和偶数的相对位置保持不变，所以使用一个数组保存奇数和偶数的值，先遍历一遍，按顺序保存奇数，然后再遍历保存偶数即可。
  - 如果不要求保证相对位置，则使用两个指针，一个p1指向数组头，一个p2指向数组尾，若p1和p2都是奇数，则p1++；若p1和p2都是偶数，则p2--；若p1奇数，p2偶数，则p1++，p2--；若p1是偶数，p2是奇数，则交换这两个数字。
- 编程实现

```c++
    void reOrderArray(vector<int> &array) {
        if(array.size() <= 0)
            return;
        int num = array.size();
        vector<int> v;
        for(int i = 0; i < num; i++) {
            if(array[i] % 2 == 1)
                v.push_back(array[i]);
        }
        
        for(int i = 0; i < num; i++) {
            if(array[i] % 2 == 0)
                v.push_back(array[i]);
        }
        
        for(int i = 0; i < num; i++)
            array[i] = v[i];
    }
```

### 1.1.4 数组中出现次数超过一半的数字

- 问题描述
  - 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
  - 例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。
- 问题思路
  - 数组中有一个数字出现的次数超过数组长度的一半，也就是说它出现的次数比其他所有数字出现次数的和还要多。因此我们可以考虑在遍历数组的时候保存两个值：一个是数组的一个数字，一个是次数。当我们遍历到下一个数字的时候，如果下一个数字和我们之前保存的数字相同，则次数加1；如果下一个数字和我们之前保存的数字不同，则次数减1。如果次数为零，我们需要保存下一个数字，并把次数设为1。由于我们要找的数字出现的次数比其他所有数字出现的次数之和还要多，那么要找的数字肯定是最后一次把次数设为1时对应的数字。
- 编程实现

```
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.size() <= 0)
            return 0;
        
        int len = numbers.size();
        int index = numbers[0];
        int count = 1;
        for(int i = 1; i < len; i++) {
            if(count == 0) {
                index = numbers[i];
                count = 1;
            }
            else if(index == numbers[i])
                count++;
            else
                count--;
        }
        count = 0;
        for(int i = 0; i < len; i++) {
            if(index == numbers[i])
                count++;
        }
        
        return (count > len / 2) ? index : 0;
    }
```

### 1.1.5 连续子数组的最大和

- 问题描述
  - 输入一个整型数组，数组中有正数也有负数，求所有子数组的和的最大值。
  - 要求时间复杂度为O(N)
- 问题思路
  - 累加的子数组和，如果大于零，那么我们继续累加就行；否则，则需要剔除原来的累加和重新开始。
- 编程实现

```c++
    int FindGreatestSumOfSubArray(vector<int> array) {
        int len = array.size();
        int res = INT_MIN;
        int tmp_sum = 0;
        
        for(int i = 0; i < len; i++) {
            if(tmp_sum <= 0)
                tmp_sum = array[i];
            else
                tmp_sum += array[i];
            res = max(res, tmp_sum);
        }
        return res;
    }
```

### 1.1.6 把数组排成最小的数

- 问题描述
  - 输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
  - 例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
- 问题思路
  - 对拼接后的字符串进行比较。
  - 排序规则如下：
  - 若ab > ba 则 a 大于 b，
  - 若ab < ba 则 a 小于 b，
  - 若ab = ba 则 a 等于 b；
  - 根据上述规则，先将数字转换成字符串再进行比较，因为需要串起来进行比较。比较完之后，按顺序输出即可。
- 编程实现

```C++
    static bool cmp(int a, int b) {
        string A = to_string(a) + to_string(b);
        string B = to_string(b) + to_string(a);
        return A < B;
    }
    
    string PrintMinNumber(vector<int> numbers) {
        if(numbers.size() <= 0)
            return "";
        
        sort(numbers.begin(), numbers.end(), cmp);
        string res;
        for(int i = 0; i < numbers.size(); i++){
            res += to_string(numbers[i]);
        }
        return res;
    }
```

### 1.1.7 数组中的逆序对

- 问题描述
  - 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007
  - 输入描述:题目保证输入的数组中没有的相同的数字
  - 数据范围：
  - 对于%50的数据,size<=10^4
  - 对于%75的数据,size<=10^5
  - 对于%100的数据,size<=2*10^5
- 问题思路
  - 这是一个归并排序的合并过程，主要是考虑合并两个有序序列时，计算逆序对数。
  - 对于两个升序序列，设置两个下标：两个有序序列的末尾。每次比较两个末尾值，如果前末尾大于后末尾值，则有”后序列当前长度“个逆序对；否则不构成逆序对。然后把较大值拷贝到辅助数组的末尾，即最终要将两个有序序列合并到辅助数组并有序。
  - 这样，每次在合并前，先递归地处理左半段、右半段，则左、右半段有序，且左右半段的逆序对数可得到，再计算左右半段合并时逆序对的个数。
  - 
  - 注意：InversePairsCore形参的顺序是(data,copy)，而递归调用时实参是(copy,data)。递归函数InversePairsCore的作用就行了，它是对data的左右半段进行合并，复制到辅助数组copy中有序。
- 编程实现

```C++
    int InversePairs(vector<int> data) {
        if(data.size() == 0){
            return 0;
        }
        // 排序的辅助数组
        vector<int> copy;
        for(int i = 0; i < data.size(); ++i){
            copy.push_back(data[i]);
        }
        return InversePairsCore(data, copy, 0, data.size() - 1) % 1000000007;
    }
    
    long InversePairsCore(vector<int> &data, vector<int> &copy, int begin, int end){
        // 如果指向相同位置，则没有逆序对。
        if(begin == end){
            copy[begin] = data[end];
            return 0;
        }
        // 求中点
        int mid = begin + (end - begin) / 2;
        // 使data左半段有序，并返回左半段逆序对的数目
        long leftCount = InversePairsCore(copy, data, begin, mid);
        // 使data右半段有序，并返回右半段逆序对的数目
        long rightCount = InversePairsCore(copy, data, mid + 1, end);
        
        int i = mid; // i初始化为前半段最后一个数字的下标
        int j = end; // j初始化为后半段最后一个数字的下标
        int indexcopy = end; // 辅助数组复制的数组的最后一个数字的下标
        long count = 0; // 计数，逆序对的个数，注意类型
        
        while(i >= begin && j >= mid + 1){
            if(data[i] > data[j]){
                copy[indexcopy--] = data[i--];
                count += j - mid;
            }
            else{
                copy[indexcopy--] = data[j--];
            }
        }
        for(;i >= begin; --i){
            copy[indexcopy--] = data[i];
        }
        for(;j >= mid + 1; --j){
            copy[indexcopy--] = data[j];
        }
        return leftCount + rightCount + count;
    }
```

### 1.1.8 数字在排序数组中出现的次数

- 问题描述
  - 统计一个数字在排序数组中出现的次数。
- 问题思路
  - 二分查找
  - 使用二分法找到数字在数组中出现的第一个位置，再利用二分法找到数字在数组中出现的第二个位置。时间复杂度为O(logn + logn)，最终的时间复杂度为O(logn)。
  - 数组data中，数字k出现的第一个位置：我们对数组data进行二分，如果数组中间的数字小于k，说明k应该出现在中间位置的右边；如果数组中间的数字大于k，说明k应该出现在中间位置的左边；如果数组中间的数字等于k，并且中间位置的前一个数字不等于k，说明这个中间数字就是数字k出现的第一个位置。
  - 同理，数字k出现的最后一个位置，也是这样找的。但是判断少有不同。我们使用两个函数分别获得他们。
- 编程实现

```C++
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int length = data.size();
        if(length == 0){
            return 0;
        }
        int first = GetFirstK(data, k, 0, length - 1);
        int last = GetLastK(data, k, 0, length - 1);
        if(first != -1 && last != -1){
            return last - first + 1;
        }
        return 0;
    }
private:
    // 迭代实现找到第一个K
    int GetFirstK(vector<int> data, int k, int begin, int end){
        if(begin > end){
            return -1;
        }
        int middleIndex = (begin + end) >> 1;
        int middleData = data[middleIndex];
        
        if(middleData == k){
            if((middleIndex > 0 && data[middleIndex - 1] != k) || middleIndex == 0){
                return middleIndex;
            }
            else{
                end = middleIndex - 1;
            }
        }
        else if (middleData > k){
            end = middleIndex - 1;
        }
        else{
            begin = middleIndex + 1;
        }
        return GetFirstK(data, k, begin, end);
    }
    // 循环实现找到最后一个K
    int GetLastK(vector<int> data, int k, int begin, int end){
        int length = data.size();
        int middleIndex = (begin + end) >> 1;
        int middleData = data[middleIndex];
        
        while(begin <= end){
            if(middleData == k){
                if((middleIndex < length - 1 && data[middleIndex + 1] != k) || middleIndex == length - 1){
                    return middleIndex;
                }
                else{
                    begin = middleIndex + 1;
                }
            }
            else if(middleData > k){
                end = middleIndex - 1;
            }
            else{
                begin = middleIndex + 1;
            }
            middleIndex = (begin + end) >> 1;
            middleData = data[middleIndex];
        }
        return -1;
    }
};
```



### 1.1.9 数组中只出现一次的数字

- 问题描述
  - 一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。
  - 要求时间复杂度是O(n)，空间复杂度是O(1)。
- 问题思路
  - 利用异或。异或的一个性质是：**任何一个数字异或它自己都等于0**。
  - 从头到尾一次异或数组中的每一个数字，那么最终得到的结果就是两个只出现一次的数组的异或结果。
  - 由于两个数字肯定不一样，那么异或的结果肯定不为0，也就是说这个结果数组的二进制表示至少有一个位为1。
  - 我们在结果数组中找到第一个为1的位的位置，记为第n位。现在我们以第n位是不是1为标准把元数组中的数字分成两个子数组，第一个子数组中每个数字的第n位都是1，而第二个子数组中每个数字的第n位都是0。
  - 举例：{2,4,3,6,3,2,5,5}
  - 依次对数组中的每个数字做异或运行之后，得到的结果用二进制表示是0010。异或得到结果中的倒数第二位是1，于是我们根据数字的倒数第二位是不是1分为两个子数组。第一个子数组{2,3,6,3,2}中所有数字的倒数第二位都是1，而第二个子数组{4,5,5}中所有数字的倒数第二位都是0。接下来只要分别两个子数组求异或，就能找到第一个子数组中只出现一次的数字是6，而第二个子数组中只出现一次的数字是4。
- 编程实现

```C++
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        int length = data.size();
        if(length < 2){
            return;
        }
        
        // 对原始数组每个元素求异或
        int resultExclusiveOR = 0;
        for(int i = 0; i < length; ++i){
            resultExclusiveOR ^= data[i];
        }
        
        unsigned int indexOf1 = FindFirstBitIs1(resultExclusiveOR);
        
        *num1 = *num2 = 0;
        for(int j = 0; j < length; j++){
            if(IsBit1(data[j], indexOf1)){
                *num1 ^= data[j];
            }
            else{
                *num2 ^= data[j];
            }
        }
    }
private:
    // 找到二进制数num第一个为1的位数，比如0010，第一个为1的位数是2。
    unsigned int FindFirstBitIs1(int num){
        unsigned int indexBit = 0;
        // 只判断一个字节的
        while((num & 1) == 0 && (indexBit < 8 * sizeof(unsigned int))){
            num = num >> 1;
            indexBit++;
        }
        return indexBit;
    }
    // 判断第indexBit位是否为1
    bool IsBit1(int num, unsigned int indexBit){
        num = num >> indexBit;
        return (num & 1);
    }
};
```



### 1.1.10 数组中重复的数字

- 问题描述
  - 在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 
  - 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。
- 问题思路
  - 把当前序列当成是一个下标和下标对应值是相同的数组（时间复杂度为O(n),空间复杂度为O(1)）； 遍历数组，判断当前位的值和下标是否相等：
    - 若相等，则遍历下一位；
    - 若不等，则将当前位置i上的元素和a[i]位置上的元素比较：若它们相等，则找到了第一个相同的元素；若不等，则将它们两交换。换完之后a[i]位置上的值和它的下标是对应的，但i位置上的元素和下标并不一定对应；重复2的操作，直到当前位置i的值也为i，将i向后移一位，再重复2。
- 编程实现

```C++
    bool duplicate(int numbers[], int length, int* duplication) {
        // 非法输入
        if(numbers == NULL || length <= 0){
            return false;
        }
        // 非法输入
        for(int i = 0; i < length; i++){
            if(numbers[i] < 0 || numbers[i] > length - 1){
                return false;
            }
        }
        // 遍历查找第一个重复的数
        for(int i = 0; i < length; i++){
            while(numbers[i] != i){
                if(numbers[i] == numbers[numbers[i]]){
                    *duplication = numbers[i];
                    return true;
                }
                swap(numbers[i], numbers[numbers[i]]);
            }
        }
        return false;
    }
```



### 1.1.11 构建乘积数组

- 问题描述
  - 给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]A[1]...A[i-1]A[i+1]...A[n-1]。不能使用除法。
- 问题思路
- 编程实现

```C++
    vector<int> multiply(const vector<int>& A) {
        int length = A.size();
        vector<int> B(length);
        if(length <= 0){
            return B;
        }
        B[0] = 1;
        for(int i = 1; i < length; i++){
            B[i] = B[i - 1] * A[i - 1];
        }
        int temp = 1;
        for(int i = length - 2; i >= 0; i--){
            temp *= A[i + 1];
            B[i] *= temp;
        }
        return B;
    }
```



## 1.2 链表

### 1.2.1 从尾到头打印链表

- 问题描述
  - 输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
- 问题思路
  - 使用栈实现这种顺序。每经过一个结点的时候，把该结点放到一个栈中。当遍历完整个链表后，再从栈顶开始逐个输出结点的值，给一个新的链表结构，这样链表就实现了反转。
- 编程实现

```c++
    vector<int> printListFromTailToHead(ListNode* head) {
        if(head == NULL)
            return {};
        
        vector<int> res;
        stack<int> s;
        while(head != NULL) {
            s.push(head->val);
            head = head->next;
        }
        
        while(!s.empty()) {
            int tmp = s.top();
            s.pop();
            res.push_back(tmp);
        }
        
        return res;
    }
```

### 1.2.2 链表中倒数第k个结点

- 问题描述
  - 输入一个链表，输出该链表中倒数第k个结点。
- 问题思路
  - 定义两个指针。第一个指针从链表的头指针开始遍历向前走k-1，第二个指针保持不动；从第k步开始，第二个指针也开始从链表的头指针开始遍历。由于两个指针的距离保持在k-1，当第一个指针到达链表的尾结点时，第二个指针指针正好是倒数第k个结点。
- 编程实现

```
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead == NULL || k <= 0)
            return NULL;
        ListNode *p1 = pListHead;
        ListNode *p2 = pListHead;
        for(int i = 0; i < k - 1; i++) {
            if(p1->next != NULL)
                p1 = p1->next;
            else
                return NULL;
        }
        
        while(p1->next != NULL) {
            p1 = p1->next;
            p2 = p2->next;
        }
        return p2;
    }
```

### 1.2.3 反转链表

- 问题描述
  - 输入一个链表，反转链表后，输出新链表的表头。
- 问题思路
  - 使用三个指针，分别指向当前遍历到的结点、它的前一个结点以及后一个结点。在遍历的时候指针交替移动和改变next指针的指向即可。
- 编程实现

```C++
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead == NULL)
            return NULL;
        ListNode *now = pHead;
        ListNode *pre = NULL;
        ListNode *p_next = NULL;
        
        ListNode *res = NULL;
        
        while(now != NULL) {
            p_next = now->next;
            if(p_next == NULL)
                res = now;
            
            now->next = pre;
            pre = now;
            now = p_next;
        }
        
        return res;
    }
```

### 1.2.4 合并两个排序的链表

- 问题描述
  - 输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
- 问题思路
  - 判断两个链表是否为空，如果list1为空，则返回list2，反之亦然；
  - 使用递归的方式，通过链表值的判断，依次递增构建新的链表节点指。
- 编程实现

```c++
    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == NULL && pHead2 == NULL)
            return NULL;
        if(pHead1 == NULL)
            return pHead2;
        if(pHead2 == NULL)
            return pHead1;
        ListNode *head = NULL;
        if(pHead1->val < pHead2->val) {
            head = new ListNode(pHead1->val);
            head->next = Merge(pHead1->next, pHead2);
        }
        else {
            head = new ListNode(pHead2->val);
            head->next = Merge(pHead1, pHead2->next);
        }
        return head;
    }
```

### 1.2.5 复杂链表的复制

- 问题描述
  - 输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。
- 问题思路
  - 复杂链表的复制可以分为三步：
  - 第一步：复制复杂指针的label和next。但是这次我们把复制的结点跟在元结点后面，而不是直接创建新的链表；
  - 第二步：设置复制出来的结点的random。因为新旧结点是前后对应关系，所以也是一步就能找到random；
  - 第三步：拆分链表。奇数是原链表，偶数是复制的链表。
- 编程实现

```C++
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(pHead == NULL)
            return pHead;
        
        RandomListNode *tmp = pHead;
        while(tmp) {
            RandomListNode *copy = new RandomListNode(tmp->label);
            RandomListNode *pNext = tmp->next;
            tmp->next = copy;
            copy->next = pNext;
            tmp = pNext;
        }
        
        tmp = pHead;
        while(tmp) {
            RandomListNode *pCloned = tmp->next;
            if(tmp->random != NULL) {
                pCloned->random = tmp->random->next;
            }
            tmp = pCloned->next;
        }
        
        tmp = pHead;
        RandomListNode* pClonedHead = NULL;
        RandomListNode* pClonedNode = NULL;
        
        while(tmp != NULL) {
            if(pClonedHead == NULL) {
                pClonedHead = pClonedNode = tmp->next;
            }
            else {
                pClonedNode->next = tmp->next;
                pClonedNode = pClonedNode->next;
            }
            
            tmp->next = pClonedNode->next;
            tmp = tmp->next;
        }
        return pClonedHead;
    }
```

### 1.2.6 两个链表的第一个公共结点

- 问题描述

  - 输入两个链表，找出它们的第一个公共结点。

- 问题思路

  - 先让把长的链表的头砍掉，让两个链表长度相同,即让长的链表先走差值步，这样，同时遍历也能找到公共结点。此时，时间复杂度O(m+n)，空间复杂度为O(MAX(m,n))。

- 编程实现

  ```c++
      int get_list_length(ListNode *head) {
          if(head == NULL)
              return 0;
          else {
              int len = 0;
              while(head != NULL) {
                  len++;
                  head = head->next;
              }
              return len;
          }
      }
      
      ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
          // 如果有一个链表为空，则返回结果为空
          if(pHead1 == NULL || pHead2 == NULL){
              return NULL;
          }
          // 获得两个链表的长度
          int len1 = get_list_length(pHead1);
          int len2 = get_list_length(pHead2);
          // 默认 pHead1 长， pHead2短，如果不是，再更改
          ListNode* pHeadLong = pHead1;
          ListNode* pHeadShort = pHead2;
          int LengthDif = len1 - len2;
          // 如果 pHead1 比 pHead2 小
          if(len1 < len2){
              ListNode* pHeadLong = pHead2;
              ListNode* pHeadShort = pHead1;
              int LengthDif = len2 - len1;
          }
          // 将长链表的前面部分去掉，使两个链表等长
          for(int i = 0; i < LengthDif; i++){
              pHeadLong = pHeadLong->next;
          }
          
          while(pHeadLong != NULL && pHeadShort != NULL && pHeadLong != pHeadShort){
              pHeadLong = pHeadLong->next;
              pHeadShort = pHeadShort->next;
          }
          return pHeadLong;
      }
  ```

  

### 1.2.7 链表中环的入口结点

- 问题描述
  - 给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。
- 问题思路
  - 可以使用快慢指针，一个每次走一步，一个每次走两步。如果两个指针相遇，表明链表中存在环，并且两个指针相遇的结点一定在环中。
  - 然后令快指针回到链表头位置，快慢指针均每次前进一步，则快慢指针下次相遇的位置就是环的入口节点
- 编程实现

```C++
    ListNode* EntryNodeOfLoop(ListNode* pHead)
    {
        if(pHead == NULL){
            return NULL;
        }
        ListNode* meetingnode = MeetingNode(pHead);
        if(meetingnode == NULL){
            return NULL;
        }
        // 回环链表结点个数
        int nodesloop = 1;
        // 找到环中结点个数
        ListNode* pNode1 = meetingnode;
        while(pNode1->next != meetingnode){
            pNode1 = pNode1->next;
            nodesloop++;
        }
        pNode1 = pHead;
        // 第一个指针向前移动nodesloop步
        for(int i = 0; i < nodesloop; i++){
            pNode1 = pNode1->next;
        }
        // 两个指针同时移动，找到环入口
        ListNode* pNode2 = pHead;
        while(pNode1 != pNode2){
            pNode1 = pNode1->next;
            pNode2 = pNode2->next;
        }
        return pNode1;
    }

    // 使用快慢指针，找到任意的一个环中结点
    ListNode* MeetingNode(ListNode* pHead){
        ListNode* pSlow = pHead->next;
        if(pSlow == NULL){
            return NULL;
        }
        ListNode* pFast = pSlow->next;
        while(pFast != NULL && pSlow != NULL){
            if(pFast == pSlow){
                return pFast;
            }
            pSlow = pSlow->next;
            pFast = pFast->next;
            if(pFast != NULL){
                pFast = pFast->next;
            }
        }
        return NULL;
    }
```



### 1.2.8 删除链表中重复的结点

- 问题描述
  - 在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5
- 问题思路
  - 删除重复结点，只需要记录当前结点前的最晚访问过的不重复结点pPre、当前结点pCur、指向当前结点后面的结点pNext的三个指针即可。如果当前节点和它后面的几个结点数值相同，那么这些结点都要被剔除，然后更新pPre和pCur；如果不相同，则直接更新pPre和pCur。
  - 如果第一个结点是重复结点，那么就把头指针pHead也更新一下。
- 编程实现

```C++
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(pHead == NULL){
            return NULL;
        }
        // 指向当前结点前最晚访问过的不重复结点
        ListNode* pPre = NULL;
        // 指向当前处理的结点
        ListNode* pCur = pHead;
        // 指向当前结点后面的结点
        ListNode* pNext = NULL;
        
        while(pCur != NULL){
            // 如果当前结点与下一个结点相同
            if(pCur->next != NULL && pCur->val == pCur->next->val){
                pNext = pCur->next;
                // 找到不重复的最后一个结点位置
                while(pNext->next != NULL && pNext->next->val == pCur->val){
                    pNext = pNext->next;
                }
                // 如果pCur指向链表中第一个元素，pCur -> ... -> pNext ->... 
                // 要删除pCur到pNext, 将指向链表第一个元素的指针pHead指向pNext->next。
                if(pCur == pHead){
                    pHead = pNext->next;
                }
                // 如果pCur不指向链表中第一个元素，pPre -> pCur ->...->pNext ->... 
                // 要删除pCur到pNext，即pPre->next = pNext->next
                else{
                    pPre->next = pNext->next;
                }
                // 向前移动
                pCur = pNext->next;
            }
            // 如果当前结点与下一个结点不相同
            else{
                pPre = pCur;
                pCur = pCur->next;
            }
        }
        return pHead;
    }
```



## 1.3 二叉树

### 1.3.1 重建二叉树

- 问题描述
  - 输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
- 问题思路
  - 前序遍历序列中，第一个数字总是树的根结点的值。在中序遍历序列中，根结点的值在序列的中间，左子树的结点的值位于根结点的值的左边，而右子树的结点的值位于根结点的值的右边。可以递归来实现分别构建左右子树。
- 编程实现

```C++
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.empty() || vin.empty())
            return nullptr;
        
        int root_index = 0;
        TreeNode* root = new TreeNode(pre[root_index]);
        
        for(int i = 0; i < vin.size(); i++)
        {
            if(vin[i] == pre[0])
                break;
            root_index++;
        }
        
        vector<int> pre_left;
        vector<int> pre_right;
        vector<int> vin_left;
        vector<int> vin_right;
        
        for(int i = 0; i < root_index; i++)
        {
            pre_left.push_back(pre[i+1]);
            vin_left.push_back(vin[i]);
        }
        
        for(int i = root_index + 1; i < pre.size(); i++)
        {
            pre_right.push_back(pre[i]);
            vin_right.push_back(vin[i]);
        }
        
        root->left = reConstructBinaryTree(pre_left, vin_left);
        root->right = reConstructBinaryTree(pre_right, vin_right);
        
        return root;
    }
```

### 1.3.2 树的子结构

- 问题描述
  - 输入两棵二叉树A，B，判断B是不是A的子结构。
  - 空树不是任意一个树的子结构.
- 问题思路
  - 使用递归的方式。
  - 分为两步，第一步：在树A中找到和树B的根节点值相同的节点，
  - 第二步：判断树A以第一步节点为根节点的子树是否和树B有一样的结构，这里要注意如果树B为空但是树A不为空时，树B是树A的子结构，
- 编程实现

```c++
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        bool flag = false;
        if(pRoot1 != NULL && pRoot2 != NULL) {
            if(pRoot1->val == pRoot2->val) {
                flag = childHasSubtree(pRoot1, pRoot2);
            }
            if(!flag) {
                flag = HasSubtree(pRoot1->left, pRoot2);
            }
            if(!flag) {
                flag = HasSubtree(pRoot1->right, pRoot2);
            }
        }
        
        return flag;
    }
    
    bool childHasSubtree(TreeNode* pRoot1, TreeNode* pRoot2) {
        if(pRoot2 == NULL)
            return true;
        
        if(pRoot1 == NULL)
            return false;
        
        if(pRoot1->val != pRoot2->val)
            return false;
        
        return childHasSubtree(pRoot1->left, pRoot2->left) && 
            childHasSubtree(pRoot1->right, pRoot2->right);
    }
```

### 1.3.3 二叉树的镜像

- 问题描述
  - 操作给定的二叉树，将其变换为源二叉树的镜像。
  - 实例：
  - 二叉树的镜像定义：源二叉树 
  - 8
  - 6   10
  - 5  7 9 11
  - 镜像二叉树
  - 8
  - 10   6
  - 11 9 7  5
- 问题思路
  - 先交换根节点的两个子结点，然后递归交换左右子树
- 编程实现

```c++
    void Mirror(TreeNode *pRoot) {
        if(pRoot == NULL)
            return;
        
        TreeNode *temp = pRoot->left;
        pRoot->left = pRoot->right;
        pRoot->right = temp;
        
        if(pRoot->left)
            Mirror(pRoot->left);
        
        if(pRoot->right)
            Mirror(pRoot->right);
    }
```

### 1.3.4 从上往下打印二叉树

- 问题描述
  - 从上往下打印出二叉树的每个节点，同层节点从左至右打印。
- 问题思路
  - 使用队列
  - 每一次打印一个结点的时候，如果该结点有子结点，则把该结点的子结点放到一个队列的末尾。接下来到队列的头部取出最早进入队列的结点，重复前面的打印操作，直至队列中所有的结点都打印出来为止。
- 编程实现

```c++
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> res;
        if(root == NULL)
            return res;
        queue<TreeNode*> q;
        q.push(root);
        
        while(!q.empty()) {
            TreeNode *tmp = q.front();
            if(tmp->left)
                q.push(tmp->left);
            if(tmp->right)
                q.push(tmp->right);
            
            res.push_back(tmp->val);
            q.pop();
        }
        
        return res;
    }
```

### 1.3.5 二叉树中和为某一值的路径

- 问题描述
  - 输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)
- 问题思路
  - 深度优先搜索
  - 由于路径总是以根节点为起始点，所以采用前序遍历的方式。
  - 使用两个全局变量二维数组result和一维数组tmp，result来存放最终结果，tmp用来存放临时结果。
  - 每次遍历，我们先把root的值压入tmp，然后判断当前root是否同时满足：与给定数值相减为0；左子树为空；右子树为空。如果满足条件，就将tmp压入result中，否则，依次遍历左右子树。
  - 需要注意的是，遍历左右子树的时候，全局变量tmp是不清空的，直到到了根结点才请空tmp。
- 编程实现

```c++
class Solution {
private:
    vector<vector<int> > res;
    vector<int> tmp;
public:
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        if(root == NULL)
            return res;
        
        tmp.push_back(root->val);
        if(expectNumber == root->val && root->left == NULL && root->right == NULL) {
            res.push_back(tmp);
        }
        
        FindPath(root->left, expectNumber - root->val);
        FindPath(root->right, expectNumber - root->val);
        
        tmp.pop_back();
        return res;
    }
};
```

### 1.3.6 二叉树的深度

- 问题描述
  - 输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。
- 问题思路
  - 求二叉树的深度。
  - 一是递归的方法，属于DFS（深度优先搜索）；
  - 一种方法是按照层次遍历，属于BFS（广度优先搜索）。
- 编程实现

```C++
    int TreeDepth_dfs(TreeNode* pRoot) {
        if(pRoot == NULL) {
            return 0;
        }
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        return (left > right) ? (left + 1) : (right + 1);
    }
    
    int TreeDepth_bfs(TreeNode* pRoot) {
        if(pRoot == NULL){
            return 0;
        }
        queue<TreeNode*> que;
        int depth = 0;
        que.push(pRoot);
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; i++){
                TreeNode* node = que.front();
                que.pop();
                if(node->left){
                    que.push(node->left);
                }
                if(node->right){
                    que.push(node->right);
                }
            }
        }
        return depth;
    }
```





### 1.3.7 平衡二叉树

- 问题描述
  - 输入一棵二叉树，判断该二叉树是否是平衡二叉树。
- 问题思路
  - 平衡二叉树的定义是：所谓的平衡之意，就是树中任意一个结点下左右两个子树的高度差不超过 1。
  - 用后序遍历的方式遍历二叉树的每一个结点，在遍历到一个结点之前我们就已经遍历了它的左右子树。
  - 只要在遍历每个结点的时候记录它的深度（某一结点的深度等于它到叶结点的路径的长度），我们就可以一边遍历一边判断每个结点是不是平衡的。
- 编程实现

````C++
	bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth = 0;
        return IsBalanced(pRoot, &depth);
    }

    int IsBalanced(TreeNode* pRoot, int* depth){
        if(pRoot == NULL){
            *depth = 0;
            return true;
        }
        int left, right;
        if(IsBalanced(pRoot->left, &left) && IsBalanced(pRoot->right, &right)){
            int diff = left - right;
            if(diff <= 1 && diff >= -1){
                *depth = 1 + (left > right ? left : right);
                return true;
            }
        }
        return false;
    }
````





### 1.3.8 二叉树的下一个结点

- 问题描述
  - 给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。
- 问题思路
  - 如果一个结点有右子树，那么它的下一个结点就是它的右子树的最左子结点。
  - 没有右子树，如果结点是它父结点的左子结点，那么它的下一个结点就是它的父结点。
  - 如果一个结点既没有右子树，并且它还是父结点的右子结点，沿着指向父结点的指针一直向上遍历，直到找到一个是它父结点的左子结点的结点。
- 编程实现

```C++
    TreeLinkNode* GetNext(TreeLinkNode* pNode)
    {
        if(pNode == NULL){
            return NULL;
        }
        TreeLinkNode* pNext = NULL;
        // 当前结点有右子树，那么它的下一个结点就是它的右子树中最左子结点
        if(pNode->right != NULL){
            TreeLinkNode* pRight = pNode->right;
            while(pRight->left != NULL){
                pRight = pRight-> left;
            }
            pNext = pRight;
        }
        // 当前结点无右子树，则需要找到一个是它父结点的左子树结点的结点
        else if(pNode->next != NULL){
            // 当前结点
            TreeLinkNode* pCur = pNode;
            // 父节点
            TreeLinkNode* pPar = pNode->next;
            while(pPar != NULL && pCur == pPar->right){
                pCur = pPar;
                pPar = pCur->next;
            }
            pNext = pPar;
        }
        return pNext;
    }
```



### 1.3.9 对称的二叉树

- 问题描述
  - 请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。
- 问题思路
  - 三种不同的二叉树遍历算法，即前序遍历、中序遍历和后序遍历。在这三种遍历算法中，都是先遍历左子结点再遍历右子结点。
  - 可以定义一个遍历算法，先遍历右子结点再遍历左子结点，暂且称其为前序遍历的对称遍历。
  - 遍历第一棵树，前序遍历的遍历序列为{8,6,5,7,6,7,5}，其对称遍历的遍历序列为{8,6,5,7,6,7,5}。
  - 遍历第二颗树，前序遍历的遍历序列为{8,6,5,7,9,7,5}，其对称遍历的遍历序列为{8,9,5,7,6,7,5}。
  - 可以看到，使用此方法可以区分前两棵树，第一棵树为对称树，第二颗树不是对称树。
- 编程实现

```C++
public:
    bool isSymmetrical(TreeNode* pRoot)
    {
        if(pRoot == NULL){
            return true;
        }
        return isSymmetriacalCor(pRoot, pRoot);
    }

    bool isSymmetriacalCor(TreeNode* pRoot1, TreeNode* pRoot2){
        if(pRoot1 == NULL && pRoot2 == NULL){
            return true;
        }
        if(pRoot1 == NULL || pRoot2 == NULL){
            return false;
        }
        if(pRoot1->val != pRoot2->val){
            return false;
        }
        return isSymmetriacalCor(pRoot1->left, pRoot2->right) && isSymmetriacalCor(pRoot1->right, pRoot2->left);
    }
```



### 1.3.10 按之字顺序打印二叉树

- 问题描述
  - 请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。
- 问题思路
  - 我们需要使用两个栈。我们在打印某一行结点时，把下一层的子结点保存到相应的栈里。如果当前打印的是奇数层（第一层、第三层等），则先保存左子树结点再保存右子树结点到第一个栈里。如果当前打印的是偶数层（第二层、第四层等），则则先保存右子树结点再保存左子树结点到第二个栈里。
- 编程实现

```C++
    vector<vector<int> > Print(TreeNode* pRoot) {
        vector<vector<int> > result;
        if(pRoot == NULL){
            return result;
        }
        stack<TreeNode* > s[2];
        s[0].push(pRoot);
        while(!s[0].empty() || !s[1].empty()){
            vector<int> v[2];
            // 偶数行
            while(!s[0].empty()){
                v[0].push_back(s[0].top()->val);
                if(s[0].top()->left != NULL){
                    s[1].push(s[0].top()->left);
                }
                if(s[0].top()->right != NULL){
                    s[1].push(s[0].top()->right);
                }
                s[0].pop();
            }
            if(!v[0].empty()){
                result.push_back(v[0]);
            }
            // 奇数行
            while(!s[1].empty()){
                v[1].push_back(s[1].top()->val);
                if(s[1].top()->right != NULL){
                    s[0].push(s[1].top()->right);
                }
                if(s[1].top()->left != NULL){
                    s[0].push(s[1].top()->left);
                }
                s[1].pop();
            }
            if(!v[1].empty()){
                result.push_back(v[1]);
            }
        }
        return result;
    }
```



### 1.3.11 把二叉树打印成多行

- 问题描述
  - 从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。
- 问题思路
  - 使用队列即可
- 编程实现

```C++
	vector<vector<int> > Print(TreeNode* pRoot) {
            vector<vector<int> > result;
            if(pRoot == NULL){
                return result;
            }
            queue<TreeNode* > nodes[2];
            nodes[0].push(pRoot);
            while(!nodes[0].empty() || !nodes[1].empty()){
                vector<int> v[2];
                while(!nodes[0].empty()){
                    v[0].push_back(nodes[0].front()->val);
                    if(nodes[0].front()->left != NULL){
                        nodes[1].push(nodes[0].front()->left);
                    }
                    if(nodes[0].front()->right != NULL){
                        nodes[1].push(nodes[0].front()->right);
                    }
                    nodes[0].pop();
                }
                if(!v[0].empty()){
                    result.push_back(v[0]);
                }
                while(!nodes[1].empty()){
                    v[1].push_back(nodes[1].front()->val);
                    if(nodes[1].front()->left != NULL){
                        nodes[0].push(nodes[1].front()->left);
                    }
                    if(nodes[1].front()->right != NULL){
                        nodes[0].push(nodes[1].front()->right);
                    }
                    nodes[1].pop();
                }
                if(!v[1].empty()){
                    result.push_back(v[1]);
                }
            }
            return result;
        }
```



### 1.3.12 序列化二叉树

- 问题描述
  - 请实现两个函数，分别用来序列化和反序列化二叉树
  - 二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（#），以 ！ 表示一个结点值的结束（value!）。
  - 二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。
- 问题思路
  - 使用前序遍历来序列化和发序列化即可
  - 使用$符号表示NULL，同时每个结点之间，需要添加逗号，即','进行分隔。
- 编程实现

```C++
    char* Serialize(TreeNode *root) {    
        if(!root){
            return NULL;
        }
        string str;
        SerializeCore(root, str);
        // 把str流中转换为字符串返回
        int length = str.length();
        char* res = new char[length+1];
        // 把str流中转换为字符串返回
        for(int i = 0; i < length; i++){
            res[i] = str[i];
        }
        res[length] = '\0';
        return res;
    }
    TreeNode* Deserialize(char *str) {
        if(!str){
            return NULL;
        }
        TreeNode* res = DeserializeCore(&str);
        return res;
    }
    void SerializeCore(TreeNode* root, string& str){
        // 如果指针为空，表示左子节点或右子节点为空，则在序列中用#表示
        if(!root){
            str += '#';
            return;
        }
        string tmp = to_string(root->val);
        str += tmp;
        // 加逗号，用于区分每个结点
        str += ',';
        SerializeCore(root->left, str);
        SerializeCore(root->right, str);
    }
    // 递归时改变了str值使其指向后面的序列，因此要声明为char**
    TreeNode* DeserializeCore(char** str){
        // 到达叶节点时，调用两次，都返回null，所以构建完毕，返回父节点的构建
        if(**str == '#'){
            (*str)++;
            return NULL;
        }
        // 因为整数是用字符串表示，一个字符表示一位，先进行转换
        int num = 0;
        while(**str != ',' && **str != '\0'){
            num = num * 10 + ((**str) - '0');
            (*str)++;
        }
        TreeNode* root = new TreeNode(num);
        if(**str == '\0'){
            return root;
        }
        else{
            (*str)++;
        }
        root->left = DeserializeCore(str);
        root->right = DeserializeCore(str);
        return root;
    }
```



## 1.4 二叉搜索树

### 1.4.1 二叉搜索树的后序遍历序列

- 问题描述
  - 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则返回true，否则返回false。
  - 输入的数组的任意两个数字都互不相同。
- 问题思路
  - 后序遍历的最后一个数字是根节点的值，由于二叉排序树的左子树上所有节点的值均小于它的根节点；右子树上所有节点的值均大于它的根节点，根据根节点值，把序列分成两个部分，前一部分是左子树，后一部分是右子树。然后使用递归的方法，再判断左子树、右子树是不是二叉搜索树。
- 编程实现

```c++
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.size() <= 0)
            return false;
        return bst(sequence, 0, sequence.size() - 1);
    }
    
     bool bst(vector<int> seq, int begin, int end){
        if(seq.empty() || begin > end){
            return false;
        }
        
        //根结点
        int root = seq[end];
        
        //在二叉搜索树中左子树的结点小于根结点
        int i = begin;
        for(; i < end; ++i){
            if(seq[i] > root)
                break;
        }
        
        //在二叉搜索书中右子树的结点大于根结点
        for(int j = i; j < end; ++j){
            if(seq[j] < root)
                return false;
        }
        
        bool left = true;
        if(i > begin)
            left = bst(seq, begin, i - 1);//判断左子树是不是二叉搜索树
        
        bool right = true;
        if(i < end - 1)
            right = bst(seq, i , end - 1);//判断右子树是不是二叉搜索树
        
        return left && right;
    }
```

### 1.4.2 二叉搜索树与双向链表

- 问题描述
  - 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。
- 问题思路
  - 根据二叉搜索树的特点：左结点的值<根结点的值<右结点的值,采用中序遍历的方式；
  - 把左右子树的转换成排序的双向链表后再和根节点连接起来，按照中序遍历的顺序，当我们遍历到根结点时，它的左子树已经转换成一个排序的好的双向链表了，并且处在链表中最后一个的结点是当前值最大的结点。先把左子树的最后一个节点和根节点连接起来，然后再和排好序的右子树的第一个节点连接起来。
- 编程实现

```C++
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        TreeNode *last_node = NULL;
        
        convert(pRootOfTree, &last_node);
        
        TreeNode *head = last_node;//返回头节点，从last_node向左找
        while(head != NULL && head->left != NULL)
            head = head->left;
        
        return head;
    }
    
    void convert(TreeNode* root, TreeNode** last_node) {
        if(root == NULL) {
            return;
        }
        TreeNode* cur = root;
        if(cur->left != NULL)
            convert(cur->left, last_node);
        
        cur->left = *last_node;
        if(*last_node != NULL)
            (*last_node)->right = cur;
        *last_node = cur;
        
        if(cur->right != NULL)
            convert(cur->right, last_node);
    }
```

### 1.4.3 二叉搜索树的第k个结点

- 问题描述
  - 给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第3小结点的值为4。
- 问题思路
  - 只需要用中序遍历一棵二叉搜索树，就很容易找出它的第k大结点。
- 编程实现

```C++
    TreeNode* KthNode(TreeNode* pRoot, int k)
    {
        if(pRoot == NULL || k == 0){
            return NULL;
        }
        return KthNodeCore(pRoot, k);
    }

    TreeNode* KthNodeCore(TreeNode* pRoot, int &k){
        TreeNode* target = NULL;
        // 先遍历左结点
        if(pRoot->left != NULL){
            target = KthNodeCore(pRoot->left, k);
        }
        // 如果没有找到target，则继续减小k，如果k等于1，说明到了第k大的数
        if(target == NULL){
            if(k == 1){
                target = pRoot;
            }
            k--;
        }
        // 如果没有找到target，继续找右结点
        if(pRoot->right != NULL && target == NULL){
            target = KthNodeCore(pRoot->right, k);
        }
        return target;
    }
```



## 1.5 字符串

### 1.5.1 替换空格

- 问题描述
  - 请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
- 问题思路
  - 先遍历一次字符串，这样就可以统计出字符串空格的总数，并可以由此计算出替换之后的字符串的总长度。每替换一个空格，长度增加2.
  - 然后从字符串的尾部开始复制和替换。首先准备两个指针，P1和P2，P1指向原始字符串的末尾，而P2指向替换之后的字符串的末尾。接下来我们向前移动指针P1，逐个把它指向的字符复制到P2指向的位置，直到碰到第一个空格为止。碰到第一个空格之后，把P1向前移动1格，在P2之前插入字符串"%20"。由于"%20"的长度为3，同时也要把P2向前移动3格。
- 编程实现

```C++
	void replaceSpace(char *str,int length) {
        if(str == NULL)
            return;
        
        int count = 0;
        int ori_length = 0;
        int i = 0;
        while(str[i] != '\0')
        {
            if(str[i] == ' ')
                count++;
            i++;
            ori_length++;
        }
        
        int new_length = ori_length + count * 2;
        
        if(ori_length > length)
            return;
        while(ori_length >= 0 && new_length > ori_length) {
            if(str[ori_length] == ' ')
            {
                str[new_length--] = '0';
                str[new_length--] = '2';
                str[new_length--] = '%';
            }
            else
            {
                str[new_length--] = str[ori_length];
            }
            ori_length--;
        }
	}
```

### 1.5.2 字符串的排列

- 问题描述
  - 输入一个字符串,按字典序打印出该字符串中字符的所有排列。
  - 例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
- 问题思路
  - 可以分为两步：
  - 第一步：求所有可能出现在第一个位置的字符，即把第一个字符和后面所有的字符交换。
  - 第二步：固定第一个字符，求后面所有字符的排列。仍把后面的所有字符分为两部分：后面的字符的第一个字符，以及这个字符之后的所有字符。然后把第一个字符逐一和它后面的字符交换。
- 编程实现

```C++
    vector<string> Permutation(string str) {
        vector<string> res;
        if(str.size() <= 0)
            return res;
        
        permutation(res, str, 0);
        
        return res;
    }
    
    void permutation(vector<string> &res, string str, int index) {
        if(index == str.size()) {//排列到最后一个字符，结束
            res.push_back(str);
            return;
        }
        
        for(int i = index; i < str.size(); i++) {
            //如果后边的字符和第一个字符重复了，则不进行交换
            if(i != index && str[i] == str[index]) { 
                continue;
            }
            
            swap(str[index], str[i]);//交换第一个字符和后边的字符
            permutation(res, str, index + 1);//固定第一个字符，求后边字符的排列
        }
    }
```

### 1.5.3 第一个只出现一次的字符

- 问题描述
  - 在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.
- 问题思路
  - 建立一个哈希表，第一次扫描的时候，统计每个字符的出现次数。第二次扫描的时候，如果该字符出现的次数为1，则返回这个字符的位置。
- 编程实现

```C++
    int FirstNotRepeatingChar(string str) {
        if(str.size() <= 0)
            return -1;
        map<char, int> m;
        for(int i = 0; i < str.size(); i++) {
            if(m.find(str[i]) != m.end())
                m[str[i]] += 1;
            else
                m[str[i]] = 1;
        }
        for(int i = 0; i < str.size(); i++) {
            if(m[str[i]] == 1)
                return i;
        }
        return -1;
    }
```



### 1.5.4 左旋转字符串

- 问题描述
  - 对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。
  - 例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。
- 问题思路
- 编程实现

```C++
	string LeftRotateString(string str, int n) {
        string result = str;
        int length = result.size();
        if(length < 0){
            return NULL;
        }
        if(0 <= n <= length){
            int pFirstBegin = 0, pFirstEnd = n - 1;
            int pSecondBegin = n, pSecondEnd = length - 1;
            ReverseString(result, pFirstBegin, pFirstEnd);
            ReverseString(result, pSecondBegin, pSecondEnd);
            ReverseString(result, pFirstBegin, pSecondEnd);
        }
        return result;
    }

    void ReverseString(string &str, int begin, int end){
        while(begin < end){
            swap(str[begin++], str[end--]);
        }
    }
```



### 1.5.5 翻转单词顺序序列

- 问题描述
  - “I am a student.”反转成“student. a am I”
- 问题思路
  - 只需要对每个单词做翻转，然后再整体做翻转就得到了正确的结果。
- 编程实现

```C++
	string ReverseSentence(string str) {
        string result = str;
        int length = result.size();
        if(length == 0){
            return "";
        }
        // 追加一个空格，作为反转标志位
        result += ' ';
        int mark = 0;
        // 根据空格，反转所有单词
        for(int i = 0; i < length + 1; i++){
            if(result[i] == ' '){
                Reverse(result, mark, i - 1);
                mark = i + 1;
            }
        }
        // 去掉添加的空格
        result = result.substr(0, length);
        // 整体反转
        Reverse(result, 0, length - 1);
        
        return result;
    }

    void Reverse(string &str, int begin, int end){
        while(begin < end){
            swap(str[begin++], str[end--]);
        }
    }
```



### 1.5.6 把字符串转换成整数

- 问题描述
  - 将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。
- 问题思路
- 编程实现

```C++
    int myAtoi(string str) {
        if(str.size() <= 0)
            return 0;
        
        int len = str.size();
        int i = 0;
        
        //找到第一个非空格的字符
        while(i < len) {
            if(str[i] != ' ')
                break;
            i++;
        }
        //如果全是空格
        if(i == len)
            return 0;
        
        //运算符正负号
        bool flag = false;
        if(str[i] == '-' && str[i+1] != '+') {
            flag = true;
            i++;
        }
        
        if(str[i] == '+' && str[i+1] != '-')
            i++;
        
        long long res = 0;
        while(i < len && str[i] >= '0' && str[i] <= '9') {
            res = res * 10 + (str[i++] - '0');
            if(res > INT_MAX)
                return flag ? INT_MIN : INT_MAX;
            if(res < INT_MIN)
                return flag ? INT_MIN : INT_MAX;
        }
        if(flag)
            res = -res;
        return static_cast<int>(res);
    }
```



### 1.5.7 正则表达式匹配

- 问题描述
  - 请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"abaca"匹配，但是与"aa.a"和"ab*a"均不匹配
- 问题思路
  - 正常匹配
  - 动态规划思路
- 编程实现

```C++
    bool match(char* str, char* pattern)
    {
        // 指针为空，返回false
        if(str == NULL || pattern == NULL){
            return false;
        }
        return matchCore(str, pattern);
    }

    bool matchCore(char* str, char* pattern){
        // 字符串和模式串都运行到了结尾，返回true
        if(*str == '\0' && *pattern == '\0'){
            return true;
        }
        // 字符串没有到结尾，模式串到了，则返回false
        // 模式串没有到结尾，字符串到了，则根据后续判断进行，需要对'*'做处理
        if((*str != '\0' && *pattern == '\0')){
            return false;
        }
        // 如果模式串的下一个字符是'*'，则进入状态机的匹配
        if(*(pattern + 1) == '*'){
            // 如果字符串和模式串相等，或者模式串是'.'，并且字符串没有到结尾，则继续匹配
            if(*str == *pattern || (*pattern == '.' && *str != '\0')){
                // 进入下一个状态，就是匹配到了一个
                return matchCore(str + 1, pattern + 2) ||
                    // 保持当前状态，就是继续那这个'*'去匹配
                    matchCore(str + 1, pattern) ||
                    // 跳过这个'*'
                    matchCore(str, pattern + 2);
            }
            // 如果字符串和模式串不相等，则跳过当前模式串的字符和'*'，进入新一轮的匹配
            else{
                // 跳过这个'*'
                return matchCore(str, pattern + 2);
            }
        }
        // 如果字符串和模式串相等，或者模式串是'.'，并且字符串没有到结尾，则继续匹配
        if(*str == *pattern || (*pattern == '.' && *str != '\0')){
            return matchCore(str + 1, pattern + 1);
        }
        return false;
    }

	bool isMatch(string s, string p){
        int len_S = s.length();
        int len_P = p.length();

        if(len_P && p[0] == '*')
            return false;

        vector<vector<bool> > dp(len_S + 1, vector<bool>(len_P + 1, false));
        dp[0][0] = true;

        for(int i = 1; i <= len_P; i++) {
            if(p[i - 1] == '*')
                dp[0][i] = dp[0][i - 1] || dp[0][i - 2];//
        }

        for(int i = 1; i <= len_S; i++) {
            for(int j = 1; j <= len_P; j++) {
                    if(p[j - 1] != '*' && (p[j - 1] == s[i - 1] || p[j - 1] == '.') ) {
                        dp[i][j] = dp[i - 1][j - 1];
                    }
                    else if(p[j - 1] == '*') {
                        if(p[j - 2] == s[i - 1] || p[j - 2] == '.')
                            dp[i][j] = dp[i - 1][j] || dp[i][j - 1] || dp[i][j - 2];
                        else
                            dp[i][j] = dp[i][j - 2];
                    }
            }
        }

        return dp[len_S][len_P];
	}
```



### 1.5.8 表示数值的字符串

- 问题描述
  - 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。
- 问题思路
  - 表示数值的字符串遵循如下模式：
  - [sign]integral-digits[.[fractional-digits]][e|E[sign]exponential-digits]其中，('['和']'之间的为可有可无的部分)。
  - 在数值之前可能有一个表示正负的'+'或者'-'。接下来是若干个0到9的数位表示数值的整数部分（在某些小数里可能没有数值的整数部分）。如果数值是一个小数，那么在小数后面可能会有若干个0到9的数位表示数值的小数部分。如果数值用科学记数法表示，接下来是一个'e'或者'E'，以及紧跟着的一个整数（可以有正负号）表示指数。
  - 判断一个字符串是否符合上述模式时，首先看第一个字符是不是正负号。如果是，在字符串上移动一个字符，继续扫描剩余的字符串中0到9的数位。如果是一个小数，则将遇到小数点。另外，如果是用科学记数法表示的数值，在整数或者小数的后面还有可能遇到'e'或者'E'。
- 编程实现

```C++
    // 数字的格式可以用A[.[B]][e|EC]或者.B[e|EC]表示，
    // 其中A和C都是整数（可以有正负号，也可以没有）
    // 而B是一个无符号整数
    bool isNumeric(char* string)
    {
        // 非法输入处理
        if(string == NULL || *string == '\0'){
            return false;
        }
        // 正负号判断
        if(*string == '+' || *string == '-'){
            ++string;
        }
        bool numeric = true;
        scanDigits(&string);
        if(*string != '\0'){
            // 小数判断
            if(*string == '.'){
                ++string;
                scanDigits(&string);
                if(*string == 'e' || *string == 'E'){
                    numeric = isExponential(&string);
                }
            }
            // 整数判断
            else if(*string == 'e' || *string == 'E'){
                numeric = isExponential(&string);
            }
            else{
                numeric = false;
            }
        }
        return numeric && *string == '\0';
    }

    // 扫描数字，对于合法数字，直接跳过
    void scanDigits(char** string){
        while(**string != '\0' && **string >= '0' && **string <= '9'){
            ++(*string);
        }
    }
    // 用来潘达un科学计数法表示的数值的结尾部分是否合法
    bool isExponential(char** string){
        ++(*string);
        if(**string == '+' || **string == '-'){
            ++(*string);
        }
        if(**string == '\0'){
            return false;
        }
        scanDigits(string);
        // 判断是否结尾，如果没有结尾，说明还有其他非法字符串
        return (**string == '\0') ? true : false;
    }
```



## 1.6 栈

### 1.6.1 用两个栈实现队列

- 问题描述
  - 用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
- 问题思路
  - 插入：入栈的元素都插入到stack1中；
  - 弹出：当stack2中不为空时，在stack2中的栈顶元素是最先进入队列的元素，可以弹出。如果stack2为空时，我们把stack1中的元素逐个弹出并压入stack2。由于先进入队列的元素被压倒stack1的栈底，经过弹出和压入之后就处于stack2的栈顶，可以直接弹出。即可实现队列的先进先出。
- 编程实现

```c++
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if(!stack1.empty() || !stack2.empty()) {
            int tmp = 0;
            if(stack2.empty()){
                while(!stack1.empty()){
                    tmp = stack1.top();
                    stack1.pop();
                    stack2.push(tmp);
                }
            }
            
            tmp = stack2.top();
            stack2.pop();
            return tmp;
        }
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

### 1.6.2 包含min函数的栈

- 问题描述
  - 定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。
- 问题思路
  - 使用两个stack，一个为数据栈，另一个为辅助栈。数据栈用于存储所有数据，辅助栈用于存储最小值。
  - 入栈时，数据栈保存所有数据，最小栈每次都入栈最小元素。
- 编程实现

```C++
class Solution {
public:
    void push(int value) {
        int min_val = 0;
        if(min_stack.empty())
            min_val = value;
        else
            min_val = value > min_stack.top() ? min_stack.top() : value;
        data_stack.push(value);
        min_stack.push(min_val);
    }
    void pop() {
        data_stack.pop();
        min_stack.pop();
    }
    int top() {
        return data_stack.top();
    }
    int min() {
        return min_stack.top();
    }
    
private:
    stack<int> data_stack;
    stack<int> min_stack;
};
```

### 1.6.3 栈的压入、弹出序列

- 问题描述
  - 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）
- 问题思路
  - 使用辅助栈，对于压入序列，依次压入辅助栈中，然后比较弹出序列的首部和辅助栈顶是否相等，如果相等，则弹出该值，并且继续判断弹出序列的首部和辅助栈顶是否相等，直到遇到不相等为止，然后继续压栈压入序列；
  - 最终判断辅助栈是否有元素，如果栈不为空，则说明不能是压入序列的弹出序列，反之则是。
- 编程实现

```c++
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        bool res = false;
        int i = 0, j = 0;
        stack<int> s;
        while(i < pushV.size()) {
            s.push(pushV[i++]);
            while(j < popV.size() && popV[j] == s.top()) {
                s.pop();
                j++;
            }
        }
        res = s.empty() ? true : false;
        return res;
    }
```



## 1.7 递归-动态规划

### 1.7.1 裴波那契数列

- 问题描述
  - 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。
  - 斐波那契数列的公式：
  - f(n)=0,n=0;
  - 1,n=1;
  - f(n-1)+f(n-2),n>1
- 问题思路
  - f(n)是依赖于f(n-1)和f(n-2)的，f(n-1)是依赖于f(n-2)和f(n-3)的，可以看出这种求解关系有很多值的重复的，所以可以从底开始向上求解，先根据f(0)和f(1)求出f(2),然后用f(1)和f(2)求出f(3)，依次计算，直到计算出f(n)。
- 编程实现

```c++
    int Fibonacci(int n) {
        if(n <= 0)
            return 0;
        if(n == 1)
            return 1;
        int first = 0, second = 1, third = 0;
        for (int i = 2; i <= n; i++) {
            third = first + second;
            first = second;
            second = third;
        }
        return third;
    }
```

### 1.7.2 跳台阶

- 问题描述
  - 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。
- 问题思路
  - 实际上就是斐波那契数列。
  - 我们把n级台阶时的跳法看成是n的函数，记为f(n)。当n>2时，第一次跳的时候就有两种不同的选择：一是第一次只跳1级，此时跳法数目等于后面剩下的n-1级台阶的跳法数目，即为f(n-1)；另外一种选择是跳一次跳2级，此时跳法数目等于后面剩下的n-2级台阶的跳法数目，即为f(n-2)。因此n级台阶的不同跳法的总数f(n)=f(n-1)+f(n-2)。
- 编程实现

```c++
    int jumpFloor(int number) {
        if(number <= 0){
            return 0;
        }
        else if(number < 3){
            return number;
        }
        int first = 1, second = 2, third = 0;
        for(int i = 3; i <= number; i++){
            third = first + second;
            first = second;
            second = third;
        }
        return third;
    }
```

### 1.7.3 变态跳台阶

- 问题描述
  - 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
- 问题思路
  - 当n=1时，结果为1；
  - 当n=2时，结果为2；
  - 当n=3时，结果为4；
  - 以此类推，我们使用数学归纳法不难发现，跳法f(n)=2^(n-1)。
- 编程实现

```c++
    int jumpFloorII(int number) {
        if(number == 0){
            return 0;
        }
        int total = 1;
        for(int i = 1; i < number; i++){
            total *= 2;
        }
        return total;
    }
```

### 1.7.4 矩形覆盖

- 问题描述
  - 我们可以用2 * 1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2 * 1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？
- 问题思路
  - 该问题还是斐波那契数列问题。
  - 把2 * n的覆盖方法记为f(n),当n=1时，只有一种覆盖方法；当n=2时，有两种覆盖方法；n>2时,矩形覆盖有两种方式：第一种是第一个矩形竖着放置，则右边还剩2 * n-1的区域，记为f(n-1)；第二种方式是左边放两个横着的矩形，则右边还剩2 * n-2的区域，记为f(n-2)，则f(n)=f(n-1)+f(n-2)。
- 编程实现

```C++
    int rectCover(int number) {
        if(number <= 0)
            return 0;
        if(number <= 2)
            return number;
        int first = 1,second = 2, third = 0;
        for(int i = 3; i <= number; i++)
        {
            third = first + second;
            first = second;
            second = third;
        }
        return third;
    }
```

## 1.8 回溯法

### 1.8.1 矩阵中的路径

- 问题描述
  - 请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。
- 问题思路
  - 用回溯法
  - 首先，遍历这个矩阵，我们很容易就能找到与字符串str中第一个字符相同的矩阵元素ch。然后遍历ch的上下左右四个字符，如果有和字符串str中下一个字符相同的，就把那个字符当作下一个字符（下一次遍历的起点），如果没有，就需要回退到上一个字符，然后重新遍历。为了避免路径重叠，需要一个辅助矩阵来记录路径情况。
- 编程实现

```C++
	bool hasPath(char* matrix, int rows, int cols, char* str){
        if(matrix == NULL || rows < 1 || cols < 1 || str == NULL){
            return false;
        }
        bool* visited = new bool[rows*cols];
        memset(visited, 0, rows*cols);
        int pathLength = 0;
        for(int row = 0; row < rows; row++){
            for(int col = 0; col < cols; col++){
                if(hasPathCore(matrix, rows, cols, row, col, str, pathLength, visited)){
                    delete[] visited;
                    return true;
                }
            }
        }
        delete[] visited;
        return false;
    }

    bool hasPathCore(char* matrix, int rows, int cols, int row, int col, char* str, int& pathLength, bool* visited){
        if(str[pathLength] == '\0'){
            return true;
        }
        bool hasPath = false;
        if(row >= 0 && row < rows && col >= 0 && col < cols && matrix[row*cols+col] == str[pathLength] && !visited[row*cols+col]){
            ++pathLength;
            visited[row*cols+col] = true;
            hasPath = hasPathCore(matrix, rows, cols, row-1, col, str, pathLength, visited)
                || hasPathCore(matrix, rows, cols, row+1, col, str, pathLength, visited)
                || hasPathCore(matrix, rows, cols, row, col-1, str, pathLength, visited)
                || hasPathCore(matrix, rows, cols, row, col+1, str, pathLength, visited);
            if(!hasPath){
                --pathLength;
                visited[row*cols+col] = false;
            }
        }
        return hasPath;
    }
```



### 1.8.2 机器人的运动范围

- 问题描述
  - 地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？
- 问题思路
  - 和上一道题十分相似，只不过这次的限制条件变成了坐标位数之和。对于求坐标位数之和，我们单独用一个函数实现，然后套入上一道题的代码中即可。
- 编程实现

```C++
    int movingCount(int threshold, int rows, int cols)
    {
        int count = 0;
        if(threshold < 1 || rows < 1 || cols < 1){
            return count;
        }
        bool* visited = new bool[rows*cols];
        memset(visited, 0, rows*cols);
        count = movingCountCore(threshold, rows, cols, 0, 0, visited);
        delete[] visited;
        return count;
    }

    int movingCountCore(int threshold, int rows, int cols, int row, int col, bool* visited){
        int count = 0;
        if(row >= 0 && row < rows && col >= 0 && col < cols && getDigitSum(row)+getDigitSum(col) <= threshold && !visited[row*cols+col]){
            visited[row*cols+col] = true;
            count = 1 + movingCountCore(threshold, rows, cols, row+1, col, visited)
                + movingCountCore(threshold, rows, cols, row-1, col, visited)
                + movingCountCore(threshold, rows, cols, row, col+1, visited)
                + movingCountCore(threshold, rows, cols, row, col-1, visited);
        }
        return count;
    }
    int getDigitSum(int num){
        int sum = 0;
        while(num){
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
```





## 1.9 其他

### 1.9.1 二进制中1的个数

- 问题描述
  - 输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
- 问题思路
  - 如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。
  - 即：把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。
  - 如：二进制1100，减一后变成1011,然后1100 & 1011 = 1000；
- 编程实现

```
     int  NumberOf1(int n) {
         int res = 0;
         
         while(n)
         {
             res++;
             n = n & (n - 1);
         }
         
         return res;
     }
```

### 1.9.2 数值的整数次方

- 问题描述
  - 给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。
- 问题思路
  - 当指数为负数的时候，可以先对指数求绝对值，然后算出次方的结果之后再取倒数。如果底数为0，则直接返回0。
  - 由于计算机表示小数（包括float和double型小数）都有误差，我们不能直接用等号（==）判断两个小数是否相等。如果两个小数的差的绝对值很小，比如小于0.0000001，就可以认为它们相等。
- 编程实现

```
    double Power(double base, int exponent) {
        if(base - 0.0 > -0.00000001 && base - 0.0 < 0.00000001)//判断base是否为0
            return 0.00;
        
        unsigned int tmp = exponent > 0 ? exponent : -exponent;
        
        double res = power_calculate(base, tmp);
        
        return exponent > 0 ? res : (1 / res);
    }
    
    double power_calculate(double base, unsigned int tmp)
    {
        if(tmp == 0)
            return 1;
        if(tmp == 1)
            return base;
        return base * power_calculate(base, tmp - 1);
    }
```

### 1.9.3 顺时针打印矩阵

- 问题描述
  - 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.
- 问题思路
  - 将结果存入vector数组，从左到右，再从上到下，再从右到左，最后从下到上遍历。
  - 注意边界，和特殊情况的处理
- 编程实现

```C++
    vector<int> printMatrix(vector<vector<int> > matrix) {
        int row = matrix.size();
        int col = matrix[0].size();
        vector<int> res;
        if(row <= 0 || col <= 0)
            return res;
        
        int left = 0, right = col - 1;
        int top = 0, bottom = row - 1;
        
        while(left <= right && top <= bottom) {
            for(int i = left; i <= right; i++)
                res.push_back(matrix[top][i]);
            
            for(int i = top + 1; i <= bottom; i++)
                res.push_back(matrix[i][right]);
            
            if(top != bottom) {
                for(int i = right - 1; i >= left; i--)
                    res.push_back(matrix[bottom][i]);
            }
            
            if(left != right) {
                for(int i = bottom - 1; i > top; i--)
                    res.push_back(matrix[i][left]);
            }
            left++;
            right--;
            top++;
            bottom--;
        }
        return res;
    }
```

### 1.9.4 最小的K个数

- 问题描述
  - 输入n个整数，找出其中最小的K个数。
  - 例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。
- 问题思路
  - Top K问题 - 最小K，使用最大堆 头文件algorithm-make_heap 、push_heap、pop_heap、sort_heap
  - 或者自己实现
- 编程实现

```C++
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> res;
        if(input.size() <= 0 || k <=0 || input.size() < k)
            return res;
        
        for(int i = 0; i < k; i++) {
            res.push_back(input[i]);
        }
        make_heap(res.begin(), res.end(), less<int>());
        
        for(int i = k; i < input.size(); i++) {
            if(input[i] < res[0]) {
                res.push_back(input[i]);
                push_heap(res.begin(), res.end());
                pop_heap(res.begin(), res.end());
                res.pop_back();
            }
        }
        
        sort_heap(res.begin(), res.end());
        return res;
    }
```

### 1.9.5 整数中1出现的次数（从1到n整数中1出现的次数）

- 问题描述
  - 求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。？
  - 1~13中包含1的数字有1、10、11、12、13因此共出现6次
- 问题思路
  - 设定整数点（如1、10、100等等）作为位置点i（对应n的各位、十位、百位等等），分别对每个数位上有多少包含1的点进行分析。
  - 根据设定的整数位置，对n进行分割，分为两部分，高位n/i，低位n%i
  - 当i表示百位，且百位对应的数>=2,如n=31456,i=100，则a=314,b=56，此时百位为1的次数有a/10+1=32（最高两位0~31），每一次都包含100个连续的点，即共有(a/10+1)*100个点的百位为1
  - 当i表示百位，且百位对应的数为1，如n=31156,i=100，则a=311,b=56，此时百位对应的就是1，则共有a/10(最高两位0-30)次是包含100个连续点，当最高两位为31（即a=311），本次只对应局部点00~56，共b+1次，所有点加起来共有（a/10*100）+(b+1)，这些点百位对应为1
  - 当i表示百位，且百位对应的数为0,如n=31056,i=100，则a=310,b=56，此时百位为1的次数有a/10=31（最高两位0~30）
  - 综合以上三种情况，当百位对应0或>=2时，有(a+8)/10次包含所有100个点，还有当百位为1(a%10==1)，需要增加局部点b+1
  - 之所以补8，是因为当百位为0，则a/10==(a+8)/10，当百位>=2，补8会产生进位位，效果等同于(a/10+1)
- 编程实现

```C++
    int NumberOf1Between1AndN_Solution(int n)
    {
        // 统计次数
        int count = 0;
        for(int i = 1; i <= n; i *= 10){
            // 计算高位和低位
            int a = n / i, b = n % i;
            count += (a + 8) / 10 * i + (a % 10 == 1) * (b + 1);
        }
        return count;
    }
```

### 1.9.6 丑数

- 问题描述
  - 把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。
- 问题思路
  - 创建一个数组，里面的数字是排好序的丑数，每一个丑数都是前面的丑数乘以2、3或者5得到的。
  - 怎样保证数组里面的丑数是排好序的。对乘以2而言，肯定存在某一个丑数T2，排在它之前的每一个丑数乘以2得到的结果都会小于已有最大的丑数，在它之后的每一个丑数乘以乘以2得到的结果都会太大。我们只需要记下这个丑数的位置，同时每次生成新的丑数的时候，去更新这个T2。对乘以3和5而言，也存在着同样的T3和T5。
- 编程实现

```
    int GetUglyNumber_Solution(int index) {
        if(index < 7){
            return index;
        }
        vector<int> res(index);
        for(int i = 0; i < 6; i++){
            res[i] = i + 1;
        }
        int t2 = 3, t3 = 2, t5 = 1;
        for(int i = 6; i < index; i++){
            res[i] = min(res[t2] * 2, min(res[t3] * 3, res[t5] * 5));
            while(res[i] >= res[t2] * 2){
                t2++;
            }
            while(res[i] >= res[t3] * 3){
                t3++;
            }
            while(res[i] >= res[t5] * 5){
                t5++;
            }
        }
        return res[index - 1];
    }
```

### 1.9.7 和为S的连续正数序列

- 问题描述
  - 有多少种连续的正数序列的和为S(至少包括两个数)
  - 输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序。
- 问题思路
  - 设定两个指针，一个指向第一个数，一个指向最后一个数,设定第一个数和最后一个数的值，由于是正数序列，所以可以把第一个数设为1，最后一个数为2.
  - 下一步就是不断改变第一个数和最后一个数的值，如果从第一个数到最后一个数的和刚好是要求的和，那么把所有的数都添加到一个序列中；如果大于要求的和，则说明从第一个数到最后一个数之间的范围太大，因此减小范围，需要把第一个数的值加1，同时把当前和减去原来的第一个数的值；如果小于要求的和，说明范围太小，因此把最后一个数加1，同时把当前的和加上改变之后的最后一个数的值。这样，不断修改第一个数和最后一个数的值，就能确定所有连续正数序列的和等于S的序列了。
  - 由于是连续的递增序列，可以利用求和公式：即sum = (a + b) * n / 2
- 编程实现

```C++
vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int> > result;
        // 高位指针和低位指针
        int phigh = 2, plow = 1;
        
        // 终止条件是phigh等于sum
        while(phigh > plow){
            // 当前和，使用求和公式s = (a+b) * n / 2
            int curSum = (plow + phigh) * (phigh - plow + 1) >> 1;
            if(curSum < sum){
                phigh++;
            }
            if(curSum == sum){
                vector<int> temp;
                for(int i = plow; i <= phigh; i++){
                    temp.push_back(i);
                }
                result.push_back(temp);
                plow++;
            }
            if(curSum > sum){
                plow++;
            }
        }
        return result;
    }
```





### 1.9.8 和为S的两个数字

- 问题描述
  - 输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。
- 问题思路
  - 定义两个指针，一个从左往右遍历（pleft），另一个从右往左遍历（pright）。首先，我们比较第一个数字和最后一个数字的和curSum与给定数字sum，如果curSum < sum，那么我们就要加大输入值，所以，pleft向右移动一位，重复之前的计算；如果curSum > sum，那么我们就要减小输入值，所以，pright向左移动一位，重复之前的计算；如果相等，那么这两个数字就是我们要找的数字，直接输出即可。
  - 这么做的好处是，也保证了乘积最小。
- 编程实现

```C++
vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        vector<int> result;
        int length = array.size();
        if(length < 1){
            return result;
        }
        int pright = length - 1;
        int pleft = 0;
        
        while(pright > pleft){
            int curSum = array[pleft] + array[pright];
            if(curSum == sum){
                result.push_back(array[pleft]);
                result.push_back(array[pright]);
                break;
            }
            else if(curSum < sum){
                pleft++;
            }
            else{
                pright--;
            }
        }
        return result;
    }
```





### 1.9.9 扑克牌顺子

- 问题描述
  - 一副扑克牌，随机从中抽出了5张牌，看看能不能抽到顺子
- 问题思路
  - 满足如下条件才可以认为是顺子：
    - 输入数据个数为5；
    - 输入数据都在0-13之间；
    - 没有相同的数字；
    - 最大值与最小值的差值不大于5。
  - 大小王可以当成任意数。
  - 利用一个flag记录每个数字出现的次数。
- 编程实现

```C++
	bool IsContinuous(vector<int> numbers ) {
        if(numbers.size() < 5){
            return false;
        }
        int max = -1, min = 14;
        int flag = 0;
        for(int i = 0; i < numbers.size(); i++){
            int curNum = numbers[i];
            if(curNum < 0 || curNum > 13){
                return false;
            }
            // 大小王，可以模拟任意数
            if(curNum == 0){
                continue;
            }
            
            // 如果数字出现了一次
            if((flag >> curNum) & 1 == 1){
                return false;
            }
            
            // 按位保存数字出现次数，比如0110表示，0出现0次，1出现1次，2出现1次，3出现0次。
            flag |= 1 << curNum;
            
            // 更新最小值
            if(curNum < min){
                min = curNum;
            }
            // 更新最大值
            if(curNum > max){
                max = curNum;
            }
            // 超过范围一定不是顺子
            if(max - min >= 5){
                return false;
            }
        }
        return true;
    }
```



### 1.9.10 孩子们的游戏（圆圈中最后剩下的数）

- 问题描述
  - 先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,求这个人的编号。
- 问题思路
  - 令f[i]表示i个人玩游戏报m退出最后胜利者的编号，最后的结果自然是f[n]。
  - 递推公式：
    - f[1]=0;
    - f[i]=(f[i-1]+m)%i; (i>1)
- 编程实现

```C++
	int LastRemaining_Solution(int n, int m) {
        if(n < 1 || m < 1){
            return -1;
        }
        int last = 0;
        for(int i = 2; i <= n; i++){
            last = (last + m) % i;
        }
        return last;
    }
```



### 1.9.11 求1+2+3+…+n

- 问题描述
  - 求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）
- 问题思路
  - 递归计算即可
- 编程实现

```C++
    int Sum_Solution(int n) {
        int ans = n;
        // &&就是逻辑与，逻辑与有个短路特点，前面为假，后面不计算。即递归终止条件
        ans && (ans += Sum_Solution(n - 1));
        return ans;
    }
```



### 1.9.12 不用加减乘除的加法

- 问题描述
  - 写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。
- 问题思路
  - 首先看十进制是如何做的： 5+7=12，
  - 可以使用三步走：
  - 第一步：相加各位的值，不算进位，得到2。
  - 第二步：计算进位值，得到10. 如果这一步的进位值为0，那么第一步得到的值就是最终结果。
  - 第三步：重复上述两步，只是相加的值变成上述两步的得到的结果2和10，得到12。
- 编程实现

```C++
    int Add(int num1, int num2) {
        return num2 ? Add(num1 ^ num2, (num1 & num2) << 1) : num1;
    }
```



### 1.9.13 字符流中第一个不重复的字符

- 问题描述
  - 请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。
  - 如果当前字符流没有存在出现一次的字符，返回#字符。
- 问题思路
  - 将字节流保存起来，通过哈希表统计字符流中每个字符出现的次数，顺便将字符流保存在string中，然后再遍历string，从哈希表中找到第一个出现一次的字符。
- 编程实现

```C++
public:
  //Insert one char from stringstream
    void Insert(char ch)
    {
        s += ch;
        count[ch]++;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        int length = s.size();
        for(int i = 0; i < length; i++){
            if(count[s[i]] == 1){
                return s[i];
            }
        }
        return '#';
    }
private:
    string s;
    int count[256] = {0};
```



### 1.9.14 数据流中的中位数

- 问题描述
  - 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
- 问题思路
  - 最大堆和最小堆；
  - 两个堆中数据的数目之差不能超过1；
  - 在数据的总数目是偶数时把新数据插入到最小堆中，否则插入到最大堆中；
  - 还要保证最大堆中所有数据小于最小堆中数据，新传入的数据需要先和最大堆的最大值或者最小堆中的最小值进行比较。
  - 基于stl中的函数push_heap、pop_heap以及vector实现堆。比较仿函数less和greater分别用来实现最大堆和最小堆。
- 编程实现

```C++
	// 使用vector建立最大堆和最小堆,min是最小堆数组,max是最大堆数组
    vector<int> min;
    vector<int> max;    
	
	void Insert(int num)
    {
        // 如果已有数据为偶数，则放入最小堆
        if(((max.size() + min.size()) & 1) == 0){
            // 如果插入的数字小于最大堆里的最大的数，则将数字插入最大堆
            // 并将最大堆中的最大的数字插入到最小堆
            if(max.size() > 0 && num < max[0]){
                // 插入数据插入到最大堆数组
                max.push_back(num);
                // 调整最大堆
                push_heap(max.begin(), max.end(), less<int>());
                // 拿出最大堆中的最大数
                num = max[0];
                // 删除最大堆的栈顶元素
                pop_heap(max.begin(), max.end(), less<int>());
                max.pop_back();
            }
            // 将数据插入最小堆数组
            min.push_back(num);
            // 调整最小堆
            push_heap(min.begin(), min.end(), greater<int>());
        }
        // 已有数据为奇数，则放入最大堆
        else{
            if(min.size() > 0 && num > min[0]){
                // 将数据插入最小堆
                min.push_back(num);
                // 调整最小堆
                push_heap(min.begin(), min.end(), greater<int>());
                // 拿出最小堆的最小数
                num = min[0];
                // 删除最小堆的栈顶元素
                pop_heap(min.begin(), min.end(), greater<int>());
                min.pop_back();
            }
            // 将数据插入最大堆
            max.push_back(num);
            push_heap(max.begin(), max.end(), less<int>());
        }
    }
    double GetMedian()
    {
        // 统计数据大小
        int size = min.size() + max.size();
        if(size == 0){
            return 0;
        }
        // 如果数据为偶数
        if((size & 1) == 0){
            return (min[0] + max[0]) / 2.0;
        }
        // 奇数
        else{
            return min[0];
        }
    }

    
```



### 1.9.15 滑动窗口的最大值

- 问题描述
  - 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}。
- 问题思路
  - 使用一个双端队列deque；
  - 可以用STL中的deque来实现，接下来我们以数组{2,3,4,2,6,2,5,1}为例，来细说整体思路。
  - 数组的第一个数字是2，把它存入队列中。第二个数字是3，比2大，所以2不可能是滑动窗口中的最大值，因此把2从队列里删除，再把3存入队列中。第三个数字是4，比3大，同样的删3存4。此时滑动窗口中已经有3个数字，而它的最大值4位于队列的头部。
  - 第四个数字2比4小，但是当4滑出之后它还是有可能成为最大值的，所以我们把2存入队列的尾部。下一个数字是6，比4和2都大，删4和2，存6。就这样依次进行，最大值永远位于队列的头部。
  - 但是我们怎样判断滑动窗口是否包括一个数字？应该在队列里存入数字在数组里的下标，而不是数值。当一个数字的下标与当前处理的数字的下标之差大于或者相等于滑动窗口大小时，这个数字已经从窗口中滑出，可以从队列中删除。
- 编程实现

```C++
    vector<int> maxInWindows(const vector<int>& num, unsigned int size)
    {
        vector<int> maxInWindows;
        // 数组大小要大于等于窗口大小，并且窗口大小大于等于1
        if(num.size() >= size && size >= 1){
            deque<int> index;
            for(unsigned int i = 0; i < size; i++){
                // 如果index非空，并且新添加的数字大于等于队列中最小的数字，则删除队列中最小的数字
                while(!index.empty() && num[i] >= num[index.back()]){
                    index.pop_back();
                }
                index.push_back(i);
            }
            for(unsigned int i = size; i < num.size(); i++){
                maxInWindows.push_back(num[index.front()]);
                while(!index.empty() && num[i] >= num[index.back()]){
                    index.pop_back();
                }
                // 控制窗口大小为size
                if(!index.empty() && index.front() <= int(i - size)){
                    index.pop_front();
                }
                index.push_back(i);
            }
            maxInWindows.push_back(num[index.front()]);
        }
        return maxInWindows;
    }
```



### 1.9.16 剪绳子

- 问题描述
- 问题思路
- 编程实现

```C++

```



### 1.9.17 n个骰子的点数

- 问题描述
- 问题思路
- 编程实现

```C++

```



### 1.9.18 股票的最大利润

- 问题描述
- 问题思路
- 编程实现

```C++

```

