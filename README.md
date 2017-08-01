# EjDbg
Windows and Linux Debugger FrontEnd for cdb and gdb




## 목적
  디버거의 경우 플랫폼 별로 따로 존재할 수 밖에 없으며 운영체제에서 기본적으로 제공되는 디버거의 경우에는 사용하기 불편한 경우가 많아 다른 여러 종류의 디버거가 존재하며 계속 생기고 있다. 개인적으로 이런 환경은 비효율적이라고 생각하며 가장 효율적인 방법은 원하는 것을 스스로 만드는 것이라고 생각했다. 하지만 현실적으로 봤을 때 혼자서 Windbg나 Gdb 같은 운영체제에서 제공되는 디버거의 기능을 따라잡기도 힘들며, 동시에 여러 오픈 소스 디버거들 처럼 사용자 친화적인 인터페이스를 만들기도 힘들다고 생각하였다. 결론적으로 최선의 방법을 선택해서 적어도 내가 사용할만한 디버거 환경을 만드려는 목적으로 개발하게 되었다.




## 기능
  여기서 제공하는 디버거 프론트엔드는 32/64비트 Cdb(Windbg의 유저 모드 CLI 버전)를 이용해 간단한 GUI 형태로 인터페이스를 제공한다. 물론 명령어를 사용하는 방식이 유지되기 때문에 아직까지는 불편한 점이 많이 남아있지만 cdb 및 gdb를 그대로 사용할 수 있다는 점이 장점이다. 프로그램은 윈도우의 경우 python3, 리눅스의 경우 python2로 개발하였다. CLI 프로그램은 pexpect(윈도우의 경우 winpexpect)를 통해 입출력을 처리하였고 GUI 환경은 Tkinter를 사용해 최대한 복잡함 및 번거로움을 줄이고자 하였다. 결론적으로 Cdb와 Gdb를 그대로 사용하면서 GUI 형태로 여러 출력들을 보기 쉽게 하였고 동시에 다른 사용자 친화적인 디버거들의 기능을 최대한 추가하려고 하였다. 가장 중요한 것은 기본적으로 명령어를 받아서 실행되는 메커니즘이기 때문에 블로그에 사용자 모드 명령어들을 정리하였다. [ 링크 : http://sanseolab.tistory.com/22 ]




## 스크린샷
![start](http://cfile21.uf.tistory.com/image/27884C3359805F223527B8)




## 설치
  가장 먼저 Windbg 또는 Gdb가 설치되어 있어야 한다. 그리고 윈도우의 경우 파이썬3, 리눅스는 파이썬2가 설치되어 있어야 한다. 그리고 추가적으로 pywin32 및 winpexpect(리눅스의 경우 pexpect)를 설치한다. 마지막으로 gdb 또는 cdb의 경로에 대한 환경 변수를 설정할 필요가 있다. 리눅스의 경우 gdb가 분석 대상인 실행 파일의 버전에 맞게 자동으로 실행되지만, 윈도우는 분석 대상의 버전 즉 x86인지 x64인지에 따라 환경변수 경로를 선택해서 설정할 필요가 있다. 또한 윈도우의 경우에는 파이썬의 경로도 환경 변수에 등록하는 것이 편하다. 참고로 리눅스의 경우에는 일반적으로 python2가 설치되어 있지만 Tkinter는 없는 경우가 있다. 이 경우에는 "yum install tkinter" 명령어로 Tkinter를 설치한다.




## 사용
리눅스의 경우<br>
$ python gdb_x64.py hello<br>
윈도우의 경우<br>
\> python cdb_x86.py hello.exe<br>
또는<br>
\> cdb_x86.py hello.exe<br>




## 상태
1. 명령어<br>
  제어 명령어의 경우 모든 제어 명령어들에 대해 왼쪽 레지스터, 디스어셈블리, 스택 패널을 업데이트 시키게 설정하였다. 기타 명령어들의 경우에는 오른쪽 명령어 패널에 결과만 출력한다.


2. 패널

2.1 레지스터 패널<br>
  윈도우와 리눅스 모두 아직까지는 변경된 레지스터를 색깔로 표시해주는 기능이 존재한다. Ollydbg처럼 문자열이나 심볼 등을 보여주는 기능은 없다. 이후 윈도우 버전은 da 명령어를 통해 문자열을 보여주는 기능은 추가하였다.

2.2 디스어셈블리 패널<br>
  모두 현재 명령어 줄을 빨간 색으로 보여주며 전후 명령어들을 보여준다. 윈도우는 ub 명령어를 사용하지만 리눅스의 경우에는 처음 명령어 부분이 깨질 수 있다. 또한 Windbg에서는 해당 명령을 실행하기 전에만 보여주던 API 함수 이름도 미리 보여준다. 원리는 IAT를 이용한 것이며 또한 간접 호출 (Indirect Call)도 미리 보여주도록 구현하였다. 이것은 모듈이 바뀌었을 때도 마찬가지이다. 

2.3 스택 패널<br>
  윈도우의 경우 dds와 dda 명령어로 심볼 및 문자열을 보여준다. 아직까지는 Ollydbg처럼 깔끔한 결과를 보여주지 않으며 API 호출 시 파라미터를 보여주는 기능도 없다. 리눅스는 오직 값만 보여주는 기능만 .

2.4 명령어 패널<br>
  일반적인 명령어의 출력 결과를 보여준다.


3. 자체 명령어

3.1 ,v<br>
  왼쪽 패널 업데이트. ip 레지스터를 수정하거나 변경 사항이 발생했을 때 왼쪽 패널 전체에 적용시킨다. 다음과 같이 사용한다.<br>
0:000> r @eip=0x004011a8<br>
0:000> ,v

3.2 ,c
  명령어 윈도우 삭제. 일종의 화면 clear 명령어이다.

3.3 ,vs \<address\> 또는 [u, d]<br>
  ,vs \<address\>는 현재 디스어셈블리를 해당 주소를 기반으로업데이트 해준다. u 옵션을 사용하면 자동으로 윗 페이지, d 옵션은 아랫 페이지를 보여준다. 이 자체 명령어를 제공하는 이유는 간단히 Enter만으로 화면을 이동시킬 수 있다는 점 뿐만 아니라 윈도우의 경우에 제공되는 API 함수 이름을 보여주는 기능도 포함되기 때문이다.


4. 기타 기능<br>
  엔터를 입력하면 과거 입력했던 명령어가 다시 한 번 입력된다.


5. 추가 사항
- 모든 제어 명령어 약자 처리.
- 주석 기능.
- 옵션 창 생성.
- 화살표를 통한 과거 명령어 검색.
- 윈도우 버전 글자 크기 줄이기
- 윈도우 버전 a 명령어
- 마우스 오른쪽 키를 이용한 주소 입력 간결화.
- Gdb의 경우 PEDA 등을 참고하여 부족한 기능을 추가한다.
