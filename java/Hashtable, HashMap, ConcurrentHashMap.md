## HashMap vs Hashtable vs ConcurrentHashMap

> `HashMap` vs `Hashtable` , `ConcurrentHashMap` </br>

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
    ==>  testKey2

```

</br>

#### `HashMap을 사용하는 이유`
--- 
HashMap은 이름 그대로 해싱(Hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는 데 있어서 뛰어난 성능을 보인다. 만약 HashMap을 사용하지 않고 List를 사용하여 원소를 검색한다면 O(n)의 시간복잡도 이지만, HashMap을 사용한다면 O(1)의 시간복잡도를 기대할 수 있다.

</br>

### `HashMap` 구조
---
|<img src="https://github.com/dongy094/knowledge-for-developer/blob/main/java/img/hashMap.png?raw=true">|
HashMap은 내부에 `key`와 `value`을 저장하는 자료 구조를 가지고 있다. `HashMap`은 `해시 함수`를 통해 `key`와 `value`이 저장되는 위치를 결정하므로, 사용자는 그 위치를 알 수 없고, 삽입되는 순서와 들어 있는 위치 또한 관계가 없다. 