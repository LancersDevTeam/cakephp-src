https://book.cakephp.org/3.0/ja/intro.html

# 概要
MiddlewareQueueを順番に動かしていって、出来たものを返す

# Application

## webroot/index.php
- App/Applicationインスタンス生成（appのconfigディレクトリを引数に与える）
    - Cake/BaseApplicationを継承していて、コンストラクタでは何もしない

- Applicationインスタンスを元にCake/Http/Serverインスタンスを生成
    - Cake/Http/Server/RunnerがServer内に設定される

- キックさせる

```
// Run the request/response through the application
// and emit the response.
$server->emit($server->run());
```

PSR-7に則って\Psr\Http\Message\ResponseInterfaceがrunの戻り値で返ってくるので、
それをemitに渡す

というわけで、ここからはserver#runから見ていきます

# CakePHP

## Server#run
- appのconfigディレクトリのbootstrap.phpを読み込む

- Request / Responseインスタンスの生成

- 新しいMiddlewareQueueをApplication#middlewareに渡す
    - 途中でMiddlewareQueue以外にしちゃうと怒られます
    - このMiddlewareQueueがこの後の一連の流れの全てです

- `Server.buildMiddleware`イベントが発火される

- 最後にApplication自身もMiddlewareQueueにaddされる

- RunnerがMiddlewareQueueを一つずつ動かしていき、最後に帰ったものをResponseInterfaceで返す

## デフォルトどおりだと
- ErrorHandlerMiddleware
    - 以降のMiddlewareでの例外をcatchして動く処理

- AssetMiddleware
    - AssetあったらResponse。無かったら次

- RoutingMiddleware
    - こいつ自体がResponseを返すのではなく、RoutingルールをRequestに設定する（あんま読んでない）
    - Contorllerを作れるなら作る

- BaseApplication
    - ActionDispatcher#dispatchが呼ばれる

## ActionDispatcher#dispatch
- `Dispatcher.beforeDispatch`の発火

- Controllerの生成（前段で作られていればそれ）

- ActionDispatch#_invokeにContollrer渡すとResponseが返ってくる

- `Dispatcher.afterDispatch`の発火

## ActionDispatcher#_invoke
- `Dispatcher.invokeController`イベントの発火

- `$controller->startupProcess`
    - Controller系のイベント発火

- `$controller->invokeAction`
    - ControllerのAction呼ぶ

- Actionから何も返ってこなくて、autoRender=trueの場合、controller#render

- Actionから何も返ってこなくて、autoRenderじゃない場合、そのままresponse

- `$controller->shutdownProcess`呼んでそれ返す
    - Controller系のイベント発火


## Controller#render
- 実体はViewVarsTrait
    - の中でViewBuilderが頑張る（実質singleton）
    - ViewVartTrait#createView
        - ViewBuilder#buildでViewインスタンスが返る

- 返ってきたViewのrender呼ぶ

## View#render
- の中の_render
 - の中の_evaluate（究極）
