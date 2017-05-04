# HTML 바인딩

- 방법 1

```
<div [innerHTML]="theHtmlString">
</div>
```

- 방법 2

 # id 부여

````typescript
<div #scheduleContents></div>
````

  # 컴포넌트 코드

````typescript
import { Component, ViewChild, ElementRef } from '@angular/core';

@Component({
    templateUrl: "some html file"
})
export class MainPageComponent {

    @ViewChild('dataContainer') dataContainer: ElementRef;

    loadData(data) { // 데이터를 불러오는 로직 추가
        this.dataContainer.nativeElement.innerHTML = data;
    }
}
````



## Reference

http://stackoverflow.com/questions/31548311/angular-2-html-binding