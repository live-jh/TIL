## Hugo Blog Setting Guide

블로그의 원격저장소는 2개

- 레이아웃 및 블로그 테마 저장소 (블로그 주소명)
- 블로그의 콘텐츠(파일)를 저장하는 저장소 (blog)



## 작업 순서

1. 현재 위치한 디렉토리에서 blog라는 폴더를 만들어준다 
2. blog 위치로 이동 후 hugo new site . 명령 (hugo site directory 생성) 
3. git init (저장소를 초기화) 
4. git remote add origin blog의 깃허브 http 주소.git (현재의 디렉토리를 log 원격저장소의 remote 로 등록) 
5. git remote -v 로 확인(origin으로 fetch와 push가 등록되어 있는지 확인) 
6. git pull origin main(현재 blog 디렉토리에서 원격저장소를 pull) 
7. cd themes 명령어를 이용해 themes 폴더로 이동 
8. git clone 내가 적용할 hugoThemes http 주소.git 입력 
9. theme를 적용할 config.toml 작성(각 테마별로 상이함) 
10. cd .. (blog폴더로 이동) 
11. git submodule add -b main [계정명.github.com.io](http://xn--989aw4xurk.github.com.io/).git public (깃허브 도메인을 가진 원격저장소를 public폴더에 생성하며 submodule로 등록) 
12. ls 확인 (readme.md와 public이 생성된걸 확인한다) 
13. hugo serve -D 로 로컬 서버 작동 확인 (localhost:1313)
14. hugo new posts/first-test.md 등으로 블로그 개시글 확인하기 
15. blog $ git status로 확인 (.gitmodules 와 public이 head file에 new로 존재해야함) 
16. untracked files엔 archetypes, config,toml, content, resources, themes등 존재함(blog 원격에 저장될 파일들) 
17. cat .gitmodules 로 submodule이 public으로 등록되어 있고 url이 **"githubID".github.io**인지 확인 
18. hugo -t 테마명 입력 
19. cd public 명령어 입력 public 폴더로 이동 
20. git add . 명령어 (커밋할 목록을 전체 선택) 
21. git status (현재 상태를 확인 -> html과 다양한 posts파일들이 존재) 
22. git commit -m “메세지 입력” 
23. git push origin main 
24. cd .. 명령어를 통해 blog 폴더로 나가기 
25. git status로 확인 
26. .gitmodules 와 public 폴더 확인 
27. git remote -v로 remote 경로 확인(blog.git 확인) 
28. git add . 명령어 (커밋할 목록을 전체 선택) 
29. git status (현재 상태를 확인 -> html과 다양한 posts파일들이 존재) 
30. git commit -m “메세지 입력” 
31. git push origin main 

>  [Hugo](https://gohugo.io/)는 다른 static 기반 시스템처럼 포스팅을 파일로 관리하고 포스팅들과 테마 설정 등을 툴이 알아서 믹싱하여 html베이스의 사이트로 컨버팅 해주는 시스템 