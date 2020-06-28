学习笔记
学习笔记
Week1
1.本周学习了数组、列表、跳表以及栈、队列和双队列；

最大的感触就是在优化算法的时候，可以去考虑是否可以通过空间来换取时间复杂度的降维，来加速算法的速度。因为很多算法的改进都是通过空间换时间的思想进行优化和提高的。

数组：数组的插入、尾部插入和删除都是O(n)的时间复杂度，但是查询的时间复杂度为O(1)，通常来说，数组的头部插入的时间复杂度为O(n),但是可以通过相关的优化来使得头部插入的时间复杂度为O(1),即是通过开辟较大的空间，并在数组
的头部预留足够的空间以便插入使用。

列表：列表的插入、头部插入、尾部插入、删除都是O(1)的时间复杂度，但是查询的时间复杂度为O(n)，可以通过哈希来加速查找。

跳表：跳表对应的是平衡树(AVL Tree)和二分查找树，是一种插入、删除、搜索都是O(logn)的数据结构。其最大的优势就是原理简单，容易实现，方便拓展并且效率更高，因此在一些热门项目通常用来代替平衡树
跳表只能用在链表元素都是有序的情况下，跳表查询的时间复杂度为O(logn)

stack(栈)：是一种先入后出的数据结构，添加和删除时间复杂度都是O(1)
队列（quene):是一种先入先出的数据结构，添加和删除的时间复杂度都是O(1)
双端队列（deque):是综合了栈和队列优点的数据结构，既能够从头部压入数据和弹出数据，同时也能够从尾部压入和弹出数据
优先队列(Priority Quene):插入操作的时间复杂度为O(1)
                        取出的时间复杂度为O(logn)
                        底层具体实现的数据结构较为多样和复杂，可以用heap,bst,treap



2. add first 或 add last 这套新的API改写Deque的代码

Deque<String> deque = new LinkedList<String>();

deque.addLast("a");
deque.addLast("b");
deque.addLast("c");
System.out.printIn(deque);

String str = deque.peek();
System.out.printIn(str);
System.out.printIn(deque);

while(deque.size()>0) 
	System.out.printIn(deque.getLast());
System.out.printIn(deque);


3.分析Quene和Priority Quene的源码
Quene的源码：
#include <iostream>

using namespace std;

int queue[100], n = 100, front = -1, rear = -1;

void Insert() {
	int val;
	if (rear == n - 1) {
		cout << "Queue Overflow" << endl;
	else {
		if (front == -1) {
			front = 0;
			cout << "Insert the element in queue: " << endl;
			cin >> val;
			rear++;
			queue[rear] = val;
		}
	}
}

void Delete() {
	if (front == -1 || front > rear) {
		cout << "Queue Overflow" << endl;
		return;
	} else {
		cout << "Element deleted from queue is: “ << queue[front] << endl;
		front++;
	}
}

void Display() {
	if (front == -1) {
		cout << "Queue Overflow" << endl;
	} else {
		cout << "Queue element are: ";
		for (int i = front; i <= rear; ++i) {
			cout　<< queue[i] << " ";
		cout << endl;
	}
}

int main() {
	int ch;
	cout << " 1). Insert element to queue" << endl;
	cout << " 2). Delete element from queue" << endl;
	cout << " 3). Display all the element of queue" << endl;
	cout << " 4). Exit" << endl;

	do {
		cout << "Enter your choice: " << endl;
		cin >> ch;
		switch(ch) {
			case 1: Insert();
				break;
			case 2: Delete();
				break;
			case 3: Display();
				break;
			case 4: cout << "Exit:" << endl;
				break;
			default:
				break;
		}
	} while(ch != 4)
	return 0;
}


4.设计循环双端队列
#include <iostream>
#include <vector>

using namespace std;

class MyCircularDeque {

private:
    vector<int> arr;
    int front;
    int rear;
    int capacity;

public:
    /** Initialize your data structure here. Set the size of the deque to be k. */
    MyCircularDeque(int k) {
        capacity = k + 1;
        arr.assign(capacity, 0);

        front = 0;
        rear = 0;
    }

    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    bool insertFront(int value) {
        if (isFull()) {
            return false;
        }
        front = (front - 1 + capacity) % capacity;
        arr[front] = value;
        return true;
    }

    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    bool insertLast(int value) {
        if (isFull()) {
            return false;
        }
        arr[rear] = value;
        rear = (rear + 1) % capacity;
        return true;
    }

    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    bool deleteFront() {
        if (isEmpty()) {
            return false;
        }
        // front 被设计在数组的开头，所以是 +1
        front = (front + 1) % capacity;
        return true;
    }

    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    bool deleteLast() {
        if (isEmpty()) {
            return false;
        }
        // rear 被设计在数组的末尾，所以是 -1
        rear = (rear - 1 + capacity) % capacity;
        return true;
    }

    /** Get the front item from the deque. */
    int getFront() {
        if (isEmpty()) {
            return -1;
        }
        return arr[front];
    }

    /** Get the last item from the deque. */
    int getRear() {
        if (isEmpty()) {
            return -1;
        }
        // 当 rear 为 0 时防止数组越界
        return arr[(rear - 1 + capacity) % capacity];
    }

    /** Checks whether the circular deque is empty or not. */
    bool isEmpty() {
        return front == rear;
    }

    /** Checks whether the circular deque is full or not. */
    bool isFull() {
        // 注意：这个设计是非常经典的做法
        return (rear + 1) % capacity == front;
    }
};

作者：liweiwei1419
链接：https://leetcode-cn.com/problems/design-circular-deque/solution/shu-zu-shi-xian-de-xun-huan-shuang-duan-dui-lie-by/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


5.接雨水
int trap(vector<int>& height)
{
    int ans = 0;
    int size = height.size();
    for (int i = 1; i < size - 1; i++) {
        int max_left = 0, max_right = 0;
        for (int j = i; j >= 0; j--) { //Search the left part for max bar size
            max_left = max(max_left, height[j]);
        }
        for (int j = i; j < size; j++) { //Search the right part for max bar size
            max_right = max(max_right, height[j]);
        }
        ans += min(max_left, max_right) - height[i];
    }
    return ans;
}

