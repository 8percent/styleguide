# 주석 스타일: Googld Style을 따른다
- 참고: [구글 스타일](http://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)

## PyCharm 설정하기
- Preferences -> Tools -> Python Integrated Tools -> DocStrings -> DocString format: Google

## 작성 규칙
- 문장: 마침표를 붙인다
- 문장이 아닌 경우: 마침표를 붙이지 않는다


# Imports 순서
- 1줄에 1개의 모듈만 쓴다
- 각 그룹 사이에는 빈 줄을 추가해야 합니다
- 그룹: Python 표준 라이브러리 > 써드파티 라이브러리 > 어플리케이션 라이브러리
참고: [Imports](https://www.python.org/dev/peps/pep-0008/#imports)
참고: Isort PyCharm 설정/사용법을 참고합니다
- isort python package 설치
- .isort.cfg 파일 생성
- isort 도구 설정
- isort 단축키 등록

# 클래스
- **최상위 클래스의 객체 상속 여부**
- Python3 부터는 기본적으로 new style을 따르기 때문에 객체를 상속하지 않아도 된다.
- class EightClass: 처럼 사용한다.

# 메서드, 인스턴스 변수 명명법
- 소문자를 쓴다
- 가독성을 위해 1개의 언더스코어로 단어를 구분한다
- 서브클래스와의 이름 충돌을 피하기 위해 2개의 언더스코어로 시작하는 이름을 사용한다
- 서브클래스가 없다면 2개의 언더스코어를 쓸 필요가 없다
참고: [메서드 이름과 인스턴스 변수](https://www.python.org/dev/peps/pep-0008/#method-names-and-instance-variables)

# 메서드 vs 프로퍼티
- 내부에서 변경(write) 작업이 없으면서, parameter가 없는 조회(read) 함수에 대해서는 다음 기준을 따른다.
1. method
- 내부에서 aggregation이 있을 때, get_* 처럼 작성해서 계산됨을 명시한다
2. property
- method에 해당하지 않는 경우
3. cached_property
- property 중, cache되어도 문제가 없는 경우
- 이름 뒤에 \_cached를 추가한다




