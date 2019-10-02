# STEP8 値の変更

## 行う内容
上げる、下げるボタンの実装  
重要度大のアイテムをクリックしたら上げるを、Disableに  
重要度小のアイテムをクリックしたら下げるを、Disableに

## 実装

### jQuery
```diff
            <p>
-             <button type="button" class="btn-action" disabled>上げる</button>
+             <button type="button" class="btn-action button-priority"
+               id="button-priority-up"
+             >上げる</button>
-             <button type="button" class="btn-action">下げる</button>
+             <button type="button" class="btn-action button-priority"
+               id="button-priority-down"
+             >下げる</button>
            </p>
```

`` `.btn-todo-high:contains("${selecedItem}")` ``はテンプレート文字列であり、文字列内挿機能をそなえている。  
`:contains`で文字列を含む要素を抽出できるので、これで含まれているかどうかの判定ができる。  

```diff
      $(document).on('click', '.btn-todo', function(){
+       const selecedItem = $(this).text();
+
        $('.btn-todo').removeClass('selected');
        $(this).addClass('selected');
 
        $('#area-is-selectd-item').show();
        $('#text-is-selectd-item').text(String(true));
-       $('#text-selected-item').text($(this).text());
+       $('#text-selected-item').text(selecedItem);
+
+       // STEP 8
+       $('#button-priority-up').prop('disabled', 
+          $(`.btn-todo-high:contains("${selecedItem}")`).length > 0);
+
+       $('#button-priority-down').prop('disabled', 
+         $(`.btn-todo-low:contains("${selecedItem}")`).length > 0);
      });
+
+     // STEP 8
+     $('.button-priority').on('click', function(){
+       const selecedItem = $('.btn-todo.selected').text();
+
+       const signum = $(this).attr('id') === 'button-priority-up' ? 1 : -1;
+
+       const keys = Object.keys(todos);
+       for(let idx in keys){
+         const key = keys[idx];
+
+         const $elm = $(`.btn-todo-${key}:contains("${selecedItem}")`);
+         if($elm.length > 0){
+           $elm.parents('p').remove();
+
+           const destIndex = (signum > 0) ? Number(idx)-1 : Number(idx)+1;
+           const dest = keys[destIndex];
+
+           $(`#area-todo-${dest}`).append(
+             $('<p></p>').append(
+               $('<button></button>', {
+                 'type': 'button',
+                 'class': `btn-todo btn-todo-${dest}`
+               }).append(selecedItem)
+             )
+           );
+
+           $('#area-is-selectd-item').hide();
+           $('#text-is-selectd-item').text(String(false));
+
+          break;
+        }
+      }
+    })
   </script>
```

### Vue.js
項目の一覧が配列になっているので`Array.prototype.includes()`で含まれているかどうかを確認できる。  

```diff
              <p>
-               <button type="button" class="btn-action" disabled>上げる</button>
+               <button type="button" class="btn-action"
+                 v-bind:disabled="todos.high.includes(selectedItem)"
+                 v-on:click="onClickPriority(1)"
+               >上げる</button>
-               <button type="button" class="btn-action">下げる</button>
+               <button type="button" class="btn-action"
+                 v-bind:disabled="todos.low.includes(selectedItem)"
+                 v-on:click="onClickPriority(-1)"
+               >下げる</button>
              </p>
```

```diff
          onClickTodo: function(todo){
            this.selectedItem = todo;
-         }
+         },
+         onClickPriority: function(signum){
+           const keys = Object.keys(this.todos);
+
+           for(let idx in keys){
+             const key = keys[idx];
+
+             if(this.todos[key].includes(this.selectedItem)){
+               this.todos[key] = this.todos[key].filter((a) => a !== this.selectedItem);
+
+               const destIndex = (signum > 0) ? Number(idx)-1 : Number(idx)+1;
+               const dest = keys[destIndex];
+               this.todos[dest].push(this.selectedItem);
+
+               this.selectedItem = null;
+               break;
+             }
+           }
+         }
        }
      })
```

### React
Vue.jsと同様に`Array.prototype.includes()`が使える。  
```diff
                    <p>
-                     <button type="button" className="btn-action" disabled>上げる</button>
+                     <button type="button" className="btn-action"
+                       disabled={ todos.high.includes(selectedItem) }
+                       onClick={ () => onClickPriority(1) }
+                     >上げる</button>
-                     <button type="button" className="btn-action">下げる</button>
+                     <button type="button" className="btn-action"
+                       disabled={ todos.low.includes(selectedItem) }
+                       onClick={ () => onClickPriority(-1) }
+                     >下げる</button>
                    </p>
```

```diff
        function isSelectdItem(){
          return selectedItem !== null;
        }

+       function onClickPriority(signum){
+         const keys = Object.keys(todos);
+
+         for(let idx in keys){
+           const key = keys[idx];
+
+           if(todos[key].includes(selectedItem)){
+             todos[key] = todos[key].filter((a) => a !== selectedItem);
+
+             const destIndex = (signum > 0) ? Number(idx)-1 : Number(idx)+1;
+             const dest = keys[destIndex];
+             todos[dest].push(selectedItem);
+
+             setSelectedItem(null);
+             break;
+           }
+         }
+
+         setTodos({...todos});
+       }
+
        return (
```

[STEP7へ](step7.md)  
[STEP9へ](step9.md)  
