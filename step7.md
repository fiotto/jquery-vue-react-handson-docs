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

```diff
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
```

```diff
+     // STEP 7
+     $(document).on('click', '.btn-todo', function(){
+       $('.btn-todo').removeClass('selected');
+       $(this).addClass('selected');
+     });
    </script>
```

- STEP7-2

```diff
            <p class="text-center">
-             <span class="color-red">洗濯</span>の重要度を
+             <span class="color-red" id="text-selected-item">___</span>の重要度を
            </p>
```

```diff
      // STEP 3
-     const isSelectdItem = Math.random() > 0.5;
-
-     $('#text-is-selectd-item').text(String(isSelectdItem));
-     isSelectdItem ? $('#area-is-selectd-item').show() : $('#area-is-selectd-item').hide();
```

```diff
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

```

### Vue.js
- STEP7-1

```diff
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
```

```diff
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
```

```diff
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
```

- STEP7-2

```diff
          return {
            title: 'Vue.js',
-           isSelectdItem: Math.random() > 0.5,
            todos: {
```

```diff
        },
+       computed: {
+         isSelectdItem: function(){
+           return this.selectedItem !== null;
+         }
+       },
        methods: {
```

```diff
            <div class="card">
              <p class="text-center">
-               <span class="color-red">洗濯</span>の重要度を
+               <span class="color-red">{{ selectedItem }}</span>の重要度を
              </p>
```

### React
- STEP7-1

```diff
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
```

```diff
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
```

- STEP7-2

```diff
      function App(){
-       const isSelectdItem = Math.random() > 0.5;
        const [todos, setTodos] = React.useState({
          high : ['掃除', '洗濯', '炊事'],
          normal: ['買い物', '草刈り', 'アイロン'],
          low: ['窓拭き', '振り込み', '家計簿管理']
        });
```

```diff
        function onClickTodo(todo){
          setSelectedItem(todo);
        }
  
+       function isSelectdItem(){
+         return selectedItem !== null;
+       }
+
        return (

```

```diff
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
```

[STEP6へ](step6.md)  
[STEP8へ](step8.md)  
