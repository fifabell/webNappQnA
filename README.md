# webNapp QnA

👻 <br>
안드로이드와 웹을 공부하면서 궁금한 것들을 정리합니다.<br>
[android samples](https://github.com/fifabell/AndroidStudy/tree/master/sample)<br>
[web samples](https://gist.github.com/fifabell)<br>

- recent updates : 2020-11-19

---
## 목차

### 1. 안드로이드 📱

- androidStudio 3.5.1
- minSDK : 16
 
  <details>
    <summary> 
      Activity vs AppCompatActivity and Context
    </summary>
  
  * Activity
    - __정의__ <br>
    _사용자에게 UI가 있는 화면을 제공_ 하는 앱 컴포넌트. <br><br>
    각 액티비티는 다른 액티비티를 실행할 수 있고, <br>
    새로운 액티비티가 시작되면 시스템은 '백스택'에 담고, 사용자에게 보여준다. <br>
    백스택은 '스택(LIFO)' 매커니즘을 따르며, 사용자가 뒤로가기 버튼을 누를 경우, <br>
    스택의 최상위(top)에 있는 현재 액티비티를 제거(pop and destroy)하고 이전의 액티비티를 시작한다.
    
    - __Activity 생명주기(LifeCycle)__

    ![LifeCycle](./img/LifeCycle.png)

      - `OnCreate()` <br>
      이 콜백은 시스템이 먼저 활동을 생성할 때 실행되는 것으로, 필수적으로 구현해야 한다. <br>
      활동이 생성되면 생성됨 상태가 된다. onCreate() 메서드에서 활동의 전체 수명 주기 동안 한 번만 발생해야 하는 기본 애플리케이션 시작 로직을 실행한다. <br>
      예를 들어 onCreate()를 구현하면 데이터를 목록에 바인딩하고, 활동을 ViewModel과 연결하고, 일부 클래스 범위 변수를 인스턴스화할 수도 있다.<br>
      이 메서드는 savedInstanceState 매개변수를 수신하는데, 이는 활동의 이전 저장 상태가 포함된 Bundle 객체다.<br>
      이번에 처음 생성된 활동인 경우 Bundle 객체의 값은 null이다.<br>

        ```java

        String str;

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            // 인스턴스 상태 복구
            if (savedInstanceState != null) {
                str = savedInstanceState.getString(STATE_KEY);
            }

            setContentView(R.layout.main_activity);

            ...
        }
        ```
        onCreate() 메서드가 실행을 완료하면 시작됨 상태가 되고, 시스템이 연달아 onStart()와 onResume() 메서드를 호출한다.<br><br>

      - `OnStart()` <br>
      활동이 시작됨 상태에 들어가면 시스템은 이 콜백을 한다.<br>
      onStart()가 호출되면 활동이 사용자에게 표시되고, 이 메서드에서 앱이 UI를 관리하는 코드를 초기화한다.<br><br>
      onStart() 메서드는 매우 빠르게 완료되고, 생성됨 상태와 마찬가지로 활동은 시작됨 상태에 머무르지 않는다.<br>
      이 콜백이 완료되면 활동이 재개됨 상태에 들어가고, 시스템이 onResume() 메서드를 호출한다.<br><br>

      - `OnResume()` <br>
      활동이 재개됨 상태에 들어가면 포그라운드에 표시되고 시스템이 onResume() 콜백을 호출한다.<br>
      이 상태에 들어갔을 때 앱이 사용자와 상호작용한다. 어떤 이벤트가 발생하여 앱에서 포커스가 떠날 때까지 앱이 이 상태에 머무른다.<br> 예를 들어 전화가 오거나, 사용자가 다른 활동으로 이동하거나, 기기 화면이 꺼지는 이벤트가 이에 해당한다.<br><br>
      방해되는 이벤트가 발생하면 활동은 일시중지됨 상태에 들어가고, 시스템이 onPause() 콜백을 호출한다.<br><br>

      - `OnPause()` <br>
      시스템은 사용자가 활동을 떠나는 것을 나타내는 첫 번째 신호로 이 메서드를 호출한다.(하지만 해당 활동이 항상 소멸되는 것은 아님)<br> 활동이 포그라운드에 있지 않게 되었다는 것을 나타낸다(다만 사용자가 멀티 윈도우 모드에 있을 경우에는 여전히 표시 될 수도 있음).<br><br>
      onPause() 메서드의 실행이 완료되더라도 활동이 일시중지됨 상태로 남아 있을 수 있다.<br> 오히려 활동은 다시 시작되거나 사용자에게 완전히 보이지 않게 될 때까지 이 상태에 머무른다.<br>
      활동이 다시 시작되면 시스템은 다시 한번 onResume() 콜백을 호출한다. <br>
      활동이 일시중지됨 상태에서 재개됨 상태로 돌아오면 시스템은 Activity 인스턴스를 메모리에 남겨두고, 시스템이 onResume()을 호출할 때 인스턴스를 다시 호출한다. 이 시나리오에서는 최상위 상태가 재개됨 상태인 콜백 메서드 중에 생성된 구성요소는 다시 초기화할 필요가 없다. 활동이 완전히 보이지 않게 되면 시스템은 onStop()을 호출한다. 
      
      - `OnStop()` <br>
      활동이 사용자에게 더 이상 표시되지 않으면 중단됨 상태에 들어가고, 시스템은 onStop() 콜백을 호출한다. <br>
      이는 예를 들어 새로 시작된 활동이 화면 전체를 차지할 경우에 적용된다. 시스템은 활동의 실행이 완료되어 종료될 시점에 onStop()을 호출할 수도 있다.<br><br>
      onPause() 대신 onStop()을 사용하면 사용자가 멀티 윈도우 모드에서 활동을 보고 있더라도 UI 관련 작업이 계속 진행됩니다.<br>
      또한 onStop()을 사용하여 CPU를 비교적 많이 소모하는 종료 작업을 실행해야 한다. 예를 들어 정보를 데이터베이스에 저장할 적절한 시기를 찾지 못했다면 onStop() 상태일 때 저장할 수 있다.<br><br>
      활동은 정지됨 상태에서 다시 시작되어 사용자와 상호작용하거나, 실행을 종료하고 사라진다.<br>
      활동이 다시 시작되면 시스템은 onRestart()를 호출한다. Activity가 실행을 종료하면 시스템은 onDestroy()를 호출한다. 

      - `OnDestory()` <br>
      onDestroy()는 활동이 소멸되기 전에 호출된다. 시스템은 다음 중 하나에 해당할 때 이 콜백을 호출한다.<br>

        1) (사용자가 활동을 완전히 닫거나 활동에서 finish()가 호출되어) 활동이 종료되는 경우
        2) 구성 변경(예: 기기 회전 또는 멀티 윈도우 모드)으로 인해 시스템이 일시적으로 활동을 소멸시키는 경우<br>
      활동이 종료되는 경우 onDestroy()는 활동이 수신하는 마지막 수명 주기 콜백이 된다.<br>
      구성 변경으로 인해 onDestroy()가 호출되는 경우 시스템이 즉시 새 활동 인스턴스를 생성한 다음, 새로운 구성에서 그 새로운 인스턴스에 관해 onCreate()를 호출한다.<br><br>
      onDestroy() 콜백은 이전의 콜백에서 아직 해제되지 않은 모든 리소스(예: onStop())를 해제해야 한다.<br><br>  

  * AppCompatActivity
    - __정의__ <br>
      안드로이드의 하위버전을 지원하는 액티비티이다. <br><br>
      하위버전 메소드가 실행이 안될 때 지를 지원하기 위해 AppCompatActivity를 사용하며,<br>
      ActionBar 역시 하위 버전 단말기에서는 이 액티비티를 사용해야 한다.<br><br>

  * Context
    - __정의__ <br>
      안드로이드 시스템에서 제공하는 추상 클래스이다.<br>
      새로 생성된 객체가 지금 어떤 일이 일어나고 있는지 알 수 있도록 한다. 따라서 액티비티와 애플리케이션에 대한 정보를 얻기 위해서는 컨텍스트를 사용하면 된다.

    - __Application Context__ <br>
      애플리케이션 컨텍스트는 싱글턴 인스턴스이며 액티비티에서 getApplicationContext()를 통해 접근할 수 있다.<br>
      이 컨텍스트는 애플리케이션의 라이프사이클과 연결되어 있다. 애플리케이션 컨텍스트는 현재의 컨텍스트와 분리된 라이프사이클을 가진 컨텍스트가 필요할 때나 액티비티의 범위를 넘어서 컨텍스트를 전달할 떄에 사용한다.

    - __Activity Context__ <br>
      액티비티 컨텍스트는 액티비티에서 사용 가능하며 이 컨텍스트는 액티비티의 라이프사이클과 연결되어 있다. 액티비티의 범위 내에서 컨텍스트를 전달하거나, 라이프사이클이 현재의 컨텍스트에 붙은 컨텍스트가 필요할 때(need the context whose lifecycle is attached to the current context) 액티비티 컨텍스트를 사용한다.<br>

    - __Context 얻기__ <br>      
      * View.getContext() :<br>
      현재 뷰가 가지고 있는 context를 반환하는데, 일반적으로는 Activity에서 View를 띄우기 때문에 Activity의 Context가 된다.<br>

      * Activity.getApplicationContext() :<br>
      애플리케이션 전체의 컨텍스트를 반환합니다. 현재 액티비티뿐만 아니라 애플리케이션의 수명주기와 관련된 컨텍스트가 필요한 경우 Activity Context대신 이 값을 사용하면 됩니다.<br>

      * ContextWrapper.getBaseContext() : <br>
      다른 컨텍스트로부터 어떤 컨텍스트에 접근해야하는경우에 ContextWrapper를 씁니다. ContextWrapper 내부에서 참조 된 Context는 getBaseContext()를 통해 액세스됩니다.<br>
  
  [Top of page](#목차)
  </details>

  <details>
    <summary> 
      AsyncTask 
    </summary>
    
  - __정의__ <br>
    쓰레드, 메시지루프 등의 원리를 이해하지 않아도 `하나의 클래스에서 UI 작업을 쉽게 할 수 있게 해준다`.<br>
    안드로이드는 UI를 담당하는 메인 스레드가 존재하는데, 이 스레드는 우리가 함부로 접근이 불가능하게 막아뒀다.<br>
    UI변경은 메인 스레드에서만 가능하므로, 우리가 만든 스레드에서는 화면을 바꾸는 어떠한 일도 할 수 없다.<br>
    이 작업을 가능하게 해주는 것이 바로 이 AsyncTasc이다.
  
  - __사용법__ <br>
  
    ![AsyncTask](./img/asyncTask.jpg)
    `onPreExcuted()` -> `doInBackground()` -> { `publishProgress()` -> onProgressUpdate():UI refresh } -> return(result) -> `onPostExcuted()` <br>
    excute()명령을 통해 AsyncTask 명령어 실행.<br>
    이후 크게 네 가지만 알고 넘어가자.<br>
    
    * onPreExcuted() : 스레드 작업 이전에 수행할 동작을 구현.<br>
    * publishProgress() : doInBackground()에서 중간중간 진행 상태를 UI에 업데이트 하도록 하는 메서드 -> 자동으로 onProgressUpdate()가 호출 됨.<br>
    * doInBackground() : 실제 스레드 작업이 진행.<br>
    * onPostExcuted() : 결과 파라미터를 리턴하면서 그 리턴값을 통해 스레드 작업이 끝났을 때 동작을 구현.<br><br>

  - __제약조건__ <br>
    * API16(젤리빈) 미만 버전에서는 AsyncTask 선언을 UI Thread에서 해주지 않으면 오류가 발생한다. <br>
    * excutes(Params)는 UI 스레드에서 직접 호출해야한다. <br>
    * 수동으로 onPreExecute(), onPostExecute(Result), doInBackground(Params...), onProgressUpdate(Progress...) 호출하면 안된다. <br>
    * Task는 오직 한번만 실행될 수 있다.

  - __장점__ <br>
    * 비교적 오래 걸리지 않은 작업에 유용함.<br>
    * Task 캔슬이 용이하며 로직과 UI 조작이 동시에 일어나야 할 때 사용<br>

  - __단점__ <br>
    * 하나의 객체이므로 재사용이 불가능하다. (메모리 효율 문제) <br>
    * 구현한 액티비티 종료 시 별도의 지시가 없다면 종료되지 않는다. <br>
    * Activity 종료 후 재시작 시 AsyncTask의 Reference는 무효하며, onPostExecute() 메소드는 새로운 Activit에 어떠한 영향도 끼치지 못한다. <br>
    * AsyncTask의 기본 처리 작업 개수는 1개다. <br>

  - __직렬 vs 병렬 실행__ <br>
    
    ```java
    .execute(); // 직렬실행
    .executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR); // 병렬실행
    ```

  [Top of page](#목차)
  </details>
 
  <details>
    <summary> 
        Background
    </summary>

  * thread vs process
    - __Thread 정의__ <br>
      스레드(thread)는 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다. 일반적으로 한 프로그램은 하나의 스레드를 가지고 있지만, 프로그램 환경에 따라 둘 이상의 스레드를 동시에 실행할 수 있다. 이러한 실행 방식을 멀티스레드(multithread)라고 한다.<br>

    - __process 정의__ <br>
      프로세스(process)는 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램을 말한다. 종종 스케줄링의 대상이 되는 작업(task)이라는 용어와 거의 같은 의미로 쓰인다. 여러 개의 프로세서를 사용하는 것을 멀티프로세싱이라고 하며 같은 시간에 여러 개의 프로그램을 띄우는 시분할 방식을 멀티태스킹이라고 한다.<br>
      
    - __thread vs process__ <br>    
      멀티프로세스와 멀티스레드는 양쪽 모두 여러 흐름이 동시에 진행된다는 공통점을 가지고 있다. 하지만 멀티프로세스에서 각 `프로세스는 독립적으로 실행되며 각각 별개의 메모리를 차지`하고 있는 것과 달리 멀티스레드는 프로세스 내의 `메모리를 공유`해 사용할 수 있다. 또한 프로세스 간의 전환 속도보다 `스레드 간의 전환 속도가 빠르다`.<br>

      멀티스레드의 다른 장점은 CPU가 여러 개일 경우에 각각의 CPU가 스레드 하나씩을 담당하는 방법으로 속도를 높일 수 있다는 것이다. 이러한 시스템에서는 여러 스레드가 실제 시간상으로 동시에 수행될 수 있기 때문이다.<br>

      멀티스레드의 단점에는 각각의 스레드 중 어떤 것이 먼저 실행될지 그 순서를 알 수 없다는 것이 있다.<br>  

  * Runnable
    - __정의__<br>
      Thread의 인터페이스화 된 형태이며, Thread내의 run()메서드를 통해 수행할 내용들을 정의한다.<br>
      void run() : 이 스레드가 별도의 Runnable실행 객체를 사용하여 작성된 경우 해당 Runnable객체의 run메소드가 호출된다.<br>

  * Cycle 
    - ThreadCycle

    ![threadcycle](./img/Thread.png)

    1. 안드로이드에서 제공하는 handler 클래스를 상속하는 클래스를 만든다.
    2. 메시지 큐에 메모리 공간을 얻기위해 obtainMessage 메소드를 이용하여 메시지 공간을 만든다.
    ```java
      Message msg = handler.ObtainMessage();
    ```
    3. 메시지 데이터를 넣기위해 Bundle 객체를 사용한다.
    ```java
      Bundle bundle = new Bundle();
    ```
    4. bundle.putString 메소드를 사용해 입력값을 집어넣는다.
    ```java
      bundle.putSting(key, text);
    ```
    5. 메시지에 번들데이터를 집어 넣는다.
    ```java
      msg.setData(bundle);
    ```
    6. 메시지큐로 보낸다.
    ```java
      handler.sendMessage(msg);
    ```
    7. 핸들러 클래스에서는 전송된 메시지를 받는다.
    ```java
      bundle = msg.getData();
    ```
    8. bundle에서 전달된 데이터를 받는다.
    ```java
      text = bundle.getString(key);
    ```

  * handler
    - __정의__ <br>
      Worker Thread에서 Main Thread로 메시지를 전달하는 역할을 수행.<br>
      안드로이드에서 UI처리를 위해 사용되는 기본 스레드는 ‘메인 스레드(Main Thread)’라고 부른다. 이 메인 스레드에서 이미 UI에 접근하고 있으므로 새로 생성한 다른 스레드에서는 핸들러(Handler) 객체를 이용해 메시지를 전달함으로써 메인 스레드에서 처리하도록 만들 수 있다.<br> 
      동시 접근에 따른 데드락 문제를 해결하는 가장 간단한 방법은 작업을 순서대로 처리하는 것이다. 이 역할은 메인스레드의 핸들러가 담당하여 처리한다.<br>

    - __주요함수__ <br>
      * Handler.sendMessage(Message msg)<br>
      Message 객체를 message queue에 전달하는 함수.<br>

      * Handler.sendEmptyMessage(int what)<br>
      Message의 what필드를 전달하는 함수<br>

      * Handler.post(new Runnable())<br>
      Runnable 객체를 message queue에 전달하는 함수.<br>
      post를 통해 전달된 Runnable 객체는 해당 핸들러가 연결된 스레드에서 실행된다. UI작업을 처리하기 위해 핸들러를 메인 스레드에서 생성하여 핸들러와 메인 스레드가 연결되어 있어야 한다.<br>


  * messageQueue
    - __정의__ <br>
      핸들러가 전달하는 message를 보관하는 FIFO(First In First Out)방식의 큐이다.<br>
      다른 스레드에게 메시지를 전달하려면 수신 대상 스레드에서 생성한 핸들러의 post나 sendMessage등의 함수를 사용해야 한다. 이후 수신대상 스레드의 Message Queue에 message가 저장된다.<br>
      Message Queue에 저장된 message나 runnable은 Looper가 차례대로 꺼내서 핸들러로 전달한다. 
  
  * Looper
    - __정의__ <br>
      루퍼는 스레드당 하나씩 밖에 가질 수 없고, 루퍼는 Message queue가 비어있는 동안 아무 행동도 하지 않고, 메시지가 들어오면 해당 메시지를 꺼내 적절한 Handler로 전달한다. 기본적으로 새로 생성한 스레드는 루퍼를 가지지 않고 Loper.prepare() 메서드를 호출해야 Looper가 생성된다.<br>

  [Top of page](#목차)
  </details>
  
  <details>
    <summary> 
      Connection 
    </summary>

  * URLConnection
    - __정의__ <br>
    사용자 인증이나 보안이 설정되어 있지 않은 웹서버에 접속하여 파일 등을 다운로드하는 데 많이 사용된다.

  * HttpsURLConnection
    ```java
    public abstract class HttpURLConnection extends URLConnection
    {
      URL u = new URL("https://www.naver.com");
      HttpURLConnection http = (HttpURLConnection) u.openConnection();
    }
    ```
    URLConnection 클래스와 마찬가지로 생성자가 protected로 선언되어있기 때문에 기본적으로는 개발자가 직접 HttpURLConnection 객체를 생성할 수 없다.<br>

    하지만 http URL을 사용하는 URL 객체의 openConnection() 메서드가 리턴하는 URLConnection 객체는 HttpURLConnection의 인스턴스가 될 수 있기 때문에 리턴된 URLConnection을 HttpURLConnection으로 캐스팅해서 사용한다. <br>

  * TrustManager
    쉽게 생각해서 웹에서 ssl인증서라고 보면된다.<br>
    하지만, googleplay에서 이를 신뢰하지 않아, CertificateExcetion 또는 IllegalArgumentException 예외를 발생시키는 코드를 구현해야 한다.

  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        DrawerLayout
    </summary>

  사전적으로는 서랍의 의미를 가지고 있으며, 평소에는 화면의 한 쪽에 숨겨져 있다가 사용자가 액션을 취하면 화면에 나타날 수 있도록 하는 레이아웃이다.<br>
  [NavigationView](https://developer.android.com/guide/navigation/navigation-ui?hl=ko#java)<br>

  
  [Top of page](#목차)
  </details>

  <details>
    <summary>
      푸시메시지
    </summary>
  
  화면이 꺼져있을 때 카톡알람이나 화면 위에서 스크롤을 내릴 때 Notification화면을 생각하면 된다.<br>
  자세한건 블로그에 정리해 놓았으니 참고해보자.<br>
  
  [블로그_확인하러가기](https://fifabell.github.io/android/fcm/Firebase-smp/)

  </details>

  <details>
    <summary> 
      stream
    </summary>

  - __정의__ <br>
    데이터의 흐름을 의미한다.<br>
    입력 스트림은 마우스, 키보드, 네트워크 등과 같은 입력 장치로부터 입력된 데이터가 순서대로 프로그램으로 흘러가는 데이터의 흐름을 뜻한다.<br>
    출력 스트림은 프로그램에서 출력된 데이터가 프린터, 모니터, 네트워크 등과 같은 출력장치로 순서대로 전송되도록 보장하는 데이터의 흐름이다.<br>
    스트림을 통해 흘러가는 데이터의 기본 단위는 바이트이다.<br>

  - __종류__ <br>
  * In/OutputStream
    이 클래스는 추상 클래스로서 바이트 스트림의 기능을 갖는 모든 클래스의 상위 클래스이다.

  * FileIn/OutputStream
    이 스트림을 이용해서 파일 시스템에 있는 파일로부터 바이트 데이터를 읽거나 파일에 바이트 데이터를 저장할 수 있다. 즉, 파일 입출력용 스트림이다.

  * DataIn/OutputStream
    이 스트림을 이용하면 자바 기본 타입의 데이터들이 바이너리 바이트(이진값)으로 다루어진다.

  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
        Inflater 
    </summary>

  - __정의__ <br>
  Inflater의 사전적 의미는 부풀리다는 뜻으로 LayoutInflater로서, XML에 저장해 둔 틀(Resource)을 실제 메모리(View객체로 반환)에 올려주는 역할을 한다.<br>
  예로, onCreate()메서드에 있는 setContentView(R.layout.activity_main) 또한 Inflater역할을 한다.<br>

  - __사용조건__ <br>
  1. 객체화하고자 하는 xml파일(sub1.xml)을 생성한다.
  2. 
  ```java
  LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
  // LayoutInflater 객체 사용할 준비를 완료한다.
  ```
  3.
  ```java
  inflater.inflate(R.layout.sub1, container, true);
  // 사전에 미리 선언해뒀던 container라는 레이아웃에 작성했던 xml의 메모리객체가 삽입.
  ```
  - __매개변수__<br>
  inflate( 객체화하고픈 xml파일, 객체화한 뷰를 넣을 부모 레이아웃/컨테이너, true(바로 인플레이션 하고자 하는지))
  
  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        permission 
    </summary>
  - manifest에 추가하는 permission 정리<br>
  - 참고 ) Android에서 permission 강회로 인한 마시멜로우(M) 이상 경우 권한 동의 체크를 반드시 얻어야 함.<br>
  
  ```java
  //basic_1
  <uses-permission android:name="android.permission.INTERNET" /> // 인터넷 사용
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> //네트워크 연결 확인
  <uses-permission android:name="android.permission.READ_PHONE_STATE" /> // 휴대전화번호 receive 가능.

  // 휴대전화번호 receive 예제
  TelephonyManager mgr;
  String phoneNumber;
  if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_DENIED) {
            try {
                phoneNumber = mgr.getLine1Number(); // getLine1Number는 내장함수
            } catch () {}
  }

  // basic_2
  <uses-permission android:name="android.permission.CAMERA" /> // 카메라 사용 
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /> // 외장 메모리 사용
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" /> // 외장 메모리 읽기
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" /> // 위치정보 확인

  // external
  <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" /> // 통신상태 변경
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/> // wifi 연결 확인
  <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/> // wifi 체인지를 확인
  <uses-permission android:name="android.permission.RECODER_AUDIO"/> // 녹음
  <uses-permission android:name="android.permission.WAKE_LOCK" /> // 알람
  <uses-permission android:name="android.permission.VIBRATE" /> // 진동
  <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" /> // 알림_윈도우.. 최상단 위치에 view띄우기 가능.
  
  // 사용예제
    // step_1 : permission
    <application>
        ...
        <service
            android:name=".MyService"
            android:enabled="true"
            android:permission="android.permission.SYSTEM_ALERT_WINDOW" >
        </service>
    </application>

    // step_2 : view_in_service.xml (서비스를 통해 보여질 view xml을 작성)
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:background="#0000ff">
    
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello World!"
            android:textColor="#ffffff"
            android:id="@+id/textView" />
    
        <ImageButton
            android:layout_width="368dp"
            android:layout_height="wrap_content"
            android:id="@+id/bt"
            android:text="click"
            android:textColor="#ffffff"
            android:src="@mipmap/ic_launcher"
            android:layout_below="@+id/textView"
            android:layout_alignParentLeft="true"
            android:layout_marginTop="12dp" />
    </RelativeLayout>

    // step_3 : MyService.java
    public class MyService extends Service {
  
      WindowManager wm;
      View mView;
  
      @Override
      public IBinder onBind(Intent intent) { return null; }
      
      @Override
      public void onCreate() {
          super.onCreate();
          LayoutInflater inflate = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
          wm = (WindowManager) getSystemService(WINDOW_SERVICE);
  
          WindowManager.LayoutParams params = new WindowManager.LayoutParams(
                  /*ViewGroup.LayoutParams.MATCH_PARENT*/300,
                  ViewGroup.LayoutParams.WRAP_CONTENT,
                  WindowManager.LayoutParams.TYPE_SYSTEM_ALERT,
                  WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE
                          |WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL
                          |WindowManager.LayoutParams.FLAG_WATCH_OUTSIDE_TOUCH,
                  PixelFormat.TRANSLUCENT);
  
          params.gravity = Gravity.LEFT | Gravity.TOP;
          mView = inflate.inflate(R.layout.view_in_service, null);
          final TextView textView = (TextView) mView.findViewById(R.id.textView);
          final ImageButton bt =  (ImageButton) mView.findViewById(R.id.bt);
          bt.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  bt.setImageResource(R.mipmap.ic_launcher_round);
                  textView.setText("on click!!");
              }
          });
          wm.addView(mView, params);
      }
  
      @Override
      public void onDestroy() {
        super.onDestroy();
        if(wm != null) {
            if(mView != null) {
                wm.removeView(mView);
                mView = null;
            }
            wm = null;
        }
      }
    }

  <uses-permission android:name="android.permission.KILL_BACKGROUND_PROCESSES" /> // 강제종료
  // 사용예제
    Intent intent = new Intent(Intent.ACTION_MAIN);
    intent.addCategory(Intent.CATEGORY_HOME);
    context.startActivity(intent);
    int pid = android.os.Process.myPid();
    android.os.Process.killProcess(pid); // kill @

  <uses-permission android:name="android.permission.CALL_PHONE" /> // 통화
  <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" /> // noti없이 다운.. 안쓰는 걸 추천.

  // 외에도 다양한 permission이 많음. 
  // 참고 https://s262701-id.tistory.com/96
  ```

    [Top of page](#목차)
  </details>

  <details>
    <summary> 
        Task 
    </summary>
  
  - __정의__ <br>
    * Task는 어플리케이션에서 실행되는 액티비티를 보관하고 관리하며 Stack형태의 연속된 Activity로 이루어진다.<br>
    * 스택 내에서 onCreate(push)-onDestroy(pop)에 의해 움직인다.<br>
    * 서로 다른 어플리케이션간의 이동에도 Task를 이용해 사용자 경험(UX)를 유지시켜 준다<br>
    * 최초 적재 액티비티는 Root Activity 라고 하며 어플리케이션 런처로부터 시작된다<br>
    * 마지막으로 적재되는 액티비티는 Top Activity 라고 하며 현재 화면에 활성화 되어있는 액티비티를 말한다<br>
    * Task내에는 서로 다른 어플리케이션의 액티비티들이 포함될 수 있어 어플리케이션에 경계없이 하나의 어플리케이션인것 처럼 보이게 해준다<br>
    * Task의 Stack내에 존재하는 액티비티들은 모두 묶여서 background와 foreground로 함께 이동한다. 홈버튼 클릭(task interrupt => background 이동), 홈버튼 롱클릭(recent task => foreground 이동)<br>
    * Flag를 사용하여 Task내 액티비티의 흐름을 제어할 수 있다<br>

  - __background vs foreground__ <br>
    일반적으로 앱을 종료하는 방식은 두 가지다.<br>

    1. 뒤로가기 버튼
    2. 모두보기-> 앱 날리기

    1번의 경우 프로세스가 백그라운드로 빠지는 것 뿐 실제로 종료되는 게 아니다.-> to background<br>
    2번의 경우 실제로 프로세스가 날아가게 된다.-> to foreground<br>

  - __Flag__ <br>
    
    > Manifest에 <activity>요소의 launchMode 속성 4가지<br>
    
    * standard <br>
      여러 개의 인스턴스가 생성가능함.<br>
    
    * singleTop <br>
    호출한 activity와 현재 최상위 activity가 동일한 경우 최상위 activity가 재사용된다. (기존 최상위 activity 는 pop)<br>
    
    * singleTask <br>
    루트 액티비티로만 존재하며 하나의 인스턴스만 생성가능.<br>
    
    * singleInstance <br>
    singleTask와 동일하지만 태스크 내에 해당 액티비티 하나만 속할 수 있어 다른 액티비티를 실행하면 새로운 Task가 생성됨.<br>

    > 소스 내 플래그<br>
    
    사용법 <br>

    ```java
    Intent intent = new Intent(MainActivity.this, SubActivity.class);
    intent.addFlag(Intent."플래그명");
    ```
    * FLAG_ACTIVITY_NEW_TASK<br>
    동일 task가 있으면 그곳에서 실행되고 아니면 새로운 task를 실행<br>

    * FLAG_ACTIVITY_SINGLE_TOP<br>
    B를 singletop설정 가정.<br>
    A,B 상태에서 B호출 시 A,재사용된 B<br>
    B,A 상태에서 B호출 시 B,A,B<br>

    * FLAG_ACTIVITY_NO_HISTORY<br>
    재활성화시(back키를 눌러 다시 활성화 될 때) pop!, 쉽게말해 뒤로가기하면 액티비티가 없어짐<br>
    B를 NO_HISTORY 설정 가정.<br>
    A,B,A 상태에서 BACK키 사용 시 A가 POP되고 B역시 NO_HISTORY에 의해 POP -> A만 남음.

    * FLAG_ACTIVITY_REORDER_TO_FRONT <br>
    호출 시 TASK내 이미 있으면, 같은 ACTIVITY는 POP시키고 해당 ACTIVITY가 PUSH됨.<br>
    A를 REORDER_TO_FRONT 설정 가정<br>
    A,B상태에서 A호출 시 같은 ACTIVITY인 A가 POP되고 -> B,A<br>

    * FLAG_ACTIVITY_CLEAR_TOP <br>
    해당 task에 있는 모든 activity를 pop시키고 해당 acitivity가 root activity로 task에 push됨.<br>
    A를 CLEAR_TOP설정 가정<br>
    A,B상태에서 A호출 시 모두 POP되고 -> A만 남음.<br>
    단, 해당 플래그는 액티비티를 모두 onDestroy()시킨 후 새롭게 onCreate() 시키기 때문에<br>
    A를 유지하려면 FLAG_ACTIVITY_SINGLE_TOP 플래그와 함께 사용하면 됨.<br>

  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
        Listener 
    </summary>
  
  - __정의__<br>
  이벤트 리스너라고 부르며, 이것은 특정 이벤트를 처리하는 인터페이스다. 이벤트 발생 여부를 기다리다가, 발생시 특정 이벤트를 처리하는 객체이다.<br>

  - __종류__<br>
  ![eventListener](./img/eventListener.png)


  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
      WebView 
    </summary>

  * WebView 
    - __정의__<br>
    각 OS별 내장된 웹 브라우저를 뷰형태로 앱에서 표현할 수 있는 방법이다. WebView를 이용하여 웹페이지를 앱내에서 호출하여 하이브리드 형태의 앱을 개발하는데 유용하게 사용이 가능하다. <br>
    앱 안에 HTML iframe을 넣었다고 생각하면 된다.
      
      [API공식문서](https://developer.android.com/reference/android/webkit/WebView)<br>

    - __종류__<br>
    1. UIWebView<br>
    2. WKWebView<br>
    3. SFSafariView<br>
    UIWebView와 WKWebView는 앱내에서 웹뷰를 보여주는 방식이며,<br>
    SFSafariView는 앱내에서 사파리 브라우저를 띄우는 형태로 보여준다.<br>

    - __기본속성__<br>
    ```java
    // 웹뷰 시작

    /* 필수요소 */
    mWebView = (WebView) findViewById(R.id.webView);

    mWebView.setWebViewClient(new WebViewClient()); // 클릭시 새창 안뜨게
    mWebSettings = mWebView.getSettings(); //세부 세팅 등록
    mWebSettings.setJavaScriptEnabled(true); // 웹페이지 자바스클비트 허용 여부
    mWebSettings.setSupportZoom(false); // 화면 줌 허용 여부
    mWebSettings.setCacheMode(WebSettings.LOAD_NO_CACHE); // 브라우저 캐시 허용 여부
    /* 필수요소 끝 */

    mWebSettings.setSupportMultipleWindows(false); // 새창 띄우기 허용 여부
    mWebSettings.setJavaScriptCanOpenWindowsAutomatically(false); // 자바스크립트 새창 띄우기(멀티뷰) 허용 여부
    mWebSettings.setLoadWithOverviewMode(true); // 메타태그 허용 여부
    mWebSettings.setUseWideViewPort(true); // 화면 사이즈 맞추기 허용 여부
    mWebSettings.setBuiltInZoomControls(false); // 화면 확대 축소 허용 여부
    web.setPluginState(WebSettings.PluginState.ON_DEMAND);    //플러그인을 사용할 수 있도록 설정
    mWebSettings.setLayoutAlgorithm(WebSettings.LayoutAlgorithm.SINGLE_COLUMN); // 컨텐츠 사이즈 맞추기
    web.setBlockNetworkImage(false);             // 네크워크 이미지의 리소스를 로드하지않음
    web.setLoadsImagesAutomatically(true);   // 웹뷰가 앱에 등록되어있는 이미지 리소스를 자동으로 로드하도록 설정
    web.setUseWidViewPort(true);        // wide viewport를 사용하도록 설정
    mWebSettings.setDomStorageEnabled(true); // 로컬저장소 허용 여부

    mWebView.loadUrl("https://github.com/fifabell/webNappQnA"); // 웹뷰에 표시할 웹사이트 주소, 웹뷰 시작
    ```

  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        SharedPreferences
    </summary>

  - __정의__<br>
    안드로이드 앱이 종료되면 앱이 가지고 있던 데이터는 사라지기 때문에 재실행시 필요한 데이터를 SharedPreferences를 통해 저장한다.<br>

    [API공식문서](https://developer.android.com/reference/android/content/SharedPreferences)<br>  

    Sharedpreferences는 단순히 디바이스의 내부에 xml 파일 형태로 key, value쌍의 값들을 저장한다.<br>
    그래서 파일 이름으로 SharedPreferences를 오픈한 후 key를 가지고 값을 찾거나 value를 저장하도록 되어있다.<br>

  - __예시__ <br>  
  <MainActivity.java>
  
  ```java
    public class MainActivity extends AppCompatActivity {
 
        private TextView textView1;
        private EditText editText;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            editText = (EditText)findViewById(R.id.edit1);
            textView1 = (TextView)findViewById(R.id.resultText1);
    
            //저장된 값을 불러오기 위해 같은 네임파일을 찾음.
            SharedPreferences sf = getSharedPreferences("sFile",MODE_PRIVATE);

            //text라는 key에 저장된 값이 있는지 확인. 아무값도 들어있지 않으면 ""를 반환.(2번째 인자는 default)
            String text = sf.getString("text","");
            textView1.setText(text);
    
        }
    
        @Override
        protected void onStop() {
            super.onStop();
    
            // Activity가 종료되기 전에 저장한다.
            //SharedPreferences 변수 선언. 기본모드로 설정.
            SharedPreferences sharedPreferences = getSharedPreferences("sFile",MODE_PRIVATE);
    
            // editor를 이용해서 값을 저장함.
            SharedPreferences.Editor editor = sharedPreferences.edit();
            String text = editText.getText().toString(); // 사용자가 입력한 저장할 데이터
            editor.putString("text",text); // key, value를 이용하여 저장하는 형태

            //다양한 형태의 변수값을 저장할 수 있다.
            //editor.putString();
            //editor.putBoolean();
            //editor.putFloat();
            //editor.putLong();
            //editor.putInt();
            //editor.putStringSet();
    
            //최종 커밋 (필수)
            editor.commit();
    
    
        }
    }


  ```

  [Top of page](#목차)
  </details>

  <details>
    <summary>
       singleton
    </summary>
  
  > 전역 변수를 사용하지 않고 객체를 하나만 생성 하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴 <br>

  들어가기에 앞서 용어설명 )<br>
  Thread safety란?<br>
  멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다.<br>
  
  싱글톤패턴의 종류는 크게 4가지가 있다.<br>
  1. Eagle Initialization (이른 초기화 방식)<br>
  싱글톤 객체를 instance라는 변수로 미리 생성해 놓고 사용하는 방식.<br>

  이 패턴은 AndroidStudio에서 쉽게 싱글톤패턴을 생성할 수 있다.<br>
  New -> class추가 -> singleton class 추가<br>
  ![newSingleton](./img/newSingleton.png)

  아래와 같은 코드가 생성됨...<Sample.java> <br>
  ```java
  public class Sample {
    private static final Sample ourInstance = new Sample();

    public static Sample getInstance() {
        return ourInstance;
    }

    private Sample() {}
  }
  ```

  * 장점 : static으로 생성된 변수에 싱글톤 객체를 선언했기 때문에 클래스 로더에 의해 클래스가 로딩 될 때 싱글톤 객체가 생성된다.<br> 또 클래스 로더에 의해 클래스가 최초 로딩 될 때 객체가 생성됨으로 Thread-safe.<br>
  * 단점 : 싱글톤 객체를 사용하든 안하든 해당 클래스가 로딩 되는 시점에 항상 싱글톤 객체가 생성(new) 되고 메모리를 차지하고 있으니 비효율적인 방법임.<br>

  2. Lazy Initialization (늦은 초기화 방식)

  ```java
  public class Sample {
    private static Sample instance;
    private Sample() {  }

    public static Sample getInstance(){
        if(instance == null){
            instance = new Sample();
        }
        return instance;
    }
  }
  ```

  * 장점 : 싱글톤 객체가 필요할 때 인스턴스를 얻을 수 있다. (Eager initialization 방식에 단점을 보완)<br>
  * 단점 : multi-thread 환경에서 여러 곳에서 동시에 getInstance()를 호출할 경우 인스턴스가 두 번 생성될 여지가 있다. (동기화 문제)==Not Thread-safe <br>

  3. Initialization on demand holder idiom (holder에 의한 초기화 방식)

  ```java
  // 클래스안에 클래스(Holder)를 두어 JVM의 Class Loader 매커니즘과 Class가 로드되는 시점을 이용한 방식

  public class Sample {
        // Private constructor prevents instantiation from other classes
        private Sample() { }

        /**
        * SampleHolder is loaded on the first execution of Singleton.getInstance() 
        * or the first access to SampleHolder.INSTANCE, not before.
        */
        private static class SampleHolder { 
                public static final Sample INSTANCE = new Sample();
        }
        public static Sample getInstance() {
                return SampleHolder.INSTANCE;
        }
  }
  ```

  가장 많이 사용하는 방법으로, Lazy initialization 장점을 가져가면서 Thread간 동기화문제를 동시에 해결한 방법이다.(Thread-safe)<br>

  4. Enum Initialization (Enum 초기화 방식)

  ```java
  // 모든 Enum type은 프로그램 내에서 한번 초기화 되는 점을 이용하는 방식.
  public enum Sample {
        INSTANCE;
        public void execute (String arg) {
                //... perform operation here ...
        }
  } 
  ```

  싱글톤의 특징(단 한번의 인스턴스 호출, Thread간 동기화) 을 가지며 비교적 간편하게 사용할 수 있는 방법<br>

  [Top of page](#목차)
  </details>

  <details>
    <summary> 
        problems 
    </summary>
  
  > _안드로이드 개발하면서 발생했던 문제들을 야기한다._<br>

  - 안드로이드 4.0 버전 한글 오류<br>

    해당 문제는 3.5버전 이후 한글관련 문제가 있었던걸로 파악이 된다.<br>
    구글링을 통해 한글처리 파일경로를 찾아 글씨체를 변경하였지만 바뀌지않는 문제가 있었다.<br>
    
    이후 해결책은 3.5버전 설치하여 업그레이드 하지않고 사용하고 있다. (문제없이 잘 작동된다.)<br>

  [사용예제_보러가기](https://github.com/fifabell/AndroidStudy/tree/master/sample/singleton_sample)<br>

  [Top of page](#목차)
  </details>
    
  <details>
    <summary> 
        etc 
    </summary>

  * HashMap
  hashmap을 이용해 키:배열 형태로 값을 저장할 수 있다.<br>
  추가로 arraylist를 이용해 hashmap을 배열로 담아 사용할 수도 있다.<br>
  
  ```java
  // 선언
  HashMap<String,String> map1 = new HashMap<String,String>();//HashMap생성
  HashMap<String,String> map2 = new HashMap<>();//new에서 타입 파라미터 생략가능
  HashMap<String,String> map3 = new HashMap<>(map1);//map1의 모든 값을 가진 HashMap생성
  HashMap<String,String> map4 = new HashMap<>(10);//초기 용량(capacity)지정
  HashMap<String,String> map5 = new HashMap<>(10, 0.7f);//초기 capacity,load factor지정
  HashMap<String,String> map6 = new HashMap<String,String>(){{//초기값 지정
      put("a","b");
  }};

  // 값 추가
  HashMap<Integer,String> map = new HashMap<>();//new에서 타입 파라미터 생략가능
  map.put(1,"사과"); //값 추가
  map.put(2,"바나나");
  map.put(3,"포도");

  // 값 삭제
  HashMap<Integer,String> map = new HashMap<Integer,String>(){{//초기값 지정
    put(1,"사과");
    put(2,"바나나");
    put(3,"포도");
  }};
  map.remove(1); //key값 1 제거
  map.clear(); //모든 값 제거
  
  // 출력
  HashMap<Integer,String> map = new HashMap<Integer,String>(){{//초기값 지정
    put(1,"사과");
    put(2,"바나나");
    put(3,"포도");
  }};
      
  System.out.println(map); //전체 출력 : {1=사과, 2=바나나, 3=포도}
  System.out.println(map.get(1));//key값 1의 value얻기 : 사과
      
  //entrySet() 활용
  for (Entry<Integer, String> entry : map.entrySet()) {
      System.out.println("[Key]:" + entry.getKey() + " [Value]:" + entry.getValue());
  }
  //[Key]:1 [Value]:사과
  //[Key]:2 [Value]:바나나
  //[Key]:3 [Value]:포도

  //KeySet() 활용
  for(Integer i : map.keySet()){ //저장된 key값 확인
      System.out.println("[Key]:" + i + " [Value]:" + map.get(i));
  }
  //[Key]:1 [Value]:사과
  //[Key]:2 [Value]:바나나
  //[Key]:3 [Value]:포도
  ```

  * static
  - 공간적 특성: 멤버는 클래스당 하나가 생성된다. <br>
            : 멤버는 객체 내부가 아닌 별도의 공간에 생성된다. <br>
            : 클래스 멤버 라고 부른다. <br>
  - 시간적 특성: 클래스 로딩 시에 멤버가 생성된다. <br>
            :객체가 생기기 전에 이미 생성된다. <br>
            :객체가 생기기 전에도 사용이 가능하다. (즉, 객체를 생성하지 않고도 사용할 수 있다.) <br>
            :객체가 사라져도 멤버는 사라지지 않는다. <br>
            :멤버는 프로그램이 종료될 때 사라진다. <br>

  * Callback
  - 일반적으로 callback함수는 매개변수에 함수를 넣어 반환하는 값이 함수의 return 값으로 들어오게 된다.<br>
    android의 경우, Callback interface를 생성해 사용가능하다.<br>
    아래는 예제.<br>
  <Callback.java(interface)>
  ```java
  Interface Callback(){
    void run(Object result);
  }
  ```

  <Callback_인터페이스가_쓰이는_어딘가..>
  ```java
  // case 1
  Callback callBack;
  private String receiveSTR = 'null';

  callBack.run(receiveSTR);

  // case 2
  public __AsyncTaskTest(HashMap<String, String> namevalue, String tmp_url, CallBack callBack, Context context) {
        try {
            this.context = context;
            url = new URL(tmp_url);
            nameValue = namevalue;
            this.callBack = callBack;
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
    }
  ```

  * OptionsMenu
  - 액션바 우측에 표시되는 메뉴바를 얘기한다.<br>
  ![OptionsMenu](./img/optionsmenu.png)
  
  ### 1) 
  /res/menu/ 경로에  Menu Resource를 위해 XML 파일을 생성,<br>
  /res/ 경로 밑에 menu Directory가 존재하지 않으면 추가를 하여 해당 Directory 하위에 XML 파일을 생성.<br> 
  ```java
  <?xml version="1.0" encoding="utf-8"?>
  <menu xmlns:android="http://schemas.android.com/apk/res/android">

      <item
          android:id="@+id/menu1"
          android:title="Menu1" />
      <item
          android:id="@+id/menu2"
          android:title="Menu2" />
      <item
          android:id="@+id/menu3"
          android:title="Menu3" />
  </menu>
  ```

  ### 2) 
  onCreateOptionsMenu() 함수를 오버라이딩<br>
  메뉴버튼을 눌렀을 때 보여줄 menu에 대해서 정의<br>

  ```java
  @Override
  public boolean onCreateOptionsMenu(Menu menu)
  {
      MenuInflater inflater = getMenuInflater();

      inflater.inflate(R.menu.main_menu, menu);

      return true;
  }
  ```

  ### 3) 
  onPrepareOptionsMenu() 함수를 오버라이딩 <br>
  추가 한 옵션 메뉴(Option Menu)를 클릭할 때마다 호출.<br>
  메뉴를 클릭하였을 때 따로 처리해야 할 내용이 있다면 이 함수안에 구현.(안해줘도 무방)<br>

  ### 4) 
  onOptionsItemSelected() 함수를 오버라이딩 <br>
  특정 Menu Item을 선택하였을 때 호출되는 함수.<br>
  매개변수로 선택 된 MenuItem의 객체가 넘어옴.<br>
  
  ```java
  @Override
  public boolean onOptionsItemSelected (MenuItem item)
  {
      Toast toast = Toast.makeText(getApplicationContext(),"", Toast.LENGTH_LONG);

      switch(item.getItemId())
      {
          case R.id.menu1:
              toast.setText("Select Menu1");
              break;
          case R.id.menu2:
              toast.setText("Select Menu2");
              break;
          case R.id.menu3:
              toast.setText("Select Menu3");
              break;
      }

      toast.show();

      return super.onOptionsItemSelected(item);
  }
  ```

  ### 5) 
  onOptionsMenuClosed() 함수를 오버라이딩 <br>
  옵션 메뉴(Option Menu)를 클릭하여 활성화 된 상태에서 이전 버튼을 클릭하거나 액티비티의 다른 영역을 클릭하여 옵션 메뉴를 닫을 때 호출되는 함수.<br>

  ![OptionSelected](./img/optionselected.png)
  
  * MVC
  
  ![mvc](./img/mvc.png)

  - 디자인 패턴 중 하나로, 사용자 인터페이스로부터 비즈니스 로직을 분리하여 애플리케이션의 시각적 요소나 그 이면에서 실행되는 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다.<br>
  
  - 구성
    * Controller : 모델에 명령을 보냄으로써 모델의 상태를 변경할 수 있다. (예: 워드 프로세서에서 문서를 편집하는 것) <br>
    또, 컨트롤러가 관련된 뷰에 명령을 보냄으로써 모델의 표시 방법을 바꿀 수 있다. (문서를 스크롤하는 것) <br>

    * Model : 모델의 상태에 변화가 있을 때 컨트롤러와 뷰에 이를 통보한다.<br>
    이와 같은 통보를 통해서 뷰는 최신의 결과를 보여줄 수 있고, 컨트롤러는 모델의 변화에 따른 적용 가능한 명령을 추가·제거·수정할 수 있다.<br>
    어떤 MVC 구현에서는 통보 대신 뷰나 컨트롤러가 직접 모델의 상태를 읽어 오기도 한다.<br>

    * View : 사용자가 볼 결과물을 생성하기 위해 모델로부터 정보를 얻어 온다.<br>

  * JSONParse

  - 기본적으로 json은 [], {}로 나뉨.<br>

  - 예시 <br>

  []대괄호는 JSONArray 를 이용하여 구별하고 {}중괄호는 JSONObject 를 이용하여 구별 <br>

  ```java
  
  // 서버에서 받아오는 result == json
  result = '[
    {"CODE":"A","NAME":"JEONG","AGE":"11"},
    {"CODE":"B","NAME":"CHOI","AGE":"12"}
  ]'; // 예를들어 이런 코드가 있을 때, 


  @Override
  public void postExecute(String result) {
    
      try {
        JSONArray jsonArr = new JSONArray(result); // JSONArray로 []를 벗긴다.

        ArrayList<String, String, String> itemList = new ArrayList<>();
        for(int i=0; i<jsonArr.length(); i++){
            JSONObject jsonObj = jsonArr.getJSONObject(i); // JSONObject로 {}를 벗긴다.
            String code = jsonObj.getString("CODE");
            String name = jsonObj.getString("NAME");
            String age = jsonObj.getInt("AGE");
            itemList.add(code, name, age);
        }

        // 보통 위 과정을 아래와 같이 ArrayList를 특정 class에 담아 형태를 객체화 시킨다.

        ArrayList<TestItem.java> itemList = new ArrayList<>();
        for(int i=0; i<jsonArr.length(); i++){
            itemList.add(new TestItem(jsonArr.getJSONObject(i)));
        }

        // in TestItem.java
        public class TestItem{
            private String code,name,age;
            // + getter, setter 
            public TestItem(JSONObject jsonObj) throws JSONException {
              // 해당 class의 지역변수 참고.
              code = jsonObj.getString("CODE");          // 코드
              name = jsonObj.getString("NAME");          // 이름
              age = jsonObj.getString("AGE");            // 나이
            }
        }
        // end TestItem.java

      } catch(JSONException e) {
        e.printStackTrace();
      }
  }

  ```

  * NavigationView
  메뉴 탭이라고 생각하면 됨.
  
  [NavigationView_샘플](https://github.com/fifabell/AndroidStudy/blob/master/sample/NavigationView_sample/app/src/main/java/com/example/navigationview/MainActivity.java)<br>
  
  주석부분 설명참고.


  * 직렬화
  
  직렬화를 해주면 intent(객체형태)로 전달할 수 있다.<br>
  
  직렬화는 메모리 내에 존재하는 정보를 보다 쉽게 전송 및 전달하기 위해 byte 코드 형태로 나열하는 것이다. <br>
  여기서 메모리 내에 존재하는 정보는 즉 객체를 말한다.<br>

  안드로이드에서 직렬화의 방법은 2가지가 있다.<br>
  
  Serializable vs Parcelable<br>

  - Serializable 는 java의 기본 인터페이스이며, 가볍게 사용하기 쉬운 (재정의함수가 필요없는) 인터페이스다.<br>
  - Parcelable 는 android에서 제공하는 인터페이스이며, 필수 재정의 함수가 존재한다.<br>

  기본 Parcelable 인터페이스의 구성은 아래와 같다.<br>

  ```java
  // 기본 item구성
  String name;
  int age;
  //  + getter setter

  // 함수 재정의에 앞서, 생성자와 Creator 클래스를 만들어줘야한다. (alt + enter를 통해 자동 생성)
  protected TestItem(Parcel in) {
      name = in.readString();
      age = in.readInt();
  }

  public static final Creator<TestItem> CREATOR = new Creator<TestItem>() {
      @Override
      public TestItem createFromParcel(Parcel in) {
          return new TestItem(in);
      }

      @Override
      public TestItem[] newArray(int size) {
          return new TestItem[size];
      }
  };

  // 두 함수는 필수 재정의 함수이다.
  @Override
  public int describeContents() {
      return 0;
  }

  @Override
  public void writeToParcel(Parcel dest, int flags) {

  }

  ```

  Serializable은 가볍게 쓸 수 있는 대신, 기본적으로 Parcelable보다 쓰레기요소가 많아 시간이 더 소요된다.<br>
  하지만, 이는 Serializable에서 함수를 추가적으로 구축하면 보다 더 짧은 시간으로 처리할 수 있다.<br>

  ```java
  private void writeObject(java.io.ObjectOutputStream out)
  throws IOException;
    
  private void readObject(java.io.ObjectInputStream in)
  throws IOException, ClassNotFoundException;

  private void readObjectNoData()
  throws ObjectStreamException;
  ```

  더 구체적인 사항은 최하단에 참고링크를 참조하자.<br>

  * FTPClient

  - 아래 다운로드 링크에서 Apache FTP Library 다운로드<br>
  - [다운로드링크](http://commons.apache.org/proper/commons-net/)<br>
  
  - app/lib에 jar파일 복사 <br>
    - commons-net-3.6-sources.jar<br>
    - commons-net-examples-3.6.jar<br>
    - commons-net-3.6.jar<br>

  - File > Project Structure 에서<br>
  Dependencies에 jar파일들 추가<br>

  - 아래 예시 코드 참고<br>

  ```java
 
  public void sendFTP(String fName)
  {
    final String parameter = fName;
    Thread thread = new Thread(new Runnable() {
        String fn = parameter;

        @Override
        public void run() {

            FTPClient ftpClient = new FTPClient();
            try {
                //ftpClient.setControlEncoding("MS949");
                ftpClient.connect("address", port);
                ftpClient.login("id", "passwd");
                ftpClient.setFileType(FTP.BINARY_FILE_TYPE); // 바이너리 파일
            } catch (Exception ex) {
                // error 
            }

            File uploadFile = new File(getApplicationContext().getFilesDir() + "/"+fn);

            FileInputStream fis = null;

            try{
                ftpClient.changeWorkingDirectory("/data"); //서버 접속 폴더 
                fis = new FileInputStream(uploadFile);
                boolean isSuccess = ftpClient.storeFile(uploadFile.getName(), fis);

                if(isSuccess){
                    // success
                }
                else {
                     // fail
                }
             }catch(Exception e){
                // exception error 
            }
        }
    });
    thread.start();
  }

  ```



  * fileprovider
  * viewPager

  * 팁
  - 1. xml_ headerlayout 설정 : layout의 header(윗부분)을 따로 layout을 만들 수 있음<br>
  예시)<br>

  <activity_main.xml>
  ```java
  <com.google.android.material.navigation.NavigationView
    android:id="@+id/navigation_view"
    app:headerLayout="@layout/drawer_header">
  </com.google.android.material.navigation.NavigationView>
  // 이후 drawer_header를 따로 정해주면 됨. 
  // drawer_header안에 id 사용 가능함.
  ```

  <android_header.xml>
  ```java
  ...
  <TextView
    android:id="@+id/textName"
    android:text="홍길동" />
  ...
  ```

  <MainActivity.java>
  ```java
  View headerView;
  TextView drName;

  headerView = navigationView.getHeaderView(0);
  drName = headerView.findViewById(R.id.textName);
  ```

  - 2. Manifest에서 가로모드로 전환<br>
  ```java
  <activity
    android:name=".XXXActivity"
    android:screenOrientation="landscape" /> // 이것.!
  ```
  
    
  [Top of page](#목차)
  </details>
    
---

### 2. 웹  🖥

  <details>
    <summary> 
        통신 
    </summary>
  
  타 업체와 api를 연결하다보면, postman의 raw형식으로 데이터가 들어올 때가 있다.<br>
  일반적인 형태는 form방식 전송을 이용하는 경우,<br>
  
  ```php
  // receive
  json_decode($_POST);
  $DATA_ARRAY = json_decode($DATA,true);
  $DATA_ARRAY["열NAME"];
  // receive-end
  ```

  하지만 c++나 c에서 프로그램을 이용해 값이 전달되는 경우 raw형식으로 종종 들어오곤 한다.<br>
  
  raw는 JSON이 아닌 날 것의 데이터형식으로,<br>
  fetch() 후, .json()을 해줘야 객체 형태로 값이 들어온다.<br>

  ```php
  // request
  {"DATA":
    {"h1":"1","h2":"2"}
  }
  // request-end

  // receive
  $json = file_get_contents('php://input'); // POST방식으로 보낸 http패킷의 body에 접근할 수 있다.
  $data = json_decode($json);

  //test
  echo $data->{"DATA"}->{"h1"};
  // receive-end
  ```

  ```php
  // 기타 팁

  // 한글변환 - 5.4이하버전 
  function han ($s) { 
    return reset(json_decode('{"s":"'.$s.'"}')); 
  } 
  function to_han ($str) {
  return preg_replace('/(\\\u[a-f0-9]+)+/e','han("$0")',$str); 
  } 

  // EUC-KR to UTF-8
  $DATA = json_decode($_POST);
  $DATA = mb_convert_encoding($DATA,"EUC-KR","UTF-8");
  
  // \제거
  $DATA = str_replace('\\', '', to_han(json_encode($DATA)));
  // 
  ```

  [Top of page](#목차)
  </details>


  <details>
    <summary> 
        javascript 기본문법
    </summary>

  ```javascript
    <script src="scripts/main.js"></script> // header

    // get the html
    let myHeading = document.querySelector('h1');
    myHeading.textContent = 'Hello world!';

    // onclick -1
    document.querySelector('html').onclick = function() {
      alert('Ouch! Stop poking me!');
    }

    // onclick -2
    guessSubmit.addEventListener('click', 함수명);

    // RANDOM 1~100
    var randomNumber = Math.floor(Math.random() * 100) + 1;

    // TEXT 변경/추가 
    guesses.textContent = 'TTTTTTT';

    // STYLE EDIT
    guesses.style.backgroundColor = 'yellow';
    guesses.style.fontSize = '200%';
    guesses.style.padding = '10px';
    guesses.style.boxShadow = '3px 3px 6px black';

  ```

  [Top of page](#목차)
  </details>

### 참고자료  🆘

  <details>
    <summary> 
        참고
    </summary>
  
  [context](https://www.charlezz.com/?p=1080)<br>
  [background](https://brunch.co.kr/@mystoryg/84)<br>
  [UrlConnection](https://goddaehee.tistory.com/161)<br>
  [LayoutInflater](https://www.crocus.co.kr/1584)<br>
  [Listener](https://m.blog.naver.com/PostView.nhn?blogId=netrance&logNo=110125233278&proxyReferer=https:%2F%2Fwww.google.com%2F)<br>
  [Task](https://arabiannight.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9CAndroid-%ED%83%9C%EC%8A%A4%ED%81%AC%EB%9E%80-Task-Activity-Stack-%EC%96%B4%ED%94%BC%EB%8B%88%ED%8B%B0%EB%9E%80-Android-Affinity-%ED%94%8C%EB%9E%98%EA%B7%B8%EB%9E%80)<br>
  [webview](https://medium.com/@pks2974/fads-9eea83f47607)<br>
  [SharedPreferences](https://bottlecok.tistory.com/26)<br>
  [OptionsMenu](https://lktprogrammer.tistory.com/161)<br>
  [Singleton](http://bictoselfdev.blogspot.com/2018/05/singleton.html)<br>
  [MVC](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC)<br>
  [직렬화](https://woovictory.github.io/2019/01/03/Android-What-is-serialization/)<br>

  [Top of page](#목차)
  </details>



