# Angular2 login

앵귤러2의 가드를 사용해서, 로그인 서비스를 만들어보자.

## 로그인 페이지

- **로그인 컴포넌트**

ngIf 지시자를 활용해 화면 표시 상태 결정.

````typescript
import { Component } from '@angular/core';
import { Router, NavigationExtras } from '@angular/router';
import { AuthService } from './auth.service';

@Component({
  template: `
    <h3>LOGIN</h3> {{message}}
    <p>
      <input type="text" [(ngModel)]="userId" placeholder="사용자 아이디" *ngIf="!authService.isLogin">
      <button (click)="login()"  *ngIf="!authService.isLogin">로그인</button>
      <button (click)="logout()" *ngIf="authService.isLogin">로그아웃</button>
    </p>`
})
export class LoginComponent {
  message: string; userId: string;

  constructor(public authService: AuthService, public router: Router) {
    this.setMessage();
  }

  setMessage() {
    this.message = (this.authService.isLogin ? this.authService.userId + '님 환영합니다.' : '로그인 필요');
  }

  private doLogin() {
    this.setMessage();
    if (this.authService.isLogin) {
      let redirect = this.authService.redirectUrl ? this.authService.redirectUrl : '/admin';
      let navigationExtras: NavigationExtras = {
        preserveQueryParams: true,
        preserveFragment: true
      };
      this.router.navigate([redirect], navigationExtras);
    } else {
      alert('로그인을 할 수 없습니다.');
    }
  }

  login() {
    if (!this.userId) {
      alert('id를 입력해주세요');
      return;
    }
    this.message = '로그인을 진행해주세요';

    return this.authService.checkId(this.userId).then(children => {
      if (children) {
        this.authService.login(this.userId).subscribe(() => this.doLogin());
      } else {
        alert('아이디가 없습니다');
      }
      this.setMessage();
    });
  }

  logout() {
    this.authService.logout();
    this.setMessage();
  }
}
````

router의 navigate 함수를 통해 로그인 성공시, 페이지 이동.

로그인 버튼을 누르면 `login()` 메서드가 실행, `authService.checkId()` 실행.

- **Auth 서비스**

````typescript
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs/Observable';
import 'rxjs/add/observable/of';
import 'rxjs/add/operator/do';
import 'rxjs/add/operator/delay';

export class User {
  constructor(public id: number, public name: string) { }
}

const USERS = [
  new User(1, '첫번째 사용자'),
  new User(2, '두번째 사용자'),
  new User(3, '세번째 사용자')
];

export let userPromise = Promise.resolve(USERS);

@Injectable()
export class AuthService {
  isLogin: boolean = false;
  redirectUrl: string;
  userId: string;

  checkId(userId: string): Promise<User> {
    return userPromise
      .then(children => children.find(children => children.id === +userId));
  }

  login(userId: string): Observable<boolean> {
    return Observable.of(true).delay(500).do(val => this.isLogin = true).do(val => this.userId = userId);
  }

  logout(): void {
    this.isLogin = false;
  }
}
````

시뮬레이션을 위해 목객체 사용.

- **권한 접근 제어 : AuthGuard 서비스**

````typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router, ActivatedRouteSnapshot, RouterStateSnapshot, CanActivateChild, NavigationExtras, CanLoad, Route } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable()
export class AuthGuard implements CanActivate, CanActivateChild, CanLoad {
  constructor(private authService: AuthService, private router: Router) { }

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    let url: string = state.url;
    return this.checkLogin(url);
  }

  canActivateChild(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    return this.canActivate(route, state);
  }

  // lazy load 사용시, canLoad 사용.
  canLoad(route: Routce): boolean {
    let url = `/${route.path}`;
    if(window.confirm("자식 라우트가 모두 로드 되었습니다. 진행하시겠습니까?")){
      return this.checkLogin(url);
    }else{
      return false;
    }    
  }

  checkLogin(url: string): boolean {
    if (this.authService.isLogin) { return true; }
    this.authService.redirectUrl = url;
    let sessionId = 1234;

    let navigationExtras: NavigationExtras = {
      queryParams: { 'session_id': sessionId },
      fragment: 'anchor'
    };

    this.router.navigate(['/login'], navigationExtras);
    return false;
  }
}
````

URL에 세션 매개변수 정보를 전달하기 위해, NavigationExtras를 사용. session_id와 앵커를 url에 붙임. (결과 : http://localhostL4200/login?session_id=1234#anchor)

## Reference

쉽고 빠르게 배우는 Angular2 프로그래밍 - 정진욱 지음