# Spring boot 学習メモ

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
