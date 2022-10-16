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

### @RestController
- これをつけたcontrollerのメソッドは@responseBodyをつけなくても戻り値がコンテンツになる
