## HashMap

</br>

### `HashMap이란`
---
`HashMap`은 `Map`인터페이스를 구현하는 클래스이다. `Map` 인터페이스를 상속하고 있기에 `Map`의 성질을 그대로 가지고 있다.
Map은 키와 값으로 구성된 Entry객체를 저장하는 구조를 가지고 있는 자료구조이며 여기서 `key`와 `value`는 모두 객체이다.
HashMap에 Key가 중복되어 사용된다면, 마지막으로 put된 value값이 key값으로 등록이된다.
```java
    hash = new HashMap<String, String>();

    hash.put("key","testKey1");
    hash.put("key","testKey2");
    System.out.print("key 값 : " + hash.get("key"));
    출력결과 ==>  testKey2

```

</br>

#### `HashMap을 사용하는 이유`
--- 
HashMap은 이름 그대로 해싱(Hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다. 만약 HashMap을 사용하지 않고 List를 사용하여 원소를 검색한다면 O(n)의 시간복잡도 이지만, HashMap을 사용한다면 O(1)의 시간복잡도를 기대할 수 있다.

</br>

### `HashMap` 구조
---
<img src="https://github.com/dongy094/knowledge-for-developer/blob/main/java/img/hashMap.png?raw=true">

<br>

HashMap은 내부에 `key`와 `value`을 저장하는 자료 구조를 가지고 있다. `HashMap`은 `해시 함수`를 통해 `key`와 `value`이 저장되는 위치를 결정하므로, 사용자는 그 위치를 알 수 없고, 삽입되는 순서와 들어 있는 위치 또한 관계가 없다. 

</br>

### `HashMap`과 `HashTable`의 차이점
---
- `Hashtable`은 key에 null을 허용하지 않지만, `HashMap`은 key에 null을 허용한다.
- `HashTable`은 동기화를 지원하여 `Thread-safe`하다. 
```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {

    public synchronized int size() { }

    @SuppressWarnings("unchecked")
    public synchronized V get(Object key) { }

    public synchronized V put(K key, V value) { }
}
```
- `Hashtable` 클래스를 보면 위와 같이 메소드 전체에 `synchronized` 키워드가 존재하는 것을 볼 수 있다.
- `HashTable`은 멀티스레드 환경에서 사용하기 좋은 자료구조이지만, `HashMap`에 비해 느리다.(다른 스레드가 block되고 unblock되는 대기 시간을 기다리기 때문이다.)

<br>

- `HashMap`은 동기화를 지원하지 않는다. `HashMap`은 단일스레드 환경에서 사용하기 좋은 자료구조이다.
```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {

    public V get(Object key) {}
    public V put(K key, V value) {}
}
```
- 멀티 쓰레드 환경이 아니라면 `HashMap`을 사용하기에 대체적으로 적합하겠지만, 멀티쓰레드의 환경이라면 `HashMap` 클래스도 `Hashtable` 클래스의 대안이 될 수는 없다.

<br>

### `ConcurrentHashMap`
---
`Hashtable` 클래스의 단점을 보완하면서 Multi-Thread 환경에서 사용할 수 있도록 나온 클래스인 `ConcurrentHashMap`
```java
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
    implements ConcurrentMap<K,V>, Serializable {

    public V get(Object key) {}

    public boolean containsKey(Object key) { }

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key,
                                                              value, null);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }
}
```
- `ConcuurentHashMap`에는 `Hashtable` 과는 다르게 `synchronized` 키워드가 메소드 전체에 붙어 있지 않습니다. `get()` 메소드에는 아예 `synchronized`가 존재하지 않고, `put()` 메소드에는 중간에 `synchronized` 키워드가 존재하는 것을 볼 수 있다.