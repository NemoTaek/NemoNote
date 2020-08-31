가장 좋은 방법

1. master repository(upstream)를 fork 한다.
2. local로 프로젝트를 clone 한다. (git clone repository url)
3. dev branch는 최신화가 되어있어야 하기 때문에 upstream의 dev를 local의 dev로 pull 한다. (git pull origin master)???
4. 최신화된 코드를 가지고 새로운 feature를 개발할 branch를 생성 후 개발한다. (git checkout -b 브랜치명)
5. 개발이 끝나면 해당 branch를 자신의 project repository의 새로 만든 feature branch에 push 한다. (git push origin master)???
6. 해당 branch를 master branch로 pull request 한다.
7. upstream 관리자는 pull request를 확인하고 merge하면 하나의 기능 개발이 마무리가 된다.
