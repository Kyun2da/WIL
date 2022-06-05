# 참고

https://blog.logrocket.com/javascript-package-managers-compared/

전통적으로, npm과 yarn은 dependency를 flat한 node_modules 폴더를 활용했다. 그러나 이러한 해결전략은 비판으로 부터 자유롭지 못했다.

따라서 pnpm은 종속성을 중첩된 node_modules 폴더에 더 효율적으로 저장하기 위해 몇 가지 새로운 개념을 도입했다.

yarn berry는 pnp모드로 node_module을 완전히 제거함으로써 더욱 발전했다.

이번 글에서는 다음과 같은 것을 다룰 것이며 적용가능한 경우, 비교를 해볼 것이다.

- 자바스크립트 패키지 매니저의 간략한 역사
- 워크플로우 설치
- 프로젝트 구조
- lock file과 의존성 저장소
- cli command
- configuration files
- monorepo support
- performance and disk-space efficiency
- security features
- adoption by popular projects

## 자바스크립트 패키지 매니저의 간략한 역사

출시된 최초의 패키지는 2010년 1월에 출시된 npm이었다. 근데 왜 10년동안 npm이 존재했지만 대안이 있는 것일까? 이러한 현상이 나타난 주요 이유는 다음과 같다.

- 다른 노드 모듈 폴더 구조를 가진 다른 의존성 해결 알고리즘 (nested vs flat)
- 보안에 영향을 미치는 호이스트에 대한 다양한 지원
- 각각 성능에 영향을 미치는 서로 다른 잠금 파일 형식
- 디스크 공간 효율성에 영향을 미치는 디스크에 패키지를 저장하는 다양한 접근 방식
- 대형 모노레포의 유지보수성과 속도에 영향을 미치는 다중 패키지 프로젝트(일명 작업공간)에 대한 다양한 지원
- 새로운 도구와 명령에 대한 서로 다른 요구 사항(각각 DX 관련 사항)
- 다양한 수준의 구성 가능성과 유연성

npm이 두각을 나타낸 후 이러한 니즈가 어떻게 파악되었는지, Yarn Classic이 그 중 일부를 어떻게 해결했는지, Pnpm이 이러한 개념을 어떻게 확장했는지, Yarn Classic의 후계자인 Yarn Berry가 이러한 전통적인 개념과 프로세스에 의해 설정된 틀을 깨기 위해 어떻게 노력했는지에 대한 간략한 역사로 들어가 보자.

#### npm, 선구자

package.json, devDependency, node_modules의 개념은 모두 npm을 통해 소개되었고, 2020년에 npm은 microsoft에 인수되었다.

#### 많은 혁신을 불러왔던 yarn v1

2016년 당시, 몇몇 사람들은 당시 npm이 가지고 있던 일관성, 보안 및 성능 문제를 해결할 새로운 패키지 관리자를 개발하기 위한 Yet Another Resource Negotitator의 약자인 yarn을 발표했다.

비록 그들은 npm이 확립한 많은 개념과 프로세스에 Yarn의 아키텍처 설계를 기반으로 했지만, yarn은 초기 릴리즈에서 패키지 매니저 환경에 큰 영향을 미쳤다. npm과 대조적으로 yarn은 초기 버전의 npm의 주요 문제점이었던 설치 프로세스의 속도를 높이기 위해 작업을 병렬화했다.

yarn은 dx, 보안 및 성능에 대한 기준을 높였으며, 다음을 포함한 많은 개념을 발명했다.

- native monorepo support
- cache-aware installs
- offline caching
- lock files

Yarn v1은 2020년에 유지보수 모드로 전환되었다. 그 이후로 v1.x 라인은 레거시(legacy)로 간주되어 Yarn Classic으로 이름이 바뀌었다. 그것의 후속작인 Yarn v2 또는 Berry는 현재 활발하게 개발되고 있다.

#### pnpm, 빠르고 디스크 효율적

PNPM 버전 1은 2017년 졸탄 코찬에 의해 출시되었다. npm의 대체품이므로, npm 프로젝트가 있다면 바로 pnpm을 사용할 수 있다

pnpm의 제작자가 npm 및 Yarn에서 겪었던 주요 문제는 프로젝트 전반에 걸쳐 사용된 종속성의 중복 저장이었다. Yarn Classic은 npm에 비해 속도 이점이 있었지만 동일한 종속성 해결 방법을 사용했다. 이는 pnpm 제작자에게는 아무런 도움이 되지 않았고, npm과 Yarn Classic은 노드 모듈을 평평하게하기 위해 호이스트를 사용했다.

pnpm은 호이스트 대신 다른 종속성 해결 전략인 컨텐츠 주소 가능 스토리지를 도입했다. 이 방법을 사용하면 홈 폴더의 글로벌 저장소(~.pnpm-store/)에 패키지를 저장하는 중첩된 node_modules 폴더가 생성된다. 모든 버전의 종속성은 해당 폴더에 물리적으로 한 번만 저장되므로 단일 진실 소스를 구성하고 상당한 디스크 공간을 절약할 수 있다.

이것은 node_modules 레이아웃을 통해 이루어지며, symlinks를 사용하여 종속성의 중첩된 구조를 생성한다. 여기서 폴더 안의 모든 패키지의 모든 파일은 저장소에 대한 하드 링크이다. 공식 문서의 다음 도표는 이를 명확히 한다.

#### yarn (v2, Berry), pnp와 함께 혁신

yarn 2는 2020년 1월에 출시되었으며 오리지널 얀에서 주요 업그레이드로 발표되었다. Yarn 팀은 그것이 본질적으로 새로운 코드 기반과 새로운 원칙을 가진 새로운 패키지 관리자라는 것을 더 분명히 하기 위해 Yarn Berry라고 부르기 시작했다.

Yarn Berry의 주요 혁신은 노드 모듈을 수정하기위한 전략으로 나온 PnP (Plug'n Play) 접근 방식이다. node_modules를 생성하는 대신 의존성 검색 테이블이 있는 .pnp.cjs 파일이 생성된다.이 파일은 중첩된 폴더 구조 대신 단일 파일이므로보다 효율적으로 처리할 수 있다. 또한 모든 패키지는 .yarn / cache / 폴더 내부에 zip 파일로 저장되므로 노드 모듈 폴더보다 디스크 공간이 적다.

이 모든 변화는 breaking change를 요구해서 처음에는 이러한 변경이 간단하지 않았다.

이러한 pnp의 비호환성을 해결하기 위해 팀은 기본 작동모드를 쉽게 변경할 수 있는 몇가지 방법을 제안했다.

Yarn Berry는 나온지 얼마 안됬지만 패키지 관리자 환경에 이미 영향을 미치고 있다. pnpm은 2020 년 말에 PnP 접근 방식을 채택했다.

## 프로젝트 구조

모든 패키지관리자는 프로젝트 메타정보를 package.json에 저장한다.
