# Angular 특수문자 정리

`<child-cmp #child1 [childname]="'자식1'"></child-cmp>`

#### # 기호의 의미

**참조변수의 의미**이다. 참조변수가 #child1이라고 선언돼 있을때 childname을 참조하려면 {{child1.childname}}과 같이 접근한다.



#### []기호의 의미

**클래스로 해당 데이터를 전달**함. 클래스에서는 @Input()을 통해 해당 값을 받음.

@Input() childname: string;



#### ()기호의 의미

<button (click)="updateParent(active)">부모에게 이벤트보내기</button>

1. (click) 이벤트로 쓰이는듯.

2. `@Output() outputProperty = new EventEmitter<boolean>();`

   `<child (outputProperty)="outputEvent($event)"></child>`

   @Output()장식자로 선언한 변수와 동일한 속성명.

