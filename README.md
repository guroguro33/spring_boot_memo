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
- 依存注入するclass用のモック

### @Profile("test")

### @ActiveProfiles("test")

