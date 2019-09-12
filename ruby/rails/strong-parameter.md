# Strong Parameters

## 概要
詳しくは公式サイトを参照  
https://railsguides.jp/action_controller_overview.html#strong-parameters  

POST や PUT/PATCH 等でユーザが更新する場合は明に項目を指定しないといけないよね？っていうのを担保する仕組み。  

## example
`ActiveModel` や `ActiveRecord` で明に `initialize` するときは、 `permit` しないといけない。

```ruby
## permit で明に項目を指定する
User.new(params.require(:user).permit(:name, :address))

## permit しないとエラーになる
User.new(params.require(:user)) # => ActiveModel::ForbiddenAttributesError
```

裏では `ActionController::Parameters` を `new` して、 `permitted?` でチェックしてるっぽい。

```ruby
## Hash で明に指定するのは問題なし
User.new(name: 'ino_h', address: 'Matsue')

## もちろん自分でパラメタ作って渡すとエラー
params = ActionController::Parameters.new(name: 'ino_h', address: 'Matsue')
params.permitted? # => false
User.new(params) # => ActiveModel::ForbiddenAttributesError

## permit 状態にすれば問題ない
params.permit!
params.permitted? # => true
User.new(params)
```
