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

```diff
{% raw %}
+ 
+     // STEP 6
+     $('#input-item').on('input', function(){
+       $('#text-item').text($(this).val());
+     }).trigger('input');
+ 
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
```diff
{% raw %}
        <div class="row">
          <input type="text" class="form-control" v-model="inputItem"/>
-         <button type="button" class="btn-action">追加</button>
+         <button type="button" class="btn-action" v-on:click="addItem(inputItem)">追加</button>
        </div>
{% endraw %}
```

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
