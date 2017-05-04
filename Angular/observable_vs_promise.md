# Observable vs Promise

Obserable과 Promise는 비동기 프로그래밍을 위해 사용된다.

어떤 차이점이 있는지 알아보자.



## 차이점

둘다 비동기 프로그래밍을 위해 사용되지만,

차이점이 존재한다.



- Promise

하나의 이벤트만 핸들링이 가능.

Subscribe(콜백 모니터링) 취소가 불가능.



- Observable

0 ~ N개 의 이벤트 핸들랭이 가능

Unscribe를 통해, Subscribe 취소가 가능.

`Stream` 방식으로 코딩작성이 가능.

`map`, `forEach`, `reduce` 의 명령어를 제공



Promise보다 더 강력한 기능이 제공되므로

Observable이 더 많이 사용되는 추세라고 한다.



## Promise 사용법

request 성공시 .then()이 실행된다.

```javascript
import { Injectable } from '@angular/core';
import { URLSearchParams, Jsonp } from '@angular/http';

@Injectable()
export class WikipediaService {
  constructor(private jsonp: Jsonp) {}
  search (term: string) {
    var search = new URLSearchParams()
    search.set('search', term);
    return this.jsonp
                .get('http://en.wikipedia.org/w/api.php?callback=JSONP_CALLBACK', { search })
                .toPromise()
                .then((response) => response.json());
  }
}
```

````javascript
export class AppComponent {
  items: Array<string>;

  constructor(private wikipediaService: WikipediaService) {}

  search(term) {
    this.wikipediaService.search(term)
                         .then(items => this.items = items);
  }
}
````

예제 [Plunker](http://plnkr.co/edit/8ap1Lm?p=preview)



## Observable 사용법

- 서비스

```javascript
import { Injectable } from '@angular/core';
import { Http, Response } from '@angular/http';
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/map';
import { Schedule } from '../models';

@Injectable()
export class ScheduleService {
	constructor(public http: Http) { }
	private baseUrl = CommonTS.BASE_URL;

	getScheduleList(): Observable<Schedule[]> {
      return this.http
          .get(this.baseUrl + '/schedule/list.do')
          .map((res: Response) => res.json());
	}
}
```

- 서비스 실행

````javascript
        const subscription = this.scheduleService.getScheduleList().subscribe(
            schedules => {
                this.schedules = schedules;
                subscription.unsubscribe();
            }, (err) => {
                console.log(err);
                  return err;
            });
````




## Reference

* stackoverflow의 [trungk18](http://stackoverflow.com/users/3375906/trungk18) 님 답변





