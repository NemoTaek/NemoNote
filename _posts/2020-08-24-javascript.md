---
title: "Javascript 개요"
tags:
  - javascript
---

Javascript는 HTML에 연결되어 기능을 넣어주는 스크립트 언어이다.  

- javaxcript를 적용하는 방법  
  + HTML 내부에 작성: `<style>`태그를 삽입하여 적용  
  <br>  
  
```
<!DOCTYPE html>
<html>
<body>
    <input type="button" id="hw" value="Hello world" />
    <script type="text/javascript">
        var hw = document.getElementById('hw');
        hw.addEventListener('click', function(){
            alert('Hello world');
        })
    </script>
</body>
</html>
```

  + HTML 외부에 작성: `<script src="js파일이름.js"></script>`  
  
기본적으로 `<script>` 태그를 만나면, 브라우저는 HTML 태그를 해석하는 것을 잠시 멈추고, javaxcript 파일을 불러와서 실행한다.  
javascript 적용을 head에 삽입하는 방법이 있고, body가 끝나기 전에 삽입하는 방법이 있다.  

- head에 삽입하는 방법  
<br>  

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="myScriptFile.js"></script>
</head>
<body>
  <div id="msg">Hello JavaScript!</div>
</body>
</html>
```

  + 브라우저 렌더링에 방해가 되어 무거운 스크립트가 실행되는 경우 오랫동안 완성되지 못한 화면을 노출하게 된다.  
  + 문서를 초기화하거나 설정하는 가벼운 스크립트들이 자주 사용된다.  
  + 문서의 DOM(Document Object Model) 구조가 필요한 스크립트의 경우 document.onload와 같은 로드 이벤트가 추가되어야 에러없이 작동된다.  

- body가 끝나기 전에 삽입하는 방법  
<br>  

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="msg">Hello JavaScript!</div>
  <script src="myScriptFile.js"></script>
</body>
</html>
```

  + 브라우저가 렌더링이 완료된 상태에서 스크립트가 실행되기에 콘텐츠를 변경하는 스크립트의 경우 화면에 노출된체로 변화된다.  
  + 대부분의 스크립트의 위치로 추천되는 위치이다.  
  + 문서의 DOM 구조가 완료된 시점에 실행되기에 별다른 추가설정이 필요없다.  


head에 삽입하면 html태그를 읽지 못하는 경우가 있으니, 웬만하면 html의 모든 태그를 읽고 body태그 끝나기 전에 삽입하는 방법을 사용하도록 하자.
