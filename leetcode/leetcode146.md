出现在:字节跳动,腾讯,阿里等

LRU缓存机制 \n
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。
获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。\n
练习地址:https://leetcode-cn.com/problems/lru-cache/

put操作时:
1,如果不在原链表中,并且链表长度小于LRU容量时,直接放入链表头部;如果链表长度等于LRU容量时,移除链表最后一个节点,然后将新节点放入链表头部;
2,如果在原链表中,将该节点移动到链表头部

get操作时:
1,如果在链表中,将该节点移动到链表头部并返回;
2,如果不在链表中,返回-1

```
class LRUCache {

    private LinkedList<Integer> list;
    private HashMap<Integer,Integer> map;
    private int capacity;
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        list = new LinkedList<>();
        map = new HashMap<>();
    }
    
    public int get(int key) {
        if(map.containsKey(key)){
            list.removeFirstOccurrence(key);
            list.addFirst(key);
            return map.get(key);
        }else{
            return -1;
        }
    }
    
    public void put(int key, int value) {
        if(!map.containsKey(key)){
            if(list.size() == capacity){
                int last = list.removeLast();
                map.remove(last);
            }
            list.addFirst(key);
            
        }else{
            list.removeFirstOccurrence(key);
            list.addFirst(key);
        }
        map.put(key,value);
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
