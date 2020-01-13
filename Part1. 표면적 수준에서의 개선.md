## 이름에 정보 담기

- 변수, 함수, 클래스 등의 이름을 결정할 때는 항상 이름을 일종의 설명문으로 간주해야 한다
- 좋은 이름을 정하면 생각보다 많은 정보를 전할 수 있다

- 좋은 이름을 고르는 여섯가지 주제

1. 특정한 단어 고르기
    - 구체적인 단어를 선택. 무의미한 단어를 피한다
    - `getPage()` 에서 get은 별다른 의미를 전달하지 않는다.
      - 만약 인터넷에서 가져오는 것 이라면?
      - `FetchPage()` 또는 `DownloadPage()` 가 더 의미있을 것이다
    - `int Size()` Size() method는 무엇을 반환할까? 구체적으로 적어주자
      - Binary Tree 인 경우
      - `Height()` 또는 `NumNodes()` 또는 `MemoryBytes()` 와 같은 이름이 더 의미있을 것
    - `void Stop()` 정확히 무엇을 수행하는지 따라 더 의미있는 이름을 사용할 수 있다
      - 다시는 되돌릴 수 없는 동작: `Kill()`
      - Resume()을 통해 다시 돌이킬 수 있는 동작: `Pause()`
    - 더 화려한 단어 고르기
      - send < deliver, dispathc, announce, distribute, route
      - find < search, extract, locate, recover
      - start < launch, create, begin, open
      - make < create, set up, build, generate, compose, add, new
2. 보편적인 이름 피하기
    - tmp나 retval 같은 보편적 이름을 피한다
    ``` javascript
    var euclidean_norm = function(v) {
        var retval = 0.0;
        for (var i=0; i< v.length; i++)
            retval += v[i] * v[i];
        return Math.sqrt(retval);
    };
    // retval 보다 sum_squares라면 이 변수의 목적이나 담고있는 값을 더 잘 설명한다
    ```

    - 보편적인 이름이 필요한 의미를 전해주는 경우
    - tmp 라는 이름은 대상이 짧게 임시적으로 존재하고, '임시적 존재' 자체가 변수의 가장 중요한 용도일 때 한해서 사용
    ``` javascript
    if (right < left) {
        tmp = right;
        right = left;
        left = tmp;
    } // tmp 라는 이름이 목적에 완벽하다
    ```
    - iterator
      - nested loop의 경우 iterator 의 이름을 명확하게 주자
        ```cpp
        for(int i=0; clubs.size(); i++ ){
            for(int j=0; clubs[i].members.size(); j++){
                for(int k=0; k < users.size(); k++){
                    if(clubs[i].members[j] == users[k])
                        // do something...
                }
            }
        }
        // i,j,k 대신 ci,mi,ui 를 사용하면 로직이 명확해진다.
        ```
3. 추상적인 이름 대신 구체적인 이름 사용하기
    - 디버깅 정보 출력하는 플래그
      - `--run_locally` 보단 `--extra_logging`이 낫다
      - 추가 로깅 이외에 로컬 DB설정 후 사용하는 일도 같이한다면?
        - `--run_locally` 로 통짜로 쓰기보단 `--use_local_database`라는 2번째 flag를 만드는 것이 낫다
4. 접두사 혹은 접미사로 이름에 추가적인 정보 덧붙이기
    - 16진수 문자열을 담고있는 id 값?
      - `string id // example: af84ef845cd8` 보단 `hex_id` 가 낫다
    - 시간이나 바이트의 수와 같은 측정치를 담고 있다면 단위를 표현하는 것이 좋다
      - ``` javascript
        // Bad
        var start = (new Date()).getTime();
        ...
        var elapsed = (new Date()).getTime() - start;

        // Good
        var start_ms = (new Date()).getTime();
        ...
        var elapsed_ms = (new Date()).getTime() - start_ms;
        document.writeln("Load time was: "+ elapsed_ms / 1000 + " Seconds");
        ```
     - Start(delay) -> delay_secs
     - CreateCache(size) -> size_mb
     - ThrottleDownload(limit) -> max_kbps
     - Rotate(angle) -> degrees_cw //clock wise
   - 다른 중요한 속성 포함하기
     - 변수에 위험요소 또는 중요한 내용이 있다면 이 방법을 사용한다 (보안)
     - 패스워드가 평문에 담겨있고 추가적인 처리를 하기 전 암호화 되야하는 경우
       - password -> plaintext_password
     - 사용자에게 보여질 comment가 화면 나타나기 전 escaping되야하는 경우
       - comment -> unescaped_comment
     - html의 바이트가 UTF-8로 변환된 경우
       - html -> html_utf8
     - 입력데이터가 'url encoded' 된 경우
       - data -> data_urlenc
5. 이름이 얼마나 길어져도 좋은지 결정하기
    - 이름은 지나치게 길면 안된다
      - `newNavigationControllerWrappingViewControllerForDataSourceOfClass`
    - 변수가 사용되는 scope를 따진다
      - 좁은 범위인 경우 -> 변수 이름에 많은 정보를 담을 필요가 없다
        ```cpp
        if (debug) {
            map<string, int> m;
            LookUpNamesNumbers(&m);
            Print(m);
        }
        ```
    - 긴 이름 입력하기 - 더이상 문제되지 않는다
      - 각종 에디터의 단어완성 기능 사용
        - vi : `ctrl + p`
        - eclipse, intellij: `alt+/`
    - 약어와 축약형
      - BackEndManager를 BEManager라 쓸 이유가 없다
    - 불필요한 단어 제거
      - ConverToString() -> ToString(), DoServeLoop() -> ServeLoop()
6. 추가적인 정보를 담을 수 있게 이름 구성하기
    - 이름 포맷팅으로 의미를 전달
     - class member를 local variable 과 구분하기 위해 '_'를 부티는 것 같이
       - 팀에 맞게 formatting conventions를 사용

## 오해할 수 없는 이름들

- 본인이 지은 이름을 다른 사람들이 다른 의미로 해석할 수 있는지 생각해보기
- `results = Database.all_objects.filter("year <= 2011")`에서 results 가 담고있는 정보?
  - 2011 년 이하의 정보? 또는 2011년 이하를 제외한 정보?
  - filter의 의미가 모호, 대상을 고르는 지 제거하는 지?
  - 대상을 고르는 기능이면 select가, 제거한다면 exclude가 더 낫다
- 텍스트의 끝을 오려낸 다음 '...'을 붙이는 함수
    ```python
    # Bad
    def Clip(text, length):

    # Good
    def Truncate(text, max_chars) # 문자의 수를 나타내고 싶을 경우
    ```
  
- 한계를 설정하는 이름을 가장 명확하게 만드는 방법은 제한받는 대상에 max, min을 붙이는 것이다
  ```python
    # Shopping cart 에 10개 이상의 품목을 구매하지 못하게 하고 싶은 경우
    # Bad (LIMIT은 이상인치 초과인지 의미가 불분명하다)
    CART_TOO_BIG_LIMIT = 10
    
    if shopping_cart.num_items() >= CART_TOO_BIG_LIMIT:
        Error("Too many items in cart.")
    # Good
    MAX_ITEMS_IN_CART = 10
    if shopping_cart.num_items() > MAX_ITEMS_IN_CART:
        Error("Too many items in cart.")
  ```
- 경계를 포함하는 범위에는 first, last를 사용
```python
    # Bad
    print integer_range(start=2, stop=4) # [2,3] 과 [2,3,4] 어느 것을 출력할지 모호
    
    # Good
    print integer_range(start=2, last=4) # last는 마지막 것이 포함되는 명확한 의미가 있다
```
- 경계를 포함하고/배제하는 범위에는 begin, end를 사용
  - Due를 나타내고 싶은 경우와 같이 현실에서는 포함 ~ 배제가 효율적인 경우가 있다.
```python
    # Bad
    PrintEventsInRange("OCT 16 12:00am","OCT 16 11:59:59.9999pm")

    # Good
    PrintEventsInRange("OCT 16 12:00am","OCT 17 12:00am")
    # 이 경우는 begin, end가 적절한 의미를 가진다
```
- Boolean 변수에 이름 붙이기
  - boolean 변수 또는 boolean을 반환하는 함수에 이름을 붙일 때는 true, false가 각각 무엇을 의미하는지 명확해야 한다
  - `bool read_password = true`는 위험하다 아래 두가지로 해석 가능하기 때문이다
    - 우리는 패스워드를 읽을 필요가 있다
    - 패스워드가 이미 읽혔다
  - `need_password` 또는 `user_is_authenticated`를 사용하는 것이 낫다
  - is, has, can, should 같은 단어를 더하면 의미가 더 명확해진다
  - 이름에서는 의미를 부정하는 용어를 피하는 것이 좋다
    - `bool disable_ssl = false;` 대신 `bool use_ssl = true;` 가 낫다

- 사용자의 기대에 부응하기
  - get()
    - 일반적으로 get으로 시작되는 이름의 메소드는 '가벼운 접근자(lightweight accessors)'로서 단순히 내부 멤버를 반환한다고 관행적으로 생각한다
    - 만일, 시간이 오래 걸리는 method라면 get보단 compute 등을 사용하는 것이 더 낫다
  - list.size()
    - 일반적으로 size()는 가벼운 접근자라고 생각한다
    - somelist.size()가 만약 O(n) 연산일 경우
    ```cpp
    void ShrinkList(list<Node>& list, int max_size) {
      while(list.size() > max_size) {
        FreeNode(list.back());
        list.pop_back();
      }
    }
    // 실제로는 size때문에 O(n^2) 연산이 된다.
    ```
- 이름을 짓기 위해 복수의 후보를 평가하기
  - 의미가 오해되지 않는 이름이 최선의 이름이다.


## 미학

- 좋은 소스코드는 눈을 편하게 한다
  - 코드를 읽는 사람이 이미 친숙한, 일관성 있는 레이아웃을 사용하라
  - 비슷한 코드는 서로 비슷해 보이게 만들어라
  - 서로 연관된 코드는 하나의 블록으로 묶어라
  - 미학과 설계를 동시에 추구하는 것이 가장 이상적이다
- 미학이 무슨 상관인가?
  - **미학적으로 보기 좋은 코드가 사용하기 더 편리하다**
```cpp
// Bad
class StatsKeeper {
  public:
//일련의 더블 변수값을 저장하는 클래스
   void Add(double d); // 그리고 그런 값들에 대한 간단한 통계를 계산하는 메소드
  private: int count; /*지금까지 몇개가 저장되었는가
*/ public:
      double Average();
  private: double minimum;
  list<double>
    past_items
      ; double maximum;
};

// Good

// 일련의 더블 변수값을 저장하는 클래스
// 그리고 그런 값들에 대한 간단한 통계를 계산하는 메소드
class StatsKeeper {
  public:
    void Add(double d);
    double Average();
  
  private:
    list<double> past_items;
    int count; // 지금까지 몇 개가 저장되었는가
  
    double minimum;
    double maximum;
}
```

- 일관성과 간결성을 위해서 줄 바꿈을 재정렬하기
- 다음 네가지 parameter를 받는 TcpConnectionSimulator 클래스를 잘 나타내면

```java
  public class PerformanceTester {
    // TcpConnectionSimulator (처리량, 지연속도, 흔들림, 패킷손실)
    //                        [kbps]  [ms]   [ms]  [percent]
    
    public static final TcpConnectionSimulator wifi =
      new TcpConnectionSimulator(500, 80, 200, 1);

    public static final TcpConnectionSimulator t3_fiber = 
      new TcpConnectionSimulator(45000, 10, 0, 0);

    public static final TcpConnectionSimulator cell = 
      new TcpConnectionSimulator(100, 400, 250, 5);
  }
```

- 메소드를 활용하여 불규칙성을 정리하라
  - 복잡하고 미학적으로 만족스럽지 않은 반복되는 코드들은 줄바꿈 정도로 코드가 개선되지 않는다
    - 이 경우 method를 따로 분리하여 개선하자
- 도움이 된다면 코드 열을 맞춰라
  - 직선으로 뻗은 끝선과 열은 텍스트를 쉽게 훑어보게 한다
  - 경우에 따라 열 정렬을 통해 코드를 더 읽기 쉽게 한다
  - 일단 해보고 정 불편하면 하지 않아도 된다
- 의미 있는 순서를 선택하고 일관성 있게 사용하라
  - 코드의 순서가 정확성이 아무런 영향을 미치지 않는 경우가 많다
  - 그런 경우에도 임의의 순서가 아니라 의미 있는 순서로 나열하는 편이 낫다
    - 기준이 될 수 있는 것들
      - 변수의 순서를 html 의 input tag 순서
      - 가장 중요한 것에서 가장 덜 중요한 것
      - 알파벳 순서
- 선언문으로 블록을 구성하라
  - 우리의 뇌는 자연스레 그룹과 계층구조를 따라 동작한다. 코드도 이렇게 짠다
```cpp
// Good

class FrontendServer {
  public:
    FrontendServer();
   ~FrontendServer();
  
  // Handlers
  void ViewProfile(HttpRequest* request);
  void SaveProfile(HttpRequest* request);
  void FindFriends(HttpRequest* request);

  // Query/Reply Utilities
  string ExtractQueryParam(HttpRequest* request, string param);
  void ReplyOK(HttpRequest* request, string html);
  void ReplyNotFound(HttpRequest* request, string error);

  // Database Helpers
  void OpenDatabase(string location, string user);
  void CloseDatabase(string location);
}
```

- 코드를 문단으로 쪼개라
  - 일반 텍스트가 여러 개의 문단(Paragraphs)으로 나뉘어진 이유
    - 비슷한 생각을 하나로 묶어서 성격이 다른 생각과 구분한다
    - 문단은 시각적 디딤돌 역할을 수행한다. 문단이 없으면 하나의 페이지 안에서 읽던 부분을 놓치기 쉽다
    - 하나의 문단에서 다른 문단으로의 전진을 촉진시킨다
  - 같은 이유로 코드로 여러 '문단'으로 분할할 필요가 있다

```python
  def suggest_new_friends(user, email_password):
    # 사용자 친구들의 이메일 주소를 읽는다.
    friends = user.friends()
    friends_emails = set(f.email for f in friends)

    # 이 사용자의 이메일 계정으로부터 모든 이메일 주소를 읽어들인다.
    contacts = import_contacts(user.email, email_password)
    contact_emails = set(c.email for c in contacts)

    # 아직 친구가 아닌 사용자들을 찾는다.
    non_friend_emails = contact_emails - friend_emails
    suggested_friends = User.objects.select(email__in=non_friend_emails)

    # 사용자 리스트를 화면에 출력한다.
    display['user'] = user
    dispaly['friends'] = friends
    dispaly['suggested_friends'] = suggested_friends

    return render("suggested_friends.html", display)
```

- 개인적인 스타일 대 일관성
  - 블록을 여는 괄호를 어디에 놓는가 같은 개인적 스타일의 문제로 귀결되는 사안이 있다
  - 뒤섞이면 가독성에 영향을 주니 기존 프로젝트의 스타일을 따르자
  - **일관성 있는 스타일은 '올바른' 스타일보다 더 중요하다**
  
## 주석에 담아야 하는 대상