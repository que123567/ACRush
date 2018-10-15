### Vnode.hpp

```js
namespace cse250 {
    template <typename T>
    struct vNode;
}

template <typename T>
struct cse250::vNode {
    std::vector<T> data;
    cse250::vNode<T>* next;
    cse250::vNode<T>* prev;
    vNode() : next(nullptr), prev(nullptr) {}
};
```

### LinkedVector.hpp

```js
#ifndef _LINKED_VECTOR_HPP_
#define _LINKED_VECTOR_HPP_

#include "vNode.hpp"
#include<cmath>
#include <algorithm>


template<typename T>
class LinkedVector {
    cse250::vNode<T> *head;
    //values' size of LinkedVector
    int n = 0;

    //nodes' (length - 1)  of LinkedVector
    int k = 0;

    //the length of LinkedVector
    int length = k + 1;

    //the max number of current node
    int max_size = 10 * (k + 1);

    //
    std::vector<T> exist_value;


public:
    /**
     * LinkedVector Iterator class.
     */
    class Iterator;

    /**
     * Constructor: construct an empty LinkedVector.
     */
    LinkedVector();

    /**
     * Destructor: cleanup any allocated memory.
     */
    ~LinkedVector();

    /**
     * Insert the given value in increasing order.
     * @param value the value to insert.
     * @return true if value is inserted and false if it is already present.
     */
    bool insert(const T &value);

    /**
     * Find the given value.
     * @param value the value to find.
     * @return an iterator at the value, if present, and end if not present.
     */
    LinkedVector<T>::Iterator find(const T &value) const;

    /**
     * Erase a value from the data storage.
     * @param value the value to remove.
     * @return true if the value was removed and false otherwise.
     */
    bool erase(const T &value);

    /**
     * Return the number of items currently stored.
     * @return the number of items currently stored.
     */
    std::size_t size() const;

    /**
     * Create an Iterator to the beginning of the stored sequence.
     * @return an Iterator to the beginning of the sequence.
     */
    LinkedVector<T>::Iterator begin() const;

    /**
     * Create an Iterator to just past the end of the sequence.
     * @return an Iterator that represents the position just past the end
     *         of the sequence.
     */
    LinkedVector<T>::Iterator end() const;


    std::vector<int> calculate(int n, int size);

private:
    /**
     * Rebalance the loaded values within each node to satisfy the necessary
     * load-balance criteria.
     */
    void rebalance();
};//LinkedVector


template<typename T>
LinkedVector<T>::LinkedVector() {
    head = new cse250::vNode<T>;
    exist_value.reserve(2000000);
}


template<typename T>
LinkedVector<T>::~LinkedVector() {
    auto tmp = head->next;
    while (tmp != nullptr) {
        auto deleteP = tmp;
        tmp = tmp->next;
        delete deleteP;
    }
    delete head;
    exist_value.clear();
}


template<typename T>
bool LinkedVector<T>::insert(const T &value) {
    //判重
    for (T a:exist_value) {
        if (a == value)
            return false;
    }
    exist_value.push_back(value);
    //只有一个节点 && 数小于10
    if (head->next == nullptr && head->data.size() < 10 * (k + 1)) {
        head->data.push_back(value);
        sort(head->data.begin(), head->data.end());
        return true;
    }
    //只有一个节点 && 容量超量
    if (head->next == nullptr && head->data.size() >= 10 * (k + 1)) {
        head->data.push_back(value);
        sort(head->data.begin(), head->data.end());
        rebalance();
        return true;
    }
//    大于一个节点
    auto curNode = head;
    bool should_rebalance = false;
    if (curNode->next != nullptr) {
        //空节点||当前节点都小于value
        while (curNode->next != nullptr &&
               (curNode->data.size() == 0 || curNode->data[curNode->data.size() - 1] < value)) {
            curNode = curNode->next;
        }
        //前一条size()==0 || 前一条的size()较小
        int now_prev_size = 0;
        if (curNode->prev != nullptr) {
            now_prev_size = curNode->prev->data.size();
        }
        int now_cur_size = curNode->data.size();

        if ((now_prev_size == 0 || now_prev_size < now_cur_size || curNode->prev->data[now_prev_size - 1] < value) &&
            curNode->data[now_cur_size - 1] > value && curNode->prev != nullptr) {
            curNode->prev->data.push_back(value);
            return true;
        } else {
            curNode->data.push_back(value);
            if (curNode->data[curNode->data.size() - 1] > value) {
                should_rebalance = true;
            }
        }
    }
    if (curNode->data.size() > max_size || (curNode->next != nullptr && curNode->next->data.size() > max_size) ||
        should_rebalance) {
        rebalance();
    }
    return true;
}


template<typename T>
typename LinkedVector<T>::Iterator LinkedVector<T>::find(const T &value) const {
    // TODO: FINISH THIS METHOD
    LinkedVector<T>::Iterator retVal(this);
    while (*retVal != value && retVal != end()) {
        ++retVal;
    }
    if (retVal == end() && *retVal != *end()) {
        ++retVal;
    }
    return retVal;
}

template<typename T>
bool LinkedVector<T>::erase(const T &value) {
    auto tmpHead = head;
    bool mark = false;
    while (tmpHead != nullptr) {
        for (auto i = tmpHead->data.begin(); i < tmpHead->data.end(); ++i) {
            if (*i == value) {
                tmpHead->data.erase(i);
                mark = true;
            }
        }
        tmpHead = tmpHead->next;
    }
    for (auto i = exist_value.begin(); i < exist_value.end(); ++i) {
        if (*i == value) {
            exist_value.erase(i);
            mark = true;
        }
    }
    return mark;
}

template<typename T>
std::size_t LinkedVector<T>::size() const {
    return exist_value.size();
}


template<typename T>
std::vector<int> LinkedVector<T>::calculate(int size, int n) {
    float ceil = ceilf(size * 1.0 / n);
    float avrg = size * 1.0 / n;
    float variance;

    std::vector<int> ivec;
    std::vector<std::vector<int>> res;

    for (int i = 0; i < n + 1; i++) {
        for (int i = 0; i < n; i++) {
            ivec.push_back(ceil);
        }
        for (int k = 0; k < i; k++) {
            ivec[n - k - 1] = ceil - 1;
        }
        int total = 0;
        for (int i: ivec) {
            total += i;
            if (total == size) {
                res.push_back(ivec);
            }
        }
        ivec.clear();
    }
    float min_dis = MAXFLOAT;
    for (std::vector<int> vector:res) {
        for (int i:vector) {
            variance += (i - avrg) * (i - avrg);
        }
        if (min_dis > variance) {
            ivec = vector;
            min_dis = variance;
        }
        variance = 0;
    }
    return ivec;
}

template<typename T>
typename LinkedVector<T>::Iterator LinkedVector<T>::begin() const {
    // TODO: FINISH THIS METHOD
    LinkedVector<T>::Iterator retVal(this);
    return retVal;
}

template<typename T>
typename LinkedVector<T>::Iterator LinkedVector<T>::end() const {
    // TODO: FINISH THIS METHOD
    LinkedVector<T>::Iterator retVal(this);
    for (int i = 0; i < size() - 1; i++) {
        ++retVal;
    }
    return retVal;
}

template<typename T>
void LinkedVector<T>::rebalance() {
    auto tmp = head;
    std::vector<T> tmpVec;

    n = 0;
    //存储数据
    while (tmp != nullptr) {
        n += tmp->data.size();
        for (int i = 0; i < tmp->data.size(); ++i) {
            tmpVec.push_back(tmp->data.at(i));
        }
        tmp = tmp->next;
    }
    //计算新长度
    k = floor(sqrt(n));//VL length
    length = k + 1;

    //创建新节点
    tmp = head->next;
    while (tmp != nullptr) {
        auto deleteP = tmp;
        tmp = tmp->next;
        delete deleteP;
    }

    tmp = head;
    for (int i = 0; i < length - 1; ++i) {
        cse250::vNode<T> *nodeNext = new cse250::vNode<T>;
        tmp->next = nodeNext;
        nodeNext->prev = tmp;
        tmp = tmp->next;//iterator next
    }
    //更新max_size
    max_size = 10 * (k + 1);

    tmp = head;
    std::vector<int> avrg_vec = calculate(n, length);
    int tmp_avrg;
    int total_i = 0;
    //节点里插入数据
    for (int j = 0; j < length; ++j) {
        tmp_avrg = avrg_vec[j];
        tmp->data.clear();
        for (int i = 0; i < tmp_avrg; ++i) {
            tmp->data.push_back(tmpVec[total_i + i]);
        }
        total_i += tmp->data.size();
        tmp = tmp->next;
    }
}


/**
 * Iterator class for the LinkedVector.
 *
 * Suggested: define Iterator functionality within class definition.
 */
template<typename T>
class LinkedVector<T>::Iterator {
    const LinkedVector<T> *associatedContainer;
    const T *position;
    std::vector<T> copy_ivec;

    /**
     * Default constructor for Iterator is made private since we cannot have an
     * iterator without an associated container.
     */
    Iterator() {}

public:
    /**
     * Constructor: setup iterator.
     */
    Iterator(const LinkedVector<T> *container) {
        // TODO: FINISH THIS METHOD
        associatedContainer = container;
        for (T i:container->exist_value) {
            copy_ivec.push_back(i);
        }
        sort(copy_ivec.begin(), copy_ivec.end());
        position = copy_ivec.data();
    }

    /**
     * Destructor: cleanup iterator. Remove if not used.
     */
    ~Iterator() {
        // TODO: FINISH THIS METHOD OR DELETE IT
    }

    /**
     * Return a copy of the value at the position held by the
     * iterator.ac
     *
     *
     * @return a copy of the value at the position held by the iterator.
     */
    T operator*() {
        return *(this->position);
    }

    /**
     * Prefix operator++
     * @return reference to this after incrementing position.
     */
    Iterator &operator++() {
        // TODO: FINISH THIS METHOD
        ++position;
        return (*this);
    }

    /**
     * Postfix operator++
     * @return copy of iterator before incrementing position.
     */
    Iterator operator++(int) {
        // Don't change this code.
        Iterator copy(*this);
        ++(*this);
        return copy;
    }

    /**
     * Compare equality of two iterators.
     * @param rhs is an iterator being compared with this iterator.
     * @return true if they represent the same position in the same container and false otherwise.
     */
    bool operator==(const Iterator &rhs) const {
        // TODO: FINISH THIS METHOD
        if (this->associatedContainer == rhs.associatedContainer) {
            if (*this->position == *rhs.position) {
                return true;
            }
        }
        return false;
    }

    /**
     * Compare inequality of two iterators.
     * @param rhs is an iterator being compared with this iterator.
     * @return false if they represent the same position in the same container and true otherwise.
     */
    bool operator!=(const Iterator &rhs) const {
        return !(*this == rhs);
    }
};//LinkedVector::Iterator


#endif //_LINKED_VECTOR_HPP_

```



