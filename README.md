# teraterm_wizfi630s_kodico_macro
This is macro file for tera term to write firmware of wizfi630s  for kodico webmodule project

* 준비물
  * 테라텀(Tera Term)
    * https://osdn.net/projects/ttssh2/releases/
    * 테스트에 사용된 버전은 v4.106
  * tftpd32
    * https://bitbucket.org/phjounin/tftpd64/downloads/
    * 테스트에 사용된 버전은 32 v2.80
  * WizFi630S 모듈(이하 630s) 1개
    * 펌웨어(이하 FW)를 밀어넣을 장치
  * WizFi630s-evb 보드(이하 evb) 1개
    * 630s 를 외부와 연결해 줄 수 있게 해주는 보드
  * micro 5pin to usb 데이터 및 전원 케이블 1개
    * 전원 공급 및 시리얼 통신용
  * 랜선 2개
    * tftp 를 이용해서 FW 를 다운로드해야 하기 때문에 반드시 필요함
  * 공유기 1개 또는 서버쪽 공인 ip
    * 630s와 tftp 가 실행되는 pc가 동일한 네트워크 대역에 물려 있어야 함.
    * 그게 안되면 tftp 가 실행되는 pc 가 공인 ip 와 함께 69번 포트가 열려 있어야 함.
  * ttl 파일
    * 이 저장소에서 제공하는 tera term 전용 매크로 파일
  * FW 이미지
    * kodico 전용으로 이 부분은 비공개임. 꼭 확인하고 싶다면 Wiznet 에 입사 바람.
* 사용 방법
  1. pc 부팅 및 랜선 연결
  2. evb 에 630s 모듈을 장착
  3. evb 에 케이블을 pc 와 연결
  4. evb 에 랜선 연결
  5. 장비 전원 켜기
  6. pc 의 ip 확인. 예) 192.168.0.2
  7. 공유기/네트워크 에서 사용되지 않는 ip 대충 확보. 예) 192.168.0.22
  8. ttl 파일, FW 이미지 를 pc 에 다운로드
  9. pc 에 테라텀, tftpd32 설치 및 실행
  10. tftpd32 에서 Browse 단추를 눌러서 FW 이미지가 위치한 폴더를 지정
  11. tftpd32 에서 Server interface 에서 630s 와 통신할 pc ip 선택
  12. tftpd32 에서 하단의 Current Action 에 69 listen 메시지가 뜨면 정상 동작 예상됨
  13. COM 포트 확인. 윈도우라면 Win + R 키를 눌러 devmgmt.msc 를 실행시키고 포트(COM & LPT) 항목이 존재해야 하고 하위에 Silicon Labs CP210x USB to UART Bridge(COM4) 형태로 장치가 보일 때 해당 포트 번호 기억
  14. 테라텀에서 Setup >> Serial Port 메뉴 선택. Port 를 아까 확인한 포트 선택. Speed 를 115200 을 선택. 직접 타이핑해도 됨. New open 버튼 클릭
  15. 테라텀에서 Control >> Macro 메뉴 선택. 위에서 받은 ttl 파일을 찾아서 열기 버튼 클릭
  16. 위에서 확인한 사용되지 않은 장비가 사용할 ip 입력. 예) 192.168.0.22
  17. pc 의 ip 입력. 예) 192.168.0.2
  18. FW 버전 입력. 예) 0.8.8
  19. FW 파일 이름 입력. 예) a.bin
  20. 장비의 전원을 끔.
  21. 빠른 조작이 필요함.
    21.1. 화면에 나온 확인(또는 OK) 버튼을 누르고 장비 전원을 켜고 테라텀 터미널 화면을 클릭하고 숫자 2를 누르는게 2초 안에 이뤄저야 실패가 적음
    22.2. 이미 Booting image at xxxxxx ... 메시지가 지나가고 정신없이 부팅되고 있다면 매크로를 껏다가 다시 실행하든지, 대략 2-30여초 기다리면 실패 확인 창이 뜰 때까지 대기. 창이 뜨면 장비를 끄고 다시 처음부터 반복
    22.3. 정상적으로 실행되었다면 모든 키보드 입력을 자동으로 입력해줌. 완료 확인 창 뜰 때까지 보통 4분 30여초에서 5분 초반까지 걸림
    22.4. 성공/실패 창이 뜨면 evb 전원을 끄고 모듈을 교체. 이 상태에서 위 동작 반복
* 스크립트 간단 설명
  * setsync 1 동기화 기능을 사용한다고 설정. 효과는 잘 모르겠지만.. 이전 코드들이 실패해서 넣어봄
  * inputbox 텍스트를 사용자로부터 입력 받음. inputbox "창 메시지" "창 제목"
  * inputstr 테라텀 예약어이므로 변수 이름으로 쓰면 안됨. 이전 입력 관련된 모든 함수의 실행 결과를 담고 있음
  * messagebox 확인창. messagebox "창 메시지" "창 제목"
  * wait_* 기다릴 문자열 wait 명령의 인수로 사용되며 해당 문자열이 나올때까지 진행 멈춤. 예약어 timeout 값에 따라 timeout 이 발생하는데 기본값은 모르겠음..
  * cmd1/2/3 문자열 내부에 작은 따옴표가 있을 때 이를 escape 할 수 없어서 분리해놓음..
  * cmd_check_done FW가 정상적으로 실행되었다면 처음으로 실행되는 파이썬 모듈 체크 코드
  * while ... endwhile 반복문. 조건 설정하기 귀찮아서 1로 조건 줌.
  * pause 파라미터 만큼 대기. 예) pause 2 2초 대기. send 명령 등은 pause 없이 쓰면 명령이 씹히거나 연속적으로 명령 내리면 붙어서 한꺼번에 내려질 가능성이 있음..
  * send 터미널에 입력을 보냄. 예) send #50 ascii 코드 50을 보냄. 50은 Y. send $32 16진수로 ascii 코드 32를 보냄. 이 역시 Y. send 'hi' hi 문자열 보냄.
  * sendln send 와 동일하나 line feed 를 추가한다는 점이 다름. 즉, 엔터가 끝에 입력으로 추가됨.
  * wait 문자열이 이 명령 이후로 나타날 때까지 대기. timeout 값에 따라 timeout 되고 다음으로 넘어감.
  * beep 삐소리를 내는데... 코드 맨 초반에 쓰면 소리가 잘 나지만 중간에 쓰면 잘 나지 않음. 파라미터는 0-5까지의 숫자값. 4만 다른거 같고 나머지는 동일함.
  * uptime 시스템이 켜져있는 시간. 날짜나 시간 값을 정확히 비교할 게 아니라 시간 연산/비교를 하려면 time.time() 느낌으로 사용하면 됨. ms 단위.
  * if 조건 then 코드 elseif endif 설명은 생략
  * ; 주석. 즉, 실행 안함.
  * end 매크로 실행을 끝냄
* 기타
  * send, wait, pause 등을 이해하지 않으면 많은 시간을 허비하기 쉬움.. 동작 안하는 것처럼 보여서 포기하게 됨. 알고 나면 쉬운데..
