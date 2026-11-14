# quiz-game-test
python과 Github 저장소에서 quiz프로그램 test

```
1. git commit 10회 이상 하기(커밋 메시지에 변경 요약 포함)
1) 메뉴기능 완성 후 git commit 실행하기
2) Quiz 클래스 작성후 커밋한다.
3) 기본 퀴즈 데이터 작성 후 커밋한다.
4) main 브랜치 이외의 추가 브랜치를 생성하고 퀴즈풀기 코딩작업한다.
   git checkout -b 새브랜치명 : 새 브랜치 생성하면서 새브랜치로 이동
5) 퀴즈풀기(브랜치 활용) 기능 완성후 커밋하고 main 브랜치로 병합
   git checkout main : 해당 브랜치에서 빠져나와 main으로 이동
   git merge 새브랜치명 : main에 새브랜치의 내용을 합치는 실행
6) 퀴즈 추가 기능 완성후 commit한다. (예 Feat : 퀴즈 출제 기능 구현)
7) 퀴즈 목록 기능 완성 후 커밋한다.
8) 점수확인 기능 완성 후 커밋한다.(예 Fix : 점수 계산 오류 수정)
9) QuizGame 클래스 작성후 git commit (예 Refacto : QuizGame 책임분리)
10) 파일 입출력 기능 완성 후 commit (퀴즈게임 프로젝트 코드가 GitHub에 업로드되어 있다.)
11) README.md에 프로젝트 개요,퀴즈 주제 선정이유,실행방법,기능 목록,파일 구조,데이터 파일 설명(state.json 경로/역할/스키마 등)의 항목을 포함하여 작성후 Git 최종 push한다.(예 Docs : README 실행방법 추가)
12) Git 저장소 복제 실습
clone과 pull을 각각 1회 이상 사용한 기록이 있다.
git clone URL(GitHub 주소) 미션수행 저장소를 clone하여 별도의 로컬 디렉터리에 복제한다.
복제된 저장소에서 간단한 변경(예:README.md에 한 줄 추가)을 하고 commit하고 push한다.
기존 로컬 작업 디렉터리에서 pull로 변경사항을 가져온다.
git pull 받은 내용이 정상적으로 반영되었는지 확인한다.


[ python 콘솔 프로그램 1개 ] : 모든 코드는 한 함수에 작성하지 않고, 기능별로 함수를 분리해야 한다. 2개이상의 클래스로 역할 분리
===
화면출력
메뉴
1. 퀴즈 출제
2. 퀴즈 등록
3. 퀴즈 목록
4. 점수 확인
5. 종료
======메뉴기능 완성후 git commit
input
입력번호
* 숫자 입력이 필요한 곳 처리기준(잘못된 입력에 대한 처리가 있어야 함.)
1) 입력 앞뒤 공백 제거후 숫자변환
2) 숫자변환 실패시 안내메시지 출력후 재입력으로 복귀
3) 허용범위 밖 숫자 입력시 안내메시지 출력후 재입력으로 복귀
4) 빈 입력(그냥 엔터)인 경우 안내메시지 출력 후 재입력으로 복귀


===
if 입력번호에 따라 동작 (사용자가 기능 선택 가능해야 함)

퀴즈풀기
퀴즈추가
퀴즈목록
점수확인  

===
퀴즈주제 선택 5개이상 포함
import json
import sys

class Quiz:
    def __init__(self, question, options, answer):
        self.question = question
        self.options = options
        self.answer = answer

    def to_dict(self):
        """객체를 저장 가능한 딕셔너리로 변환"""
        return {
            "question": self.question,
            "options": self.options,
            "answer": self.answer
        }

# 5가지 주제의 퀴즈 데이터
quiz_data = [
    {"question": "태양계에서 가장 큰 행성은 무엇인가요?", "options": ["지구", "화성", "목성", "토성"], "answer": "목성"},
    {"question": "조선 왕조를 세운 태조의 이름은 무엇인가요?", "options": ["이방원", "이성계", "이도", "이산"], "answer": "이성계"},
    {"question": "파이썬에서 리스트의 길이를 구하는 함수는 무엇인가요?", "options": ["add()", "insert()", "len()", "count()"], "answer": "len()"},
    {"question": "세계에서 가장 넓은 면적을 가진 나라는 어디인가요?", "options": ["캐나다", "미국", "중국", "러시아"], "answer": "러시아"},
    {"question": "레오나르도 다 빈치의 대표적인 작품은 무엇인가요?", "options": ["별이 빛나는 밤", "모나리자", "절규", "진주 귀걸이를 한 소녀"], "answer": "모나리자"}
]

# Quiz 인스턴스 생성
quiz_instances = [Quiz(d["question"], d["options"], d["answer"]) for d in quiz_data]
===
데이터 파일이 없거나 손상된 경우에도 프로그램이 실행 가능해야 한다.
파일이 없으면 기본 퀴즈 데이터를 사용한다.
파일이 손상되었으면 안내 메시지를 출력하고, 기본 퀴즈 데이터로 복구(또는 초기화)한다.
===
* 프로그램 종료기능 존재해야 함
* 프로그램 종료후 재실행시 추가한 퀴즈와 최고점수가 유지(파일로 저장)
* 프로그램 실행 중 Ctrl+C 또는 입력스트림 종료(EOFError)가 발생해도 비정상 종료하지 않도록 처리
1) 안내메시지 출력, 가능한 범위에서 저장 후 안전하게 종료한다.
def save_progress(quizzes, filename="quiz_results.json"):
    """현재까지의 퀴즈 객체를 파일로 저장"""
    if not quizzes:
        print("[시스템] 저장할 퀴즈 데이터가 없습니다.")
        return

    try:
        # 객체를 저장 가능한 딕셔너리 형태로 변환
        serializable_data = [q.to_dict() for q in quizzes]
        with open(filename, "w", encoding="utf-8") as f:
            json.dump(serializable_data, f, ensure_ascii=False, indent=4)
        print(f"[시스템] {filename}에 저장을 완료했습니다.")
    except Exception as e:
        print(f"[오류] 저장 중 문제가 발생했습니다: {e}")

def run_quiz(quizzes):
    print("--- 퀴즈를 시작합니다! (종료하려면 Ctrl+C 또는 Ctrl+D) ---\n")
    
    try:
        for i, quiz in enumerate(quizzes, 1):
            print(f"문제 {i}: {quiz.question}")
            for idx, option in enumerate(quiz.options, 1):
                print(f"  {idx}) {option}")
            
            user_input = input("정답을 입력하세요: ").strip()
            
            if user_input == quiz.answer:
                print("정답입니다!\n")
            else:
                print(f"틀렸습니다. 정답은 '{quiz.answer}'입니다.\n")

        print("모든 문제를 풀었습니다. 프로그램을 종료합니다.")

    except KeyboardInterrupt:
        print("\n[알림] 사용자가 Ctrl+C로 중단을 요청했습니다.")
    except EOFError:
        print("\n[알림] 입력 스트림이 종료되었습니다.")
    except Exception as e:
        print(f"\n[오류] 예상하지 못한 오류가 발생했습니다: {e}")
    finally:
        # 예외 발생 여부와 관계없이 저장 시도
        save_progress(quizzes)
        print("[시스템] 안전하게 종료합니다.")
        sys.exit(0)

if __name__ == "__main__":
    run_quiz(quiz_instances)
===
git main 브랜치 이외에 새로 생생된 브랜치 활용한 퀴즈풀기 : 게임진행
1) 저장된 퀴즈를 출제한다.
2) 사용자가 정답을 입력할 수 있다.
3) 정답/오답 여부를 알려준다.
4) 모든 문제를 풀면 결과를 표시한다.
5) 퀴즈가 없는 경우를 처리한다????
=======퀴즈풀기 기능 완성후 커밋하고 main브랜치로 병합한다.
===
퀴즈 추가 : 입력처리
1) 새로운 퀴즈를 등록할 수 있다.
2) 필요한 정보(문제, 선택지 4개, 정답번호)를 입력받는다.
3) 잘못된 입력에 대한 처리가 있어야 한다. (공통입력/예외 처리 기준 포함) 예 잘못된 입력입니다.
4) 추가한 퀴즈를 파일에 저장한다.
======= 퀴즈 추가 기능 완성후 commit한다.

===
퀴즈 목록
1) 저장된 퀴즈 목록을 확인할 수 있다. 예 저장된 데이터를 불러왔습니다.(퀴즈6개, 최고점수 80점)
2) 퀴즈가 없는 경우를 처리한다.
======= 퀴즈 목록 기능 완성 후 커밋한다.

===
점수확인
1) 최고 점수를 확인할 수 있다. 예 최고점수 : 80점(5문제 중 4문제 정답)
2) 퀴즈를 풀 때마다 최고 점수와 비교하고 갱신한다.
3) 최고 점수를 파일에 저장한다.
4) 아직 퀴즈를 풀지 않은 경우를 처리한다.
========= 점수확인 기능 완성 후 커밋한다.

===

1. 클래스 정의
1) 개별 퀴즈를 표현하는 Quiz 클래스 정의
class Quiz:
문제 question
선택지 choices  : 4개를 기본
정답 answer : 1~4 중 하나의 번호로 관리하는 방식 권장
2) 메서드 : 퀴즈 출력, 정답확인 등 필요한 기능 구현
메서드 : 퀴즈출력(print), 정답확인(if) 등 필요한 기능 구현
======= Quiz 클래스 작성후 git commit
3) 기본 퀴즈 데이터
본인이 선택한 주제의 퀴즈 5개 이상을 직접 작성한다.
각 퀴즈는 문제, 선택지(4개), 정답을 포함한다.
Quiz 클래스의 인스턴스로 퀴즈를 생성한다.
*인스턴스 : 클래스를 바탕으로 실제로 만들어진 개별 객체
======= 기본 퀴즈 데이터 작성 후 git commit

2. QuizGame 클래스
1) 게임 전체를 관리하는 QuizGame 클래스를 정의한다.
2) 속성 : 퀴즈 목록, 최고 점수 등
3) 매서드 : 메뉴 표시, 퀴즈 풀기, 퀴즈 추가, 목록 보기, 점수 확인, 파일 저장/불러오기 등
========QuizGame 클래스 작성후 git commit

3. 파일 저장/불러오기(state.json) : 저장로직
1) 퀴즈 데이터와 최고 점수를 프로젝트 루트의 state.json에 저장하는 기능 구현하기(UTF-8 인코딩 권장)
2) JSON파일에서 데이터를 불러오는 기능을 구현한다.
3) 파일이 없는 경우(첫 실행) 기본 퀴즈 데이터를 사용한다.
4) 파일이 손상되었거나 읽기/쓰기 오류가 발생한 경우를 try/exxept로 처리한다.
5) 최소 스키마 예시(데이터 형태 참고; 키 이름/구조는 일관되게 유지)
6) quizzes: 퀴즈 목록, best_score : 최고점수(또는 최고 정답 수 등)
======== 파일 입출력 기능 완성 후 commit


*파일을 열고, 읽고 쓰는 기본 과정을 설명할 수 있다.
데이터는 state.json에 저장하고 불러온다(JSON형식 UTF-8 인코딩 데이터 저장에 사용하는지 설명)
파일 열기 try/except를 사용하여 오류를 처리할 수 있다.
try:
        with open(DATA_FILE, 'r', encoding='utf-8') as f:
            data = json.load(f)
            # expected key check
            questions = data.get("questions", default_questions)
            high_score = data.get("high_score", default_score)
            return questions, high_score

    except FileNotFoundError:
        print("알림: 저장 파일이 없어 기본 데이터를 사용합니다.")
        return default_questions, default_score

    except json.JSONDecodeError:
        print("경고: 파일 형식이 손상되어 기본 데이터를 사용합니다.")
        return default_questions, default_score

    except Exception as e:
        print(f"로드 실패: {e}")
        return default_questions, default_score
2. GitHub 저장소 1개
1) GitHub에 새 저장소를 생성
2) 로켈에 저장소 설정
3) .gitignore, README.md 파일을 생성한다.
4) Git : 초기설정 완료후 첫번째 commit과 push를 수행한다.

README.md에 
프로젝트 개요
퀴즈 주제 선정이유
실행방법 (예 python main.py)
기능 목록(퀴즈 풀기/ 추가/ 목록/점수)
파일 구조
데이터 파일 설명(state.json 경로/역할/필드구조/스키마 등)
의 항목을 포함하여 작성후 Git 최종 push한다.

<제출물>

1. GitHub 저장소 URL 

2. 개발 환경 설정 스크린샷(VSCode, Python버전, Git 설정)

3. 프로그램 실행 결과 스크린샷(퀴즈 추가, 목록, 플레이, 점수)
1)docs/screenshots/add_quiz.png
2)docs/screenshots/menu.png
3)docs/screenshots/play.png
4)docs/screenshots/score.png

4. git log --oneline --graph 결과를 스크린샷

확인 항목	터미널 명령어	대체 또는 보조 명령어
VSCode	code --version	설치 경로 확인은 별도 탐색
Python	python --version	python3 --version
Git	git --version	해당 없음
Windows 운영체제	systeminfo | findstr /B /C:"OS Name" /C:"OS Version"	PowerShell 이용
macOS 운영체제	sw_vers	uname -a
Linux 운영체제	uname -a	해당 없음
여러 명령어를 한 장에 담으려면 하나씩 실행하고 결과를 남긴 뒤 다음 명령어를 입력하는 방식이 보기 좋아요. 운영체제 정보까지 요구받았다면 사용하는 운영체제의 명령어를 추가하면 됩니다.
운영체제별 스크린샷 단축키
운영체제	전체 화면	선택 영역	활성 창
Windows	Win + PrtSc	Win + Shift + S	Alt + PrtSc
 --oneline 옵션은 커밋 정보를 한 줄로 표시해줍니다. 기존의 git log를 통한 커밋 정보 확인은 커밋 해시, 브랜치, 작성자(Author), 작성일(Date), 그리고 커밋 메세지로 표기되었습니다. 그래서 커밋기록이 많은 경우 긴 스크롤을 만들게 되며 커밋 메세지만 확인하고 싶을 경우 번거로움을 주게 됩니다. 그래서 git log명령과 함께 --oneline 옵션을 이용할 경우에 짧은 커밋 해시, 브랜치, 커밋 메세지만을 한 줄로 나타내어 좀 더 간결하게 확인이 가능합니다. --branches --branches 옵션은 현재 존재하는 각 브랜치별로 최신 커밋을 확인할 수 있는 옵션입니다. 위의 상황에서 master에서 한 번 커밋작업을, automn브랜치에서 또 한 번 커밋작업을 해둔 상태입니다. 그러면 --branches 옵션으로 내용을 확인해보겠습니다. 앞에 브랜치를 나타내는 괄호가 보이는데, 이는 해당 브랜치의 최신 커밋을 의미합니다. summer와 spring 커밋은 "commit a1"이라는 메세지를 가진 커밋이 최신인 브랜치이고, master브랜치는 "add msg a1 by bam"이라는 커밋 메세지를 가진 커밋을 최신으로, automn은 "add msg a1 by otherStudent"라는 메세지를 가진 커밋이 최신임을 가리키고 있는 것 입니다. --graph --graph 옵션은 현재 브랜치들의 상태를 막대로 나타냅니다. 반드시 --branches 옵션과 함께 사용하여야 작동이 되며, 이 옵션을 통해 좀 더 직관적으로 브랜치가 언제 나뉘었는지, 병합되었는지를 확인할 수 있는 옵션입니다. 결과 화면을 해석해보자면 37fc3bd 커밋 이후에 master와 automn브랜치로 나뉘었다고 해석할 수 있습니다.

git log --oneline --graph 결과를 스크린샷하려면 어떻게 해야할까요?
가장 간단한 방법은 명령어를 실행한 뒤 Windows는 Win + Shift + S, macOS는 Cmd + Shift + 4로 로그 영역을 드래그해 캡처하는 것입니다. 문서용으로 깔끔하게 저장하려면 출력 내용을 복사해 carbon.now.sh에 붙여넣고 언어를 Plain Text로 설정한 뒤 이미지로 내보내세요.
터미널 직접 캡처 방법
로그가 길다면 창 크기를 충분히 키운 뒤 원하는 구간만 선택해 캡처하는 편이 좋아요. 캡처할 때 터미널 테마나 글꼴 색상이 바뀌면 그래프의 선과 별표가 잘 보이지 않을 수 있으므로, 밝은 배경에는 어두운 글자, 어두운 배경에는 밝은 글자가 드러나도록 설정하면 가독성이 좋아집니다.
고품질 이미지 제작 도구 활용
터미널에서 git log --oneline --graph를 실행합니다.
출력된 내용을 모두 복사합니다.
Carbon에 붙여넣은 뒤 설정을 Plain Text로 바꿉니다.
Export에서 PNG 또는 SVG를 선택해 저장합니다.
이 방식은 터미널 주변의 불필요한 메뉴나 프롬프트까지 제거할 수 있어 발표 자료나 기술 문서에 삽입할 때 유리해요. 특히 벡터 형식인 SVG는 확대해도 글자와 선이 깨지지 않아 문서 품질을 높이는 데 적합합니다.
텍스트 파일 저장 후 변환
먼저 git log --oneline --graph > git_log.txt 명령으로 출력 결과를 파일에 저장할 수 있습니다. 이후 git_log.txt의 내용을 복사해 이미지 변환 도구에 붙여넣으면 되며, 이 방식은 긴 로그를 편집하거나 일부 구간만 골라 캡처할 때 편리해요.
```


# 과제를 마치고...
## 1. python 기초
```
1-1. 변수(Variable)란? data가 저정되는 공간의 이름
변수 이름을 지을 때 a, b처럼 짧게 짓기보다는 user_name, total_score처럼 의미가 담긴 이름을 지어주는 습관이 중요!
1-2. 변수를 사용하는 이유는 컴퓨터에 값을 기억시키고, 코드를 더 편하고 정확하게 관리하기 위함.
① 데이터를 기억하고 재사용하기 위해서
프로그램을 만들다 보면 같은 값을 여러 번 써야 할 때가 있습니다.
변수에 값을 담아두면, 나중에 그 이름만 불러서 계속 다시 쓸 수 있어요.
변수가 없을 때: 매번 숫자를 직접 입력해야 함
변수가 있을 때: 이름을 불러서 사용
② 코드의 의미를 쉽게 파악하기 위해서 (가독성)
숫자나 문자만 덩그러니 있으면 그것이 무엇을 의미하는지 알기 어렵습니다.
변수 이름을 잘 지어주면 코드를 읽는 사람이 "아, 이게 이 데이터구나!"라고 바로 알 수 있죠.
③ 수정과 유지가 편리하기 위해서 (유지보수)
만약 프로그램 전체에서 '사과 가격'을 100번 사용했는데, 가격이 올랐다면 어떻게 될까요?
변수가 없다면 100곳을 일일이 찾아가서 고쳐야 하지만, 변수를 사용했다면 처음 선언한 부분만 수정하면 됩니다.
④ 자료형(Data Type)
name = "파이썬"      # 문자열 str(String) 
age = 25            # 정수 int(Integer)
height = 175.5      # 실수 float(Float)
is_fun = True       # 불리언 bool(Boolean)
⑤ 자료구조
리스트(List): 순서대로 나열된 장바구니입니다.
딕셔너리(Dictionary): '이름: 파이썬'처럼 키(Key)와 값(Value)이 쌍을 이루는 사전입니다.
⑥ 조건이 여러개일 경우 if / elif / else 사용
if 조건1:
    조건1이 참일 때 실행
elif 조건2:
    조건1은 거짓이고, 조건2가 참일 때 실행
elif 조건3:
    조건1과 조건2는 거짓이고, 조건3이 참일 때 실행
else:
    위 조건들(조건1,조건2,조건3)이 모두 거짓일 때 실행
들여쓰기 중요! Tab 사용
조건이 1개만 일경우 if, 조건의 참/거짓 판단시 if/else, 조건이 여러개일 경우 if / elif / else 사용
⑦ for는 정해진 대상을 하나씩 반복할 때 사용,
for를 쓰면 좋은 경우는 반복할 횟수가 정해져 있을 때/ 리스트. 문자열, range를 하나씩 처리할 때
while은 조건이 참인 동안 계속 반복할 때 사용, 조건이 False가 되면 종료, 무한반복 주의
사용자가 종료를 선택할 때까지 반복(break 사용 반복문 즉시 종료/continue 현재 반복을 건너뛰고 다음 반복으로 넘어가게 함)
⑧ 반환값은 함수가 계산한 결과를 함수 밖으로 돌려주는 값
반환할 때는 return을 사용하면 결과를 변수에 저장하거나 다른 계산에 사용할 수 있다.
(결과를 돌려주는 return과 비교해볼 명령어 print는 화면에만 결과를 출력하고 값을 돌려주지 않는다)
-반환값이 없고 단순히 연산결과만 화면에 출력된 경우
def add_numbers(a, b):
    return a + b
result = add_numbers(3, 5)
print(result)
8
-반환값이 저장되어 다음 연산에 사용한 경우
def add_return(a, b):
    return a + b

result = add_return(3, 5)
print(result)
8
result = add_return(3, 5)
total = result + 10

print(total)
18
⑨ 함수는 python에서 자주 사용하는 코드에 이름을 붙여 저장해 둔 것
def show_menu():
    print("퀴즈 게임에 오신 것을 환영합니다!")
    print("1. 퀴즈 시작")
    print("2. 점수 보기")
    print("3. 종료")
매개변수는 함수에 값을 전달하기 위한 변수로 함수를 호출할 때 매개변수에 값을 넣는다.
def say_hello(name):
    print(name + "님, 안녕하세요!")
say_hello("지민")
실행하면, 지민님, 안녕하세요!
```
## 2. 클래스와 객체
```
① 클래스(class) : 데이터와 기능을 하나로 묶어서, 프로그램을 더 체계적으로 만들기 위한 도구. 객체이름
클래스 사용이유
관련 있는 데이터와 기능을 하나로 묶기 위해서
코드를 더 깔끔하게 정리하기 위해서
같은 구조의 객체를 여러 개 쉽게 만들기 위해서
유지보수를 쉽게 하기 위해서

② 메서드(method) : 클래스 안에 정의된 함수로 객체가 가진 데이터를 사용하거나 객체에게 어떤 동작을 시킬 때 사용한다. 실행 객체의 데이터와 관련된 기능을 실행하며 클래스이름.메서드이름() 형태로 호출된다.
메서드에서 가장 자주 보이는 것이 self인데 현재 메서드를 호출한 객체 자기 자신을 의미함. 매서드이름(self)
지금 이 매서드를 실행하고 있는 객체를 의미함. 함수처럼 return을 사용할 수 있다.
클래스만 존재하는 코드블록이 있고, 클래스 안에 메서드가 존재하는 코드블록이 있다.
객체의 데이터를 쉽게 다룰 수 있다.
코드의 재사용이 쉬워진다.
관련있는 기능을 클래스안에 묶기 위해
 
구성요소 	의미
class Question:	Question 클래스 정의
__init__()	객체가 만들어질 때 자동 실행
self.text	객체가 가진 속성
self.answer	객체가 가진 속성
check_answer()	객체가 가진 메서드
self	현재 객체 자기 자신
class Question:
    def __init__(self, text, answer):
        self.text = text
        self.answer = answer

    def check_answer(self, user_answer):
        return user_answer == self.answer

```

## 3. 파일 입출력
```
데이터 영속성 (JSON) : 프로그램이 종료되어도 데이터가 남으려면 json 모듈을 활용해야 함.
    데이터는 state.json에 저장하고 불러온다
저장 시: json.dump()를 사용하여 파이썬 객체(딕셔너리/리스트)를 파일로 저장.
불러오기 시: json.load()를 사용하여 파일 내용을 파이썬 객체로 복원.
```

## 4. Git 기초명령어 7종 1번 이상 사용하기
```
git init 새 프로젝트 시작시 빈 저장소 만들기

git add 파일명 또는 . GitHub에 저장할 파일 담기
    git add quizgame.py

git commit GitHub에 저장할 코드에 대한 설명 적어주기
    git commit -m "quiz프로그램 test"

git push 준비된 세이브 포인트를 GitHub로 저

git pull 작업하는 동안 수시로 원격저장소에 업데이트된 최신 프로젝트를 내 컴퓨터로 저장

git checkout main(브랜치명) main브랜치로 이동

git clone원격저장소 전체를 내 컴퓨터에 저장하므로 내 컴퓨터에 새로운 펄더가 생김. 주로 프로젝트 처음시작시 사용
```

### 4-1 Git의 정의와 필요성
```
Git은 파일의 변경 이력을 기록하고 필요할 때 이전 상태로 되돌릴 수 있게 해 주는 버전 관리 도구. 변경 내역을 추적하고 실수를 복구할 수 있으며, 여러 사람이 같은 파일을 동시에 수정할 때 작업이 덮어씌워지는 문제를 막아 협업에 꼭 필요함.
```

# 나만의 퀴즈 게임

## 1. 프로젝트 개요

Python 기본 문법과 객체 지향 프로그래밍을 활용하여 만든 터미널 기반 퀴즈 게임입니다.

사용자는 메뉴를 통해 퀴즈를 풀고, 새로운 퀴즈를 등록하고, 퀴즈 목록을 확인하고, 최근 점수와 최고 점수를 확인할 수 있습니다.

프로그램 종료 후에도 퀴즈 데이터와 점수 데이터가 유지되도록 `state.json` 파일에 데이터를 저장합니다.

## 2. 퀴즈 주제 선정 이유

이 프로젝트에서는 다음 5가지 주제를 기본 퀴즈 주제로 사용했습니다.

1. 파이썬 기초
2. 웹 개발
3. 데이터베이스
4. 알고리즘
5. 일반상식

선정 이유는 프로그래밍 학습자가 기초 문법뿐만 아니라 웹, 데이터베이스, 알고리즘 등 개발 전반의 기본 개념을 함께 복습할 수 있도록 하기 위해서입니다. 또한 일반상식 주제를 포함하여 비전공자도 부담 없이 퀴즈를 풀 수 있도록 구성했습니다.

## 3. 실행 방법

### 1) 저장소 클론

```bash
git clone 저장소_URL
cd 저장소_폴더명
