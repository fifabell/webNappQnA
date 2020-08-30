# webNapp QnA

👻 <br>
안드로이드와 웹을 공부하면서 궁금한 것들을 정리합니다.

- recent updates : 2020-08-30

---
## 목차

### 1. 안드로이드 📱

  
  <details>
    <summary> 
        Activity / Context 
    </summary>
  
  * Activity
    - __정의__ <br>
    _사용자에게 UI가 있는 화면을 제공_ 하는 앱 컴포넌트. <br><br>
    각 액티비티는 다른 액티비티를 실행할 수 있고, <br>
    새로운 액티비티가 시작되면 시스템은 '백스택'에 담고, 사용자에게 보여준다. <br>
    백스택은 '스택(LIFO)' 매커니즘을 따르며, 사용자가 뒤로가기 버튼을 누를 경우, <br>
    스택의 최상위(top)에 있는 현재 액티비티를 제거(pop and destroy)하고 이전의 액티비티를 시작한다.
    
    - Activity 생명주기(LifeCycle)

    ![LifeCycle](./img/LifeCycle.png)

      - OnCreate() <br>
      이 콜백은 시스템이 먼저 활동을 생성할 때 실행되는 것으로, 필수적으로 구현해야 한다. <br>
      활동이 생성되면 생성됨 상태가 된다. onCreate() 메서드에서 활동의 전체 수명 주기 동안 한 번만 발생해야 하는 기본 애플리케이션 시작 로직을 실행한다. <br>
      예를 들어 onCreate()를 구현하면 데이터를 목록에 바인딩하고, 활동을 ViewModel과 연결하고, 일부 클래스 범위 변수를 인스턴스화할 수도 있다.<br>
      이 메서드는 savedInstanceState 매개변수를 수신하는데, 이는 활동의 이전 저장 상태가 포함된 Bundle 객체다.<br>
      이번에 처음 생성된 활동인 경우 Bundle 객체의 값은 null이다.

  <br>  
  * Context
    - 정의 : 
    
  <br>  
  
  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        AsyncTask 
    </summary>
  
  [Top of page](#목차)
  </details>
 
  <details>
    <summary> 
        Background 
    </summary>

    * thread
    * handler
    * message
    * messageQueue
    * looper
    * runnable

  [Top of page](#목차)
  </details>
  
  <details>
    <summary> 
        Connection 
    </summary>

    * URLConnection
    * HttpsURLConnection
    * TrustManager
  
  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        Input output 
    </summary>

    * InputStream 
    * InputStreamReader
  
  [Top of page](#목차)
  </details>
    
    
  <details>
    <summary> 
        Inflater 
    </summary>
  
  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
        Listener 
    </summary>
  
  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
        WebView 
    </summary>

    * WebView 
    * Drawer
    
  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        SharedPreference 
    </summary>
  
  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
        etc 
    </summary>

    * ArrayList<HashMap>
    * static 
    * Callback
    * OncreateOptionsMenu
    
  [Top of page](#목차)
  </details>
    
---

### 2. 웹  🖥

  <details>
    <summary> 
        통신 
    </summary>
  
  [Top of page](#목차)
  </details>




