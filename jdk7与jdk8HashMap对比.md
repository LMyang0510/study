## JDK7与JDK8 HashMap 对比
### 1. HashMap简介
    HashMap可以理解为一种存储k-v形式数据的一个容器，底层采用了数组+链表的组合方式，
    存储的数组我们一般又称为哈希表或散列表，经过散列函数f(k)后结果相同的称之为冲突，以链表的形式进行解决。

### 2. 哈希表长度元素定位
    为保证元素能尽可能的在哈希表上均匀分布，不超出哈希表范围，且能够高效定位，
    规定哈希表长度必须为2的n次方，利于散列结果对哈希表长度-1(二进制全是1)进行与运算，
    这样任意散列结果对哈希表长度-1进行与运算结果都会在【0，哈希表长度)范围内，
    然后对散列函数进行特殊处理，就能尽可能的让元素在哈希表上均匀分布。
      0000000    1111111
    & 0111111  & 0111111
    ---------  ---------
      0000000    0111111

### 3. JDK7 HashMap 实现
1. 散列函数
```text
// 对hashCode二次散列
h = key.hashCode();
h ^= (h >>> 20) ^ (h >>> 12); 
h ^ (h >>> 7) ^ (h >>> 4);
```
2. 哈希表index定位
```text
// 利用位运算符进行高效定位
h & (length-1)
```
3. 冲突解决
```text
// 原哈希表数据
Entry[] src = table;
// 扩容后大小（2倍于原哈希表长度，但不超过Integer.MAX_VALUE）
int newCapacity = newTable.length;
// 循环遍历哈希表元素
for (int j = 0; j < src.length; j++) {
    Entry<K,V> e = src[j];
    if (e != null) {
        src[j] = null;
        // 循环遍历链表元素
        do {
            Entry<K,V> next = e.next;
            // 将散列结果重新 按 新哈希表长度进行 index 定位
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];
            newTable[i] = e;
            e = next;
        } while (e != null);
    }
}
```
4. 扩容机制

### 4. JDK8 HashMap实现
1. hash散列
2. index定位
3. 冲突解决
4. 扩容机制