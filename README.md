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
