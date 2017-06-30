# Javascript String으로 Element 생성

string으로 DOM element를 생성해보자

**Template Element를 이용한다**

````typescript
const html = "<li>1234</li>";

// element를 생성하여 추가합니다.
const template = document.createElement('template');
template.innerHTML = html;
document.getElementsByClassName("collection")[0].appendChild(template.content.firstChild);
````



## Reference

https://stackoverflow.com/questions/494143/creating-a-new-dom-element-from-an-html-string-using-built-in-dom-methods-or-pro