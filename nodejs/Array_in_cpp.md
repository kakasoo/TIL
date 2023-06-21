# 의문점

> Cpp에서 Array와 Linked List는 다르다. Array는 고정된 크기를 가진다. Linked List는 length를 확장할 수 있다. 반면 자바스크립트 Array는 크기가 늘어난다. 그러면 자바스크립트 Array의 내부 구조(Cpp)는 Array가 아니다. 그렇다면 JavaScript Array는 Linked List로 구현되어 있나? 만약 Linked List라면 자바스크립트 Array에서 인덱스 i를 통한 랜덤 액세스의 시간 복잡도가 O(1)일 수는 없다. 이게 가능하려면 아마도 해시 맵일 텐데?

1. 자바스크립트 Array는 cpp에서는 TypedArray나 FixedArray 등 Array의 특징에 따라 다른 컨테이너로 생성된다.
2. 타입이 일정하면 메모리의 크기를 일정하게 할당할 수 있으므로 TypedArray로 할당한다.
3. 추후 타입이 바뀌거나 또는 타입이 다른 요소가 배열에 들어오게 되면 TypedArray는 다른 Array 타입으로 바뀐다.
    - 이 과정에서 데이터를 전부 새 컨테이너로 옮기는 작업을 해야 한다.
        - Q. 이런 과정을 하는 이유는?
            - 추론 : OS 레벨에서 paging, segmentation하듯이, 고작 컨테이너 레벨에서 메모리를 효율적으로 하기 위해서 인접하지 않은 메모리를 하나의 메모리인 것처럼 다루려면 파편화가 너무 많이 발생하기 때문일지도 모른다.
                - 결론 : 모든 요소가 같은 타입을 가지고 있을 경우에는 연속된 메모리, 다른 타입이 들어오면 해제 후 비연속적인 메모리로 재할당한다. - 그래서 나온 것이 ArrayBuffer와 SharedArrayBuffer.
                    - ArrayBuffer는 웹 워커끼리 공유가능하다.
                        - hashdos에 대해서 더 알아볼 것
4. 그래서 Linked List는 맞는가?
    - Linked List는 맞다.
    - 하지만 Linked List로 구현된 해시 맵이라고 해야 옳다. 해시 맵의 각 요소가 Linked List로 이루어져 있어서 중복된 해시 값일 경우에는 리스트에 담는다. 따라서 엄밀히 말해 O(1)이 아니다. 모든 값이 충돌일 경우에는 O(n)이 된다.

## Reference

[ystl 라이브러리에서의 vector](https://github.com/samchon/ystl/blob/master/src/mystl/vector.hpp)  
[EASTL에서의 hashmap 구현](https://github.com/electronicarts/EASTL/blob/master/include/EASTL/hash_map.h)
[TSTL에서 타입스크립트로 구현한 hash 함수](https://github.com/samchon/tstl/blob/master/src/functional/hash.ts#L16)
