# Spring boot 学習メモ

- Spring bootはWebサーバー内蔵方式のため、Tomcatを別途立てる必要はない

## Thymeleaf　
- htmlテンプレート
- th:〇〇で属性に埋め込みを行う
```
// 初期値がdefaultで表示されるが、変数messageがあればそちらを表示する
<p th:text="${message}">default</p>
```


## アノテーション
### @ResponseBody
- これをつけたメソッドは戻り値がそのままレスポンスのコンテンツになる
- JSONやXMLを返す時にも便利

### @RestController
- これをつけたcontrollerのメソッドは@responseBodyをつけなくても戻り値がコンテンツになる

### @RequestMapping
- @controllerのメソッドに追加する
- アドレスとメソッドを指定して、該当する場合に呼び出される
```
// URLが`/send`でmethodがpostの時にアノテーションをつけたメソッドが呼ばれる
@RequestMapping(value="/send", method="RequestMethod.POST") // methodなしの場合、value=""の表記を省略可能
```

### @RequestParam
- 引数につける
- 送られてきたフォームのタグのnameを指定し、引数として設定する
- 送られてきたオブジェクトからパラメータの値を取り出す処理は不要になるから便利
```
// text1という名前で送られてきたフォームの値をString型のstrとして引数をセット
def send(@RequestParam("text1")String str, ModelAndView mav) {
  // メソッドの内容
}
```

＠RestControllerAdvice
- REST API用のエラーハンドラの作成用
- @RestControllerAdviceを付与したクラスを作成し例外処理を行うメソッドを作成
- 各メソッドには処理したい例外を@ExceptionHandlerアノテーションで指定
```
@Slf4j  // ログ出力のためにLombokを使用
@RestControllerAdvice
public class ErrorHandlerController {

    @ExceptionHandler({AccessDeniedException.class})  // 処理したい例外
    @ResponseStatus(HttpStatus.FORBIDDEN)  // レスポンスのステータスコード、ここでは403
    public ErrorResponse handleException(AccessDeniedException e) {
        log.error("Error:", e.getMessage());
        return new ErrorResponse("notAuthorized", "The request was not authorized.");
    }

    @ExceptionHandler({EmployeeNotFoundException.class})  //  独自例外
    @ResponseStatus(HttpStatus.NOT_FOUND)  // レスポンスのステータスコード、ここでは404
    public ErrorResponse handleEmployeeNotFoundException(EmployeeNotFoundException e) {
        log.error("Error:", e.getMessage());
        return new ErrorResponse("notFound", "The Employee was not found.");
    }

    @ExceptionHandler({Exception.class})
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR) // レスポンスのステータスコード、ここでは500
    public ErrorResponse handleException(Exception e) {
        log.error("Error:", e.getMessage());
        return new ErrorResponse("systemError", "System error occurred.");
    }
}
```

## テスト系アノテーション
### @InjectMocks
- 依存注入されるclass用のモック

### @Mock
- Mockito
- 依存注入するclass用のモック
- 宣言するだけでMockオブジェクトが作成できる
```
// クラス変数の上に@Mockをつけることで、mockRepositoryにはモックオブジェクトがセットされる
@Mock
UserRepository mockRepository;
```

### @MockBean
- Spring Frameworkのアノテーション
- 通常のMockitoのモックを作成し、それをアプリケーションコンテキスト内に登録する
- そのため、@MockBeanで生成されたモックオブジェクトは、@Autowiredなど、SpringのDIのメカニズムを通じて注入される
```
@MockBean
UserRepository mockRepository;

// userServiceはmockRepositoryに依存しているためAutowiredで自動注入される
@Autowired
UserService userService;
```

### @Profile("test")
- テスト時にのみ呼ばれるメソッド
- テスト用クラス以外で使用する

### @ActiveProfiles("test")
- テスト時用のapplication.propartiesを読み込む？
- とりあえずテスト用クラスではこれを使うようだ

### @Before
- テスト時に各テストメソッドが呼ばれる前に実行される
```
@Before
    public static void setup() {
        // 毎テスト前に必要な処理を追加
    }
```

### @BeforeClass
- テスト時にテストクラスが呼ばれる前に1度だけ実行される
```
@BeforeClass
    public static void setup() {
        // 初回に必要な処理を追加
    }
```
