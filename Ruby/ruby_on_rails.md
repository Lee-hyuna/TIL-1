# Ruby On Rails

레일즈는 루비의 웹 프레임워크 이다. 이 게시글은 mysql 기반으로 셋팅

## 레일즈 지향점

- DRY(Don't Repeat Yourself! 반복하지 말것)
- 설정보다는 관습 : 설정파일을 최소화
- REST 패턴 지향
- MVC 아키텍처

## 레일즈 시작

- 다운로드

https://rubyinstaller.org/downloads/ 

OS에 맞는 루비와, Development Kit를 다운로드.

- 설치

루비와 Development Kit을 설치.

- Development Kit

압축을 풀고, `msys.bat`파일을 실행. 

> $ ruby dk.rb init
>
> $ ruby dk.rb install

루비 경로 에러가 나온다면 `config.yml`파일에서 자신의 ruby 경로를 `- C:\Ruby23-x64`과 같은 패턴으로 입력 후 다시 커맨드를 입력해볼 것.

- 레일즈 설치

> $ gem install rails

- 프로젝트 만들기

*MySql 사용시*

> $ gem install mysql2
>
> $ rails new blog -d mysql

*SQLite3 사용시*

> $ rails new blog 

- 생성되는 파일

| 파일/폴더     | 목적                                       |
| --------- | ---------------------------------------- |
| Gemfile   | 이 파일은 여러분의 레일즈 어플리케이션에게 필요한 젬의 의존성 정보를 기술하는데 사용됩니다. |
| README    | 이 파일은 어플리케이션을 위한 짧막한 설명입니다. 설치, 사용 방법 기술에 쓰입니다. |
| Rakefile  | 이 파일은 터미널에서 실행할 수 있는 배치잡들을 포함합니다.        |
| app/      | 어플리케이션을 위한 컨트롤러, 모델, 뷰를 포함합니다. 이 가이드에서는 이 폴더에 집중할 것 입니다. |
| config/   | 어플리케이션의 실행 시간의 규칙, 라우팅, 데이터베이스 등 설정을 저장합니다. |
| config.ru | 랙(Rack) 기반의 서버들이 시작할때 필요한 설정 입니다.        |
| db/       | 현재 데이터베이스의 스키마를 볼 수 있습니다.(데이터베이스 마이그레이션으로 잘 알려져 있습니다.) 여러분은 마이그레이션에 대해서 간단하게 배우게 됩니다. |
| doc/      | 어플리케이션에 대한 자세한 설명 문서입니다.                 |
| lib/      | 어플리케이션을 위한 확장 모듈입니다. (이 문서에서 다루지 않습니다.)  |
| log/      | 어플리케이션의 로그 파일입니다.                        |
| public/   | 외부에서 볼수 있는 유일한 폴더 입니다.이미지, 자바스크립트, 스타일시트나 그외 정적인 파일들은 이곳에 두세요. |
| script/   | 레일즈 스크립트를 포함합니다. 여러분의 어플리케이션을 실행시키거나, 배포, 실행 관련한 스크립트를 두세요. |
| test/     | 유닛 테스트, 픽스쳐, 그와 다른 테스트 도구들 입니다. 이 부분은 [레일즈 어플리케이션 테스트하기](http://rubykr.github.io/rails_guides/testing.html) 가 담당합니다. |
| tmp/      | Temporary files                          |
| tmp/      | 임시 파일                                    |
| vendor/   | 서드 파티 코드들을 위한 공간입니다. 일반적인 레일즈 어플리케이션은 루비 젬과 레일즈 소스-프로젝트 내에 설치시-와 미리 패키징된 추가 플러그인들이 위치합니다. |

- 필요한 젬 설치

> $ bundle install

레일즈는 젬의 의존성을 번들러를 통해 관리함. 생선된 Gemfile에 기술된 젬을 위 커맨드로 설치할 수 있음.

- DB 설정

`config/database.yml`을 수정한다

````
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: DB 아이디
  password: DB 암호
  host: DB 주소
````

> $ rake db:create

위 커맨드 입력시 `databse.yml`에 정의된대로 데이터베이스가 생성

- 웹 서버 실행

> $ rails server

http://localhost:3000 에서 기본 페이지 확인가능.

## MVC 생성

- 시작페이지 생성

> $ rails generate controller home index

`app/views/home/index.html.erb` 에서 첫 페이지 수정 가능.

- 시작페이지 설정

`config/routes.rb` : 외부 요청과 컨트롤러, 액션을 연결하는 라우팅 파일.

>  root :to는 시작페이지의 컨트롤러, 액션을 설정

````ruby
Blog::Application.routes.draw do

  # root 액션을 home 컨트롤러의 index 액션과 연결
  root :to => "home#index" end
````

http://localhost:3000 에서 생성한 첫 페이지 확인가능.

- Scaffolding을 이용한 새로운 MVC 생성

> $ rails generate scaffold Post name:string title:string content:text

| 파일                                       | 목적                                       |
| ---------------------------------------- | ---------------------------------------- |
| db/migrate/20100207214725_create_posts.rb | 데이터베이스에 ‘posts’ 테이블 생성하는 마이그레이션 (여러분의 파일 이름은, 다른 타임 스템프 값을 가지고 있습니다.) |
| app/models/post.rb                       | Post 모델                                  |
| test/fixtures/posts.yml                  | 테스트를 위한 더미(Dummy) posts                  |
| app/controllers/posts_controller.rb      | Posts 컨트롤러                               |
| app/views/posts/index.html.erb           | 모든 posts 를 출력하는 index 뷰                  |
| app/views/posts/edit.html.erb            | 존재하는 post 를 수정하는 edit 뷰                  |
| app/views/posts/show.html.erb            | 단일 post를 보여주는 show 뷰                     |
| app/views/posts/new.html.erb             | 새로운 post 를 만들기 위한 new 뷰                  |
| app/views/posts/_form.html.erb           | post 를 수정하거나 새로 만드는데 사용되는 폼(form)을 저장하는 조각(partial) 파일 |
| app/helpers/posts_helper.rb              | post 뷰를 위한 헬퍼(Helper) 함수를 위한 파일          |
| test/unit/post_test.rb                   | posts 모델을 위한 유닛 테스트 파일                   |
| test/functional/posts_controller_test.rb | posts 컨트롤러를 위한 기능 테스트 파일                 |
| test/unit/helpers/posts_helper_test.rb   | posts 헬퍼(Helper)를 위한 유닛 테스트 파일           |
| config/routes.rb                         | posts 를 위한 라우팅 정보를 담은 수정된 라우팅 파일         |
| public/stylesheets/scaffold.css          | 발판(Scaffold) 뷰를 좀 더 미려하게 만드는 CSS 파일      |

- **테이블 생성**

 `*_create_posts.rb` 파일에서, 테이블의 네이밍과 속성값들을 수정할 수 있음.

> $ rake db:migrate

정의된 파일대로 DB 생성.

- **생성한 MVC 링크 추가**

`app/views/home/index.html.erb` 에 링크 추가

````ruby
# 리스트 페이지 링크 : <a href="/posts">My Blog</a>
<%= link_to "My Blog", posts_path %>

# (참고) 단일 페이지 링크 : <a href="/profiles/1">Profile</a>
<%= link_to "Profile", profile_path(@profile) %>
````

link_to는 레일즈의 뷰 헬퍼 내장 메서드로 html 코드를 생성함.

## 모델

`app/models/post.rb` : ActiveRecord::Base를 상속. 기본 데이터베이스 CRUD, 읽기/갱신/삭제, 데이터 검증, 검색등의 기본 기능을 지원.

- 모델에 데이터 검증 추가

````ruby
class Post < ActiveRecord::Base
  validates :name,  :presence => true
  validates :title, :presence => true,
                    :length => { :minimum => 5 }
end
````

post는 이름과, 제목을 가지고 가지고 있어야하며, 제목의 길이는 최소 5글자 이상.

> $ rails console
>
> \>\> p = Post.new(:content => "A new post")
>
> \>\> p.errors

콘솔에 접근해서, validation이 동작하는 것을 확인할 수 있음.

## 컨트롤러

`app/controllers/posts_controller.rb`

- 전체 리스트

````ruby
  # GET /posts
  # GET /posts.json
  def index
    @posts = Post.all
  end
````

`@posts`는 PostsController의 인스턴스 변수, `Post.all`은 DB의 모든 글을 Post 모델로 반환하는 메서드.

- 새 포스트 작성

````ruby
  # POST /posts
  # POST /posts.json
  def create
    @post = Post.new(post_params)

    respond_to do |format|
      if @post.save
        format.html { redirect_to @post, notice: 'Post was successfully created.' }
        format.json { render :show, status: :created, location: @post }
      else
        format.html { render :new }
        format.json { render json: @post.errors, status: :unprocessable_entity }
      end
    end
  end

private
  def post_params
    params.require(:post).permit(:name, :title, :content)
  end
````

1. 사용자가 글 내용입력후 완료 버튼을 누르면 컨트롤러의 create에게 파라미터를 보낼것.
2. post_param 기반으로 새로운 Post 객체 생성.
3. post가 성공적으로 저장된 이후에 @post.save 실행
4. 성공시, 작성한 @post 화면으로 리다이렉트, 성공 메세지를 flash에 저장.

> 저장 : @post.save
>
> 수정 : @post.update_attributes(post_params) 
>
> 삭제 : @post.destroy

## 뷰

#### 리스트 페이지

`app/views/posts/index.html.erb`

````ruby
<h1>Listing posts</h1>
 
<table>
  <tr>
    <th>Name</th>
    <th>Title</th>
    <th>Content</th>
    <th></th>
    <th></th>
    <th></th>
  </tr>
 
<% @posts.each do |post| %>
  <tr>
    <td><%= post.name %></td>
    <td><%= post.title %></td>
    <td><%= post.content %></td>
    <td><%= link_to 'Show', post %></td>
    <td><%= link_to 'Edit', edit_post_path(post) %></td>
    <td><%= link_to 'Destroy', post, :confirm => 'Are you sure?', :method => :delete %></td>
  </tr>
<% end %>
</table>
 
<br />
 
<%= link_to 'New post', new_post_path %>
````

- 뷰 헬퍼 link_to를 사용하여 링크 생성
- edit_post_path와 new_post_path는 레일즈가 제공하는 RESTfull 라우팅 기능

#### 레이아웃

- `app/views/layouts/application.html.erb`  : 전체 페이지들의 레이아웃을 설정.

````ruby
<!DOCTYPE html>
<html>
<head>
  <title>Blog</title>
  <%= stylesheet_link_tag :all %>
  <%= javascript_include_tag :defaults %>
  <%= csrf_meta_tags %>
</head>
<body style="background: #EEEEEE;">
 
<%= yield %>
 
</body>
</html>
````

#### 레일즈 Partials

조각(partial)은 HTML과 루비 코드의 조각으로, 재사용가능.

- `new.html.erb` : Post 작성페이지, partial을 사용

````ruby
<h1>New post</h1>
<%= render 'form' %> # partial 사용
<%= link_to 'Back', posts_path %>
````

- `views/posts/_form.html.erb`

````ruby
<%= form_for(@post) do |f| %>
  <% if @post.errors.any? %>
  <div id="errorExplanation">
    <h2><%= pluralize(@post.errors.count, "error") %> prohibited this post from being saved:</h2>
    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
  <% end %>
 
  <div class="field">
    <%= f.label :name %><br />
    <%= f.text_field :name %>
  </div>
  <div class="field">
    <%= f.label :title %><br />
    <%= f.text_field :title %>
  </div>
  <div class="field">
    <%= f.label :content %><br />
    <%= f.text_area :content %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>
````

form_for을 통해 HTML폼을 생성. `f.text_field :name` 는 <input type="text" name="name" value="f.name"/> 와 같이 생성.

모델에 가지고 있는 속성 값으로만 이러한 메서드를 사용할 수 있음.

`f.submit`은 뷰 헬퍼를 사용하여 서브밋 버튼을 만들어줌.

````ruby
<%= f.submit "버튼이름", :type => "reset" %>
# <input name="commit" type="reset" value="버튼이름" />
````



## Refernece

레일즈 시작하기 : http://rubykr.github.io/rails_guides/getting_started.html

뷰 헬퍼 가이드 : https://apidock.com/rails/v2.1.0/ActionView/Helpers/UrlHelper/link_to

submit 뷰헬퍼 : http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html

https://www.digitalocean.com/community/tutorials/how-to-use-mysql-with-your-ruby-on-rails-application-on-ubuntu-14-04