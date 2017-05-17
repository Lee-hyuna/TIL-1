# Mouse Over 이벤트 처리

## 지시자 선언

````typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[highlight]'
})
export class HighlightDirective {
  private el: HTMLElement;
  constructor(el: ElementRef) {
    this.el = el.nativeElement;
    this.el.style.fontSize = "30px";
  }
  @HostListener('mousemove') onMouseMove() {
    this.el.style.backgroundColor = "blue";
    this.el.style.color ="white";
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.el.style.backgroundColor = null;
    this.el.style.color ="black";
  }
}
````

## 템플릿

````typescript
import { Component } from '@angular/core';

@Component({
  selector: 'member-list',
  template: ` 
  <div highlight (mouseover)="setLike('like this')">
    {{like}}  
  </div>`
})
export class MemberListComponent {
  like: string;

  setLike(likethis: string){
    this.like = likethis;
  }
}
````

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱