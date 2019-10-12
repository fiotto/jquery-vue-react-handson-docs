# STEP7 属性のバインド

## 行う内容
DOMの属性を動的に変更する  
* 7-1  
最後にクリックしたアイテムのclassに「selected」を追加する  

* 7-2  
STEP3で作成した表示/非表示のトリガーをランダムから  
アイテムをクリックした状態であるかトリガーとする。

## 実装

### jQuery
- STEP7-1

今までと同様にイベントにクラスを追加する  
今回は、jQueryで動的に作成したボタンに追加するので、  
該当するJavaScriptの追加する。

```diff
{% raw %}
      $('#area-todo-high').empty();
      for(todo of todos.high){
        $('#area-todo-high').append(
          $('<p></p>').append(
            $('<button></button>', {
              'type': 'button',
-             'class': 'btn-todo-high'
+             'class': 'btn-todo btn-todo-high'
            }).append(todo)
          )
        );
      }

      $('#area-todo-normal').empty();
      for(todo of todos.normal){
        $('#area-todo-normal').append(
          $('<p></p>').append(
            $('<button></button>', {
              'type': 'button',
-             'class': 'btn-todo-normal'
+             'class': 'btn-todo btn-todo-normal'
            }).append(todo)
          )
        );
      }

      $('#area-todo-low').empty();
      for(todo of todos.low){
        $('#area-todo-low').append(
          $('<p></p>').append(
            $('<button></button>', {
              'type': 'button',
-             'class': 'btn-todo-low'
+             'class': 'btn-todo btn-todo-low'
            }).append(todo)
          )
        );
      }
{% endraw %}
```

動的に作成したボタンなので、イベント自体は  
ルートノードであるdocumentにイベントを設定する。  
```diff
{% raw %}
+     // STEP 7
+     $(document).on('click', '.btn-todo', function(){
+       $('.btn-todo').removeClass('selected');
+       $(this).addClass('selected');
+     });
    </script>
{% endraw %}
```

- STEP7-2

```diff
{% raw %}
            <p class="text-center">
-             <span class="color-red">洗濯</span>の重要度を
+             <span class="color-red" id="text-selected-item">___</span>の重要度を
            </p>
{% endraw %}
```

STEP3で乱数で生成して、表示/非表示を切り替え亭た部分を削除する。
```diff
{% raw %}
      // STEP 3
-     const isSelectdItem = Math.random() > 0.5;
-
-     $('#text-is-selectd-item').text(String(isSelectdItem));
-     isSelectdItem ? $('#area-is-selectd-item').show() : $('#area-is-selectd-item').hide();
{% endraw %}
```

初期状態は対象領域を非表示、クリックされた時に表示されるように文を記述する。  
同時に、変更する必要のある値を修正する。  

```diff
{% raw %}
      // STEP 7
+     $('#area-is-selectd-item').hide();
+     $('#text-is-selectd-item').text(String(false));
+
      $(document).on('click', '.btn-todo', function(){
        $('.btn-todo').removeClass('selected');
        $(this).addClass('selected');
+
+       $('#area-is-selectd-item').show();
+       $('#text-is-selectd-item').text(String(true));
+       $('#text-selected-item').text($(this).text());
      });
{% endraw %}
```

### Vue.js
- STEP7-1

クリックした値を格納する値と関数を用意する
```diff
{% raw %}
        data: function(){
          return {
            title: 'Vue.js',
            todos: {
              high: ['掃除', '洗濯', '炊事'],
              normal: ['買い物', '草刈り', 'アイロン'],
              low: ['窓拭き', '振り込み', '家計簿管理']
            },
-           inputItem: ''
+           inputItem: '',
+           selectedItem: null
          }
        },
{% endraw %}
```

```diff
{% raw %}
        methods: {
          addItem: function(){
            this.todos.normal.push(this.inputItem);
            this.inputItem = '';
-         }
+         },
+         onClickTodo: function(todo){
+           this.selectedItem = todo;
+         }
        }
{% endraw %}
```

`v-bind:属性名`で属性を動的に切り替えることができる。
`v-bind:class`ではobjectを指定することで、値によって、classをon/offを切り替える実装ができる。
※`v-bind:属性名`は`@属性名`と省略することができる。  

```diff
{% raw %}
            <tbody>
              <tr>
                <td>
                  <p v-for="todo in todos.high" v-bind:key="todo">
-                   <button type="button" class="btn-todo-high">{{ todo }}</button>
+                   <button type="button" class="btn-todo-high"
+                     v-bind:class="{ 'selected': todo === selectedItem }"
+                     v-on:click="onClickTodo(todo)"
+                   >{{ todo }}</button>
                  </p>
                </td>
                <td>
                  <p v-for="todo in todos.normal" v-bind:key="todo">
-                   <button type="button" class="btn-todo-normal">{{ todo }}</button>
+                   <button type="button" class="btn-todo-normal"
+                     v-bind:class="{ 'selected': todo === selectedItem }"
+                     v-on:click="onClickTodo(todo)"
+                   >{{ todo }}</button>
                  </p>
                </td>
                <td>
                  <p v-for="todo in todos.low" v-bind:key="todo">
-                   <button type="button" class="btn-todo-low">{{ todo }}</button>
+                   <button type="button" class="btn-todo-low"
+                     v-bind:class="{ 'selected': todo === selectedItem }"
+                     v-on:click="onClickTodo(todo)"
+                   >{{ todo }}</button>
                  </p>
                </td>
              </tr>
            </tbody>
{% endraw %}
```

- STEP7-2

```diff
{% raw %}
          return {
            title: 'Vue.js',
-           isSelectdItem: Math.random() > 0.5,
            todos: {
{% endraw %}
```

`computed`は、値に何らかの処理を与えたものキャッシュすることができる。
複数回値を使う場合に便利  
今回の場合では、`selectedItem`が変わるたび、   
内部で自動的に、値の再評価を行う。

```diff
{% raw %}
        },
+       computed: {
+         isSelectdItem: function(){
+           return this.selectedItem !== null;
+         }
+       },
        methods: {
{% endraw %}
```

```diff
{% raw %}
            <div class="card">
              <p class="text-center">
-               <span class="color-red">洗濯</span>の重要度を
+               <span class="color-red">{{ selectedItem }}</span>の重要度を
              </p>
{% endraw %}
```

### React
- STEP7-1

クリックした値を格納する値と関数を用意する

```diff
{% raw %}
        const [inputItem, setInputItem] = React.useState('');
+       const [selectedItem, setSelectedItem] = React.useState(null);

        function addItem(){
          todos.normal.push(inputItem);
          setTodos({...todos});
          setInputItem('');
        }

+       function onClickTodo(todo){
+         setSelectedItem(todo);
+       }

        return (
{% endraw %}
```

Reactのclass属性を動的にするには、文字列を作り直す必要がある。
```diff
{% raw %}
                <tbody>
                  <tr>
                    <td>
                      { todos.high.map((todo) => (
                        <p key={ todo }>
-                         <button type="button" className="btn-todo-high">{ todo }</button>
+                         <button type="button"
+                           className={ "btn-todo-high " + ( todo === selectedItem ? 'selected' : '' ) }
+                           onClick={ () => onClickTodo(todo) }
+                         >{ todo }</button>
                        </p>
                      )) }
                    </td>
                    <td>
                      { todos.normal.map((todo) => (
                        <p key={ todo }>
-                         <button type="button" className="btn-todo-normal">{ todo }</button>
+                         <button type="button"
+                           className={ "btn-todo-normal " + ( todo === selectedItem ? 'selected' : '' ) }
+                           onClick={ () => onClickTodo(todo) }
+                         >{ todo }</button>
                        </p>
                      )) }
                    </td>
                    <td>
                      { todos.low.map((todo) => (
                        <p key={ todo }>
-                         <button type="button" className="btn-todo-low">{ todo }</button>
+                         <button type="button"
+                           className={ "btn-todo-low " + ( todo === selectedItem ? 'selected' : '' ) }
+                           onClick={ () => onClickTodo(todo) }
+                         >{ todo }</button>
                        </p>
                      ))}
                    </td>
                  </tr>
                </tbody>
{% endraw %}
```

- STEP7-2

```diff
{% raw %}
      function App(){
-       const isSelectdItem = Math.random() > 0.5;
        const [todos, setTodos] = React.useState({
          high : ['掃除', '洗濯', '炊事'],
          normal: ['買い物', '草刈り', 'アイロン'],
          low: ['窓拭き', '振り込み', '家計簿管理']
        });
{% endraw %}
```

Vueの`computed`のようなものは存在しないため、
関数を定義し、template内で呼び出すようにすることで対応する。

```diff
{% raw %}
        function onClickTodo(todo){
          setSelectedItem(todo);
        }
  
+       function isSelectdItem(){
+         return selectedItem !== null;
+       }
+
        return (
{% endraw %}
```

```diff
{% raw %}
            <div className="tmp-space">
-             重要度の変更を表示 : <span>{ String(isSelectdItem) }</span>
+             重要度の変更を表示 : <span>{ String(isSelectdItem()) }</span>
            </div>
           
-           { isSelectdItem && (
+           { isSelectdItem() && (
              <div>
                <div className="content-center">
                  <div className="card">
                    <p className="text-center">
-                     <span className="color-red">洗濯</span>の重要度を
+                     <span className="color-red">{ selectedItem }</span>の重要度を
                    </p>
                    <p>
                      <button type="button" className="btn-action" disabled>上げる</button>
                      <button type="button" className="btn-action">下げる</button>
                    </p>
                  </div>
                </div>
              </div>
            )}
{% endraw %}
```

[STEP6へ](step6.md)  
[STEP8へ](step8.md)  
