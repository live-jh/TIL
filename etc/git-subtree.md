## github 잔디밭 유지한 채 Git Repository 합치기

1. Github에 새로운 repository 생성
2. 컴퓨터에서 관리할 디렉토리 생성 및 지정 (local 환경)
   1.  `$ mkdir ios-study-playground`
   2.  `$ cd ios-study-playground`
3. git 저장소 생성
   1. `$ git init`
   2. `$ git remote add origin 'repository web URL'`
   3. `$ git pull origin main --allow-unrelated-histories`
   4. `$ git remote -v` 
      1. 저장소 등록 확인
4. git subtree 등록
   1. `$ git subtree add --prefix=(레포지토리에 등록할 디렉토리명) (옮길 Repository web URL) (옮길 Repository의 메인 브랜치명)  `
      1. `$ git subtree add --prefix=ios-study-first https://github.com/live-jh/blahblahblah.git main`
5. 기존 Repository 제거



## References

- https://homoefficio.github.io/2015/07/18/git-subtree/
- http://yeoseon.kr/git-repository-habcigi-commit-log-yuji-subtree-iyong/