# Git Flow

## Vincent Driessen의 브랜칭 모델

소스코드를 관리하는데 브랜치 기능을 적극적으로 사용할 수 있는게 git의 장점이라고 업급했다. 브랜치 기능도 효율적으로 사용하지 못하면 혼란이 더 가중될 가능성이 있다. Vincent Driessen은 효과적으로 git 브랜치 전략을 사용하는 것에 대한 제안을 했다.

Vincent Driessen의 브랜칭 모델에는 5개의 브랜치가 사용된다.

- master
- develop
- feature
- release
- hotfix

### master

정식 배포되는 안정적인 버전의 소스코드가 이곳에 관리된다. master 브랜치의 HEAD는 소프트웨어의 최신 배포판의 소스코드 버전이 들어있다. master 브랜치에는 지난 배포판 버전의 소스코드를 따라가기 위해 태그(tag)들이 추가되어 있다. 이 태그를 이용해 각 릴리즈 버전의 소스코드를 빠르게 확인할 수 있다.

master 브랜치에는 배포해도 될 만큼 안정성이 충분히 검증된 코드들만이 병합되어야 한다.

### develop

develop 브랜치에는 소스코드들이 끊임 없이 추가된다. 버그들을 수정하기 위한 코드와 새로운 기능을 추가하기 위한 코드, 성능을 개선하기 위한 코드들이 검증이 완료되고 PR 요청을 거치게 되면 이 곳으로 병합된다.

개발자는 feature 브랜치에서 소스코드 수정을 한 다음 develop 브랜치로 PR 요청을 하게 된다. 또 다른 리뷰어가 기능들을 검토한다음 최종적으로 develop 브랜치에 병합된다. 새로운 기능을 위한 feature 브랜치는 develop 브랜치의 HEAD에서 생성된다.

### feature

새로운 기능 개발이나 버그 수정을 위한 일련의 코드 수정이 이뤄지는 브랜치다. 이 브랜치에서 개발자 혼자 작업을 할 수도 있고, 특정 기능 개발을 위한 여러명의 개발자들이 공동으로 작업할 수도 있다.

feature 브랜치에서 작업된 내용은 최종적으로 PR을 거쳐 develop 브랜치에 병합된다.

### release

git으로 관리되는 소프트웨어는 정기적으로 성능 개선, 기능추가, 버그 수정을 반영하면서 릴리즈 된다. release 브랜치는 릴리즈를 하기 위한 목적으로 생성되는 브랜치다.

release 브랜치는 develop 브랜치를 기반으로 생성된다. 다음 릴리즈에 포함되어야 하는 기능셋과 버그픽스들이 확정된 다음 생성된다. 릴리즈 브랜치가 생성된 다음 QA 테스트가 릴리즈 브랜치를 기준으로 진행된다. 이 과정에서 발견된 버그 수정 사항들이 릴리즈 브랜치와 develop 브랜치에 적용되며, 그 밖에 메이저 기능들이 중간에 추가되지는 않는다.

테스트 이후 릴리즈 브랜치의 코드가 안정적이라고 판단되면, master 브랜치에 병합되고 릴리즈에 해당하는 태그가 생성된다. 릴리즈 브랜치가 생성된 이후 반영된 수정사항들은 develop 브랜치에도 반영된다.

### hotfix

여기까지 본 작업은 PR, QA 테스트 등의 작업들이 필요하다. 안정성을 높이기 위한 작업이지만 긴급한 대응에는 장애물이 될 수 있다. hotfix는 정기적인 릴리즈 이외에 긴급하게 수행되어야 할 버그 수정을 반영하기 위해 생성되는 브랜치다. 다음 릴리즈 프로세스를 기다릴 수 없을 정도로 긴급한 패치가 바로 반영되어야 하는 경우 이 브랜치를 사용한다.

hotfix 브랜치는 master 브랜치를 기반으로 생성된다. 생성된 hotfix 브랜치에 긴급한 패치들이 반영된다. 이후 hotfix 브랜치는 master 브랜치에 병합되고 태그 생성이 된다. 마찬가지로 수정 사항은 develop 브랜치로도 병합되어 긴급 수정 사항이 이후 릴리즈에도 반영되도록 한다.

### git-flow

git-flow는 Vincent Driessen의 "A successful git branching model'이라는 글에서 제안한 브랜치 모델을 쉽게 사용할 수 있도록 몇개의 명령으로 구현해 놓은 git의 확장이다. 간단한 명령을 이용해서 Vincent Driessen의 브랜치 모델에서 필요한 브랜칭 동작들을 수행할 수 있다.
