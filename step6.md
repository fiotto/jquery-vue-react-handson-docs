# STEP6 イベントの追加

## 行う内容
追加ボタンをクリックすることで、  
TODOリストに内容を追加する

## 実装
### jQuery
```diff
{% raw %}
      <div class="row">
        <input type="text" class="form-control" id="input-item"/>
-       <button type="button" class="btn-action">追加</button>
+       <button type="button" class="btn-action" id="button-add">追加</button>
      </div>
{% endraw %}
```

`on`メソッドでイベント処理を追加できる。

```diff
{% raw %}
+ 
+     // STEP 6
+     $('#button-add').on('click', function(){
+       const inputItem = $('#input-item').val();
+ 
+       $('#area-todo-normal').append(
+         $('<p></p>').append(
+           $('<button></button>', {
+             'type': 'button',
+             'class': 'btn-todo-normal'
+           }).append(inputItem)
+         )
+       );
+ 
+       $('#input-item').val('');
+       $('#input-item').trigger('input');
+     });
    </script>
{% endraw %}
```

### Vue.js
`v-on:イベント名`でイベント処理に使われるイベントを設定できる。  
※`v-on:イベント名`は`:イベント名`と省略することができる。  

```diff
{% raw %}
        <div class="row">
          <input type="text" class="form-control" v-model="inputItem"/>
-         <button type="button" class="btn-action">追加</button>
+         <button type="button" class="btn-action" v-on:click="addItem(inputItem)">追加</button>
        </div>
{% endraw %}
```

`methods`オブジェクトで、Vue.jsで使うメソッドを定義する。  
`data`や`methods`内のメソッドや値にアクセスするには  
`this`を使うことでアクセスできる。  
template(HTML)からの呼び出しでは、thisは不要    

```diff
{% raw %}
        data: function(){
          ･･･
-       }
+       },
+       methods: {
+         addItem: function(){
+           this.todos.normal.push(this.inputItem);
+           this.inputItem = '';
+         }
+       }
{% endraw %}
```

### React
値に代入するときは直接代入せずに、  
React.useStateで作成したsetter関数を使う  
値や、メソッドは対象のfunction(コンポーネント)内でスコープにする
現状の作りでは、objectの中まで監視するようにはなっていないので、
オブジェクトに変更を加えた後にsetする場合に、スプレッド構文と用いて、ディープコピーを作成する。

```diff
{% raw %}
      function App(){
        const isSelectdItem = Math.random() > 0.5;
-       const todos = {
+       const [todos, setTodos] = React.useState({
          high : ['掃除', '洗濯', '炊事'],
          normal: ['買い物', '草刈り', 'アイロン'],
          low: ['窓拭き', '振り込み', '家計簿管理']
-       };
+       });
        const [inputItem, setInputItem] = React.useState('');
 
+       function addItem(){
+         todos.normal.push(inputItem);
+         setTodos({...todos});
+         setInputItem('');
+       }
+
        return (
          <div className="container">
            <h1><span>{ title }</span>のサンプル</h1>
{% endraw %}
```

Vue.jsやそのままのJavaScriptと同じように属性に記述する。  
Reactの場合はイベントをcamelCaseにしたもの記述する。

```diff
{% raw %}
            <div className="row">
              <input type="text" className="form-control" 
                value={ inputItem }
                onChange={ (e) => setInputItem(e.target.value) }
              />
-             <button type="button" className="btn-action">追加</button>
+             <button type="button" className="btn-action" onClick={ addItem }>追加</button>

            </div>
{% endraw %}
```

[STEP5へ](step5.md)  
[STEP7へ](step7.md)  
