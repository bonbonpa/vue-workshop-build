# Vue work shop building app

src : [link source](https://jayway.github.io/vue-js-workshop/docs)

# Chap 1. Create you first Vue.js project

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Hello Vue</title>
    <script src="https://unpkg.com/vue"></script>
    <script type="text/javascript">
      document.addEventListener('DOMContentLoaded',function9){
        new Vue({
          el: '#app',
          data: {
            message: 'Hello Vue.'
          }
        });
      });
    </script>
  </head>
  <body>
      <div id="app">
        {{message}}
      </div>
  </body>
</html>
```
