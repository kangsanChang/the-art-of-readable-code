# 1. 코드는 이해하기 쉬워야 한다

**코드는 이해하기 쉬워야 한다**
- 무엇이 코드를 더 좋게 만드는가?
  - case 1
    ```c
    for (Node* node = list->head; node!=NULL; node = node->next)
        Print(node->data);

    // 위 코드가 아래보다 낫다
    
    Node* node = list->head;
    if (node == NULL) return;

    while(node->next != NULL) {
        Print(node->data);
        node = node->next;
    }
    if (node != NULL) Print(node->data);
    ```
  - case 2
    ```c
    return exponent >= 0 ? mantissa * (1 << exponent) : mantissa / (1 << -exponent);

    // 아래 코드가 조금 더 친숙하다
    
    if (exponent >= 0) {
        return mantissa * (1 << exponent);
    } else {
        return mantissa / (1 << -exponent);
    }
    ```

- 가독성의 기본 정리
  - 코드는 다른 사람이 이해하는데 들이는 시간을 최소화하는 방식으로 작성되어야 한다

- 분량이 적으면 항상 더 좋은가?
  - 적은 분량으로 코들르 작성하는 것은 좋은 목표긴 하지만, 이해를 위한 시간을 최소화하는 게 더 좋은 목표다

- 이해를 위한 시간은 다른 목표와 충돌하는가?
  - 코드의 효율성, 잘 구성된 아키텍처, 테스트 용이성 등 다른 제약조건들과 이해하기 쉬운 코드는 충돌하지 않는다

