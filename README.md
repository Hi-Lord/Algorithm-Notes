# -记录日常算法刷题的笔记

## 数据结构篇

### 二叉树的遍历

#### <a href="#a1">前序遍历</a>

<a href="#a2">中序遍历</a>

<a href="#a3">后序遍历</a>

<a href="#a4">层次遍历</a>

<a href="#a5">直接插入排序</a>

<a href="#a6">折半插入排序</a>

<a href="#a7">希尔排序</a>

<a href="#a8">冒泡排序</a>

<a href="#a9">快速排序</a>

<a href="#a10">简单选择排序</a>

<a href="#a11">堆排序</a>

<a href="#a12">归并排序</a>

<a href="#a13">基数排序</a>

<a href="#a14">桶排序</a>



#### <a id="a1">1.前序遍历</a> 

##### 方法1：递归

算法思想：

1. 在当前结点非空时，访问当前节点，然后递归左子树、递归右子树
2. 递归出口：当前节点为空时，return

时间复杂度：o(n)

空间复杂度：平均o(log(n))	最坏o(n)

```c++
// 方法1：递归方法

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

 // 递归模式
class Solution {
public:
    void preorder(TreeNode* root, vector<int> &res) {
        if (root != nullptr) {
            res.push_back(root->val);
            preorder(root->left, res);
            preorder(root->right, res);
        } else {
            return;
        }
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        preorder(root, res);
        return res;
    }
};
```

**注意：**

- 空指针为nullptr 注意有返回值类型的函数，若想直接return作为递归出口是不可以的

##### 方法2：迭代

思想：

栈用于递归的关键点在于：栈底的元素具有记忆功能，即需要先存入后访问或者后面才对它及其相关元素进行操作时，可将这个元素先入栈，作为栈底元素。
使用递归对二叉树进行前序遍历的原理：先将根节点入栈，然后开始循环，循环条件为栈非空，而后栈顶元素（父节点）访问并出栈，同时让该元素的右子结点和左子结点依次进栈（先右后左，因为先序遍历是根左右，右结点后访问，因此要先入栈，放到栈底，等到左子树都访问完并出栈后，栈顶元素就是这个右子结点）。然后就结束了

时间复杂度：o(n)

空间复杂度：平均o(logn)	最坏o(n)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

// 迭代方法
// 栈用于递归的关键点在于：栈底的元素具有记忆功能，即需要先存入后访问或者后面才对它及其相关元素进行操作时，可将这个元素先入栈，作为栈底元素。
// 使用递归对二叉树进行前序遍历的原理：先将根节点入栈，然后开始循环，循环条件为栈非空，而后栈顶元素（父节点）访问并出栈，同时让该元素的右子结点和左子结点依次进栈（先右后左，因为先序遍历是根左右，右结点后访问，因此要先入栈，放到栈底，等到左子树都访问完并出栈后，栈顶元素就是这个右子结点）。然后就结束了

class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        // 建立栈
        stack<TreeNode*> stack;
        vector<int> res;

        if (root == nullptr) {
            return res;
        }

        stack.emplace(root);
        while (!stack.empty()) {
            root = stack.top(); // C++中想取得栈顶值，只能用top()，pop()只能弹出栈顶值，但不返回栈顶元素
            stack.pop();
            res.emplace_back(root->val);
            if (root->right != nullptr) {
                stack.emplace(root->right);
            }
            if (root->left != nullptr) {
                stack.emplace(root->left);
            }
        }
        return res;
    }
};
```

##### 方法3：Morris遍历

思想：前两种方法需要利用栈或者递归来实现从左子树的最右结点回到根节点的右子树，本算法的核心思想在于，利用左子树的最右节点空余的右指针，不断的指向根节点的右子树，从而节省空间开销，空间复杂度为o(1)

时间复杂度：o(n)

空间复杂度：o(1)

```C++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode *root) {
        vector<int> res;
        if (root == nullptr) {
            return res;
        }

        TreeNode *p1 = root, *p2 = nullptr;

        while (p1 != nullptr) {
            p2 = p1->left;
            if (p2 != nullptr) {
                while (p2->right != nullptr && p2->right != p1) {
                    p2 = p2->right;
                }
                if (p2->right == nullptr) {
                    res.emplace_back(p1->val);
                    p2->right = p1;
                    p1 = p1->left;
                    continue;
                } else {
                    p2->right = nullptr;
                }
            } else {
                res.emplace_back(p1->val);
            }
            p1 = p1->right;
        }
        return res;
    }
};
```

#### <a id="a2">2.中序遍历</a> 

##### 方法1：递归法

原理同前序遍历

时间复杂度：o(n)

空间复杂度：平均o(log(n))	最坏o(n)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
 // 递归
 // （非空判断）{递归左子树，访问根节点，递归右子树}
class Solution {
public:
    void inorder(TreeNode* root, vector<int>& vec) {
        if (root != nullptr) {
            inorder(root->left, vec);
            vec.push_back(root->val);
            inorder(root->right, vec);
        }
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> vec;
        if (root == nullptr) {
            return vec;
        }

        inorder(root, vec);
        return vec;
    }
};
```

##### 方法2：迭代法

原理：(结点非空或栈非空时){若有左子树，则一直入栈，没有左子树时，就访问并出栈，同时右子树入栈}

时间复杂度：o(n)

空间复杂度：平均o(logn)	最坏o(n)

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
 // 方法2：迭代
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stc;
        TreeNode * p = root;
        if (p == nullptr) {
            return res;
        }
        while (p!=nullptr || !stc.empty()) {
            // 如果有左子树，就一直往下入栈
            if (p!=nullptr) {   // 这里的if也可以换成while，其实都是一样的
                stc.push(p);
                p = p->left;
            } else{
                // 若没有左子树就出栈，访问,同时让右子树入栈
                p = stc.top();
                res.push_back(p->val);
                stc.pop();
                p = p->right;
                }
        }
        return res;
    }
};
```

#### <a id="a3">3.后序遍历</a> 

##### 方法1：递归法

原理同前序

时间复杂度：o(n)

空间复杂度：平均o(log(n))	最坏o(n)

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

 // 方法1：递归法
class Solution {
public:
    void postorder(TreeNode * root, vector<int>& vec) {
        if (root != nullptr) {
            postorder(root->left, vec);
            postorder(root->right, vec);
            vec.push_back(root->val);
        }
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;

        if (root == nullptr) {
            return res;
        }

        postorder(root, res);
        return res;
    }
};
```

##### 方法2：迭代法

原理：根结点入栈后，按照先右后左的顺序入栈（为了防止出栈时栈顶元素再次将左右子树入栈，所以要将入栈左右子结点后，将其父结点的左右指针置空），同时将指针指向每次循环最后入栈的结点，当栈顶元素无子树时，再出栈访问

时间复杂度：o(n)

空间复杂度：平均o(logn)	最坏o(n)

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

 // 方法2：迭代法
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        stk.push(root);
        

        if (root == nullptr) {
            return res;
        }

        // 先将根节点入栈，然后开启循环，按照右子树、左子树的顺序入栈（为了防止出栈时栈顶元素再次将左右子树入栈，所以要将入栈左右子结点后，将其父结点的左右指针置空），若栈顶元素无子树的情况下就出栈、访问，
        while(!stk.empty()) {

            if (root->right != nullptr){
                stk.push(root->right);
                root->right = nullptr;
            }
            if(root->left != nullptr) {
                stk.push(root->left);
                root->left = nullptr;
            }
            // 这是最关键的一步，我们的目的就是将指针指向每个循环最后入栈的结点，那么直接取栈顶元素是最准确的，指错元素的话会产生很多逻辑错误
            root = stk.top();
            if (root->right == nullptr && root->left == nullptr){
                root = stk.top();
                res.push_back(root->val);
                stk.pop();
            }
            
        }
        return res;
            
    }
};
```

#### <a id="a4">4.层次遍历</a> 

本题与普通层次遍历不同，要求将同一层的结点全部保存到一个vector中，最终返回一个vector<vector<int>>

思想：使用队列，每入队列一个结点，便将其左右子结点入队

本题需要将每层结点独立输出为一个vector，故引入计数num，计算每层的结点个数，以这个个数构建num次循环，一个循环内将当前层全部输出，并保存到一个vector内，循环结束后初始化本

时间复杂度：o(n)

空间复杂度：o(n)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> q;

        if(root == nullptr) {
            return result;
        }

        q.push(root);
        while(!q.empty()) {
            vector<int> res;
            // 本题需要将每层结点独立输出为一个vector，故引入计数num，计算每层的结点个数，以这个个数构建num次循环，一个循环内将当前层全部输出，并保存到一个vector内，循环结束后初始化本
            int num = q.size();
            for (int i = 0; i < num; i++) {
                root = q.front();
                res.push_back(root->val);
                q.pop();
                if (root->left != nullptr) {
                    q.push(root->left);
                }
                if (root->right != nullptr) {
                    q.push(root->right);
                }
            }
            result.push_back(res);
            
        }
        return result;
    }
};
```

### 排序算法

#### 1.插入排序

##### <a id="a5">1.1直接插入排序</a> 

在leetcode中直接插入排序会超出时间限制

时间复杂度：0(n2)	o(n2)

空间复杂度：o(1)

稳定性：稳定

```C++
// 方法1：直接插入排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        for (int i = 1; i < len; i++) {
            int t = nums[i];
            int j = i;
            for (; j > 0 && t < nums[j-1]; j--) { //  注意循环的条件判断语句，不能t < nums[j]在前，j >= 0在后，因为当j==0时，循环结束后j==-1，此时其实是不满足循环条件的，但若是t < nums[j]在前，则是需要取出nums[-1]的值判断是否>t的，而nums没有-1下标，所以会报错！！！
                nums[j] = nums[j-1];
            }
            nums[j] = t;
        }
        return nums;
    }
};
```

##### <a id="a6">1.2折半插入排序</a> 

相较直接插入排序减少了寻找待插入位置的比较次数，而移动次数没有改变

在leetcode中直接插入排序会超出时间限制

时间复杂度：0(n2)	o(n2)

空间复杂度：o(1)

稳定性：稳定

```C++
// 折半插入排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        for (int i = 1; i < len; i++) {
            int temp = nums[i];
            // 寻找待插入元素在前面已排序序列中该插入的位置
            int low, high;
            low = 0, high = i-1;
            while(low <= high) {
                int mid = (low + high) / 2;
                if (temp < nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            }
            // 前面已排序序列后移
            // 此处不能用high代替low，因为第14行high = mid - 1;导致high可能为-1，导致报错
            for (int k = i; k > low; k--) {
                nums[k] = nums[k-1];
            }
            nums[low] = temp;
        }
        return nums;
    }
};
```



##### <a id="a7">1.3希尔排序</a> 

将数组按照步长分为多个子序列，对子序列进行直接插入排序，而后将步长缩短，继续对子序列排序，知道步长为1

要注意循环条件！！！

时间复杂度：0(n1.3)	o(n2)

空间复杂度：o(1)

稳定性：不稳定

```C++
// 希尔排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        for (int step = len/2; step >0; step/=2) {
            for (int i = 0; i < step; i++) {
            
                for (int j = i+step; j < len; j += step) {
                    int temp = nums[j];
                    int k=j;
                    for (k = j;k >= step && temp < nums[k-step] ;k-=step) { // 注意这里循环条件应该是k >= step而不是k>=0，k>=0时，当k执行到每个希尔子序列的第一个元素时依然可以进入循环，则nums[k-step]就会出现下标<0 的情况
                        nums[k] = nums[k-step];
                    }
                    nums[k] = temp;
                }
            }
        }
        return nums;
    }
};
```

#### 2.交换排序

##### <a id="a8">2.1 冒泡排序</a> 

时间复杂度：0(n2)	o(n2)

空间复杂度：o(1)

稳定性：稳定

```C++
// 冒泡排序
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        for (int i = 0; i < len - 1 ; i++) {
            for (int j = 0; j < len - i - 1; j++) {
                if (nums[j+1] < nums[j]) {
                    int temp = nums[j+1];
                    nums[j+1] = nums[j];
                    nums[j] = temp;
                }
            }
        }
        return nums;
    }
};
```



##### <a id="a9">2.2 快速排序</a> 

快速排序的普通版，在特殊情况下时间复杂度会降为o(n2)，导致leetcode超时

时间复杂度：0(nlog2n)	o(n2)

空间复杂度：o(log2n)	o(n)

稳定性：不稳定

```C++
// 快速排序
class Solution {
public:

    // 每执行一次，可以确定一个枢轴元素的最终位置
    int step01(vector<int>& nums, int low, int high) {
        // 直接使用第一个元素作为枢轴元素，也可采用随机元素
        int temp = nums[low];
        while (low < high) {
            // 从右向左一直找到比枢轴值小的元素，然后将其移动到枢轴元素的临时位置
            while (low < high && nums[high] >= temp) {
            --high;
            }
            // 其实应该交换，但未找到枢轴元素的最终位置时，可以先不交换，而是只移动其他元素，最后再将枢轴元素移动到最终位置，以提高运行效率
            nums[low] = nums[high];
            // 其实可以不判断，以提高效率
            /*if (nums[high] < temp) {
                nums[low] = nums[high];
            }*/
            // 从左向右一直找到比枢轴值大的元素，然后将其移动到枢轴元素的临时位置
            while (low < high && nums[low] <= temp) {
                ++low;
            }
            nums[high] = nums[low];
            /*
            if (nums[low] > temp) {
                nums[high] = nums[low];
            }*/
        }
        // 最后将枢轴值放到最终位置
        nums[low] = temp;
        return low;
    }

    void step02(vector<int>& nums, int low, int high) {
        if (low >= high) {
            return;
        } else {
            int a = step01(nums, low, high);
            step02(nums, low, a-1);
            step02(nums, a+1, high);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        int low = 0;
        int high = len-1;
        step02(nums, low, high);
        return nums;
    }
};
```



#### 3.选择排序

##### <a id="a10">3.1简单选择排序</a> 

在leetcode中超时

时间复杂度：0(n2)

空间复杂度：o(1)

稳定性：不稳定	如L={2,2,1}第一趟排序后变为{1,2,2}，原来的第一个2与1交换后跑到了第二个2的后面

```C++
// 简单选择排序
class Solution {
public:
    void swap(vector<int>& nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        for (int i = 0; i < len-1; i++) {
            int min = i;
            for (int j = i+1; j < len; j++) {
                if (nums[j] < nums[min]) {
                    min = j;
                }
            }
            swap(nums, i, min);
        }
        return nums;
    }
};
```



##### <a id="a11">3.2堆排序</a> 

时间复杂度：0(nlog2n)	o(nlog2n)

空间复杂度：o(1)

稳定性：不稳定

```c++
// 堆排序
class Solution {
public:
    void swap(vector<int>& nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    // 对一个父节点循环向下调整
    void adjust(vector<int>& nums, int k, int len) {
        while (k < len && 2*k+1 <len) {
            int j = 2*k+1;
            if (j+1 < len && nums[j+1] > nums[j]) {
                j++;
            }
            if (nums[k] < nums[j]) {
                swap(nums, k, j);
            } else {
                break;
            }
            k = j;
        }
    }

    // 对一整个数循环调用调整函数
    void buildHeap(vector<int>& nums, int len) {
        for (int i = len/2-1; i >= 0; i--) {
            adjust(nums, i, len);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        int len = nums.size();
        buildHeap(nums, len);
        for (int i = len-1; i>=0; i--) {
            swap(nums,i,0);
            adjust(nums, 0, i);
        }
        return nums;
    }
};
```

#### 4. 其他排序

##### <a id="a12">4.1 归并排序</a> 

就是合并子序列，每个子序列都是有序的，（递归实现）。

先写一个合并两个有序数组的函数，然后在主函数里递归调用，实现从  由多个单元素构成的数组  合并成一个有序数组。

相较于快速排序，增加了部分空间复杂度，换来了稳定性。

**注意点：**

- 要注意复制数组复制原数组的时机，是在每次做数组融合的时候重新复制，不能在第一个主函数里就复制，因为原数组nums一直在变，而复制数组是在已改变的原数组的基础上递归做融合的
- 同时可以在每次复制数组时只复制low到high之间的元素，以减少运行时间

时间复杂度：0(nlog2n)

空间复杂度：o(n)

稳定性：稳定

```C++
// 归并排序
class Solution {
public:
    vector<int> temp;
    void merge(vector<int>& nums, int low, int mid, int high) {
        // 要注意复制数组复制原数组的时机，是在每次做数组融合的时候重新复制，不能在第一个主函数里就复制，因为原数组nums一直在变，而复制数组是在已改变的原数组的基础上递归做融合的
        // 同时可以在每次复制数组时只复制low到high之间的元素，以减少运行时间
        for (int t = low; t <= high; t++) {
            temp[t] = nums[t];
        }
        int i, j, k;
        // 循环直到某一个数组已全部放到新数组里
        for (i = low, j = low, k = mid + 1; i <= high && j <= mid && k <= high; i++){
            if (temp[j] <= temp[k]) {
                nums[i] = temp[j];
                j++;
            } else {
                nums[i] = temp[k];
                k++;
            }
        }
        // 将未全部放到新数组的数组的剩余部分直接复制到新数组里
        while(j <= mid) {
            nums[i++] = temp[j++];
        }
        while(k <= high) {
            nums[i++] = temp[k++];
        }
    }

    void mergeSort(vector<int>& nums, int low, int high) {
        
        if (low < high) {
            int mid = (low+high) / 2;
            mergeSort(nums,low,mid);
            mergeSort(nums,mid+1,high);
            merge(nums,low, mid, high);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        temp.resize(nums.size(), 0);
        int low, mid, high;
        low = 0;
        high = nums.size()-1;
        mergeSort(nums, low, high);
        return nums;
    }
};
```



##### <a id="a13">4.2 基数排序</a> 

例如一堆三位数，依次利用其个位数、十位数、百位数进行从小到大的排序即可

时间复杂度：0(d*(n+r))

空间复杂度：o(r)

稳定性：稳定

##### <a id="a14">4.3 桶排序</a> 

观察待排元素的取值范围，分成几个区间，将每个元素分配到对应的桶内，再对各个桶进行排序
