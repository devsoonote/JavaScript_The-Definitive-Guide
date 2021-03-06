# 2. 어휘구조

> 자바스크립트 완벽가이드를 읽고 작성한 개인 노트입니다. 조원들과 토론을 통해서 서로 배운 내용과 깨달은 내용에 대해서 논의한 부분 또한 함께 녹이려고 노력했습니다.
>
1. **어휘 구조란?**
   - 그 언어로 프로그램을 작성할 때 지켜야 할 기본적인 규칙의 집합(ex. 변수 이름은 어떻게 지어야 하는지, 주석은 어떻게 만드는지, 문과 문을 어떻게 구분하는 지 등...)

   - 자바스크립트 어휘 구조의 특징
      - 대소문자를 구별한다 → online, Online, ONLINE은 전부 다른 변수 이름
      - 토큰 사이의 공백 및 줄바꿈을 무시한다
      - 식별자는 글자, 밑줄(_), 달러($)로 시작해야한다
      - 예약어는 식별자로 사용 불가능하다

2. **유니코드**
   - 자바스크립트 프로그램은 유니코드 문자셋으로 작성된다 → 수정할 때 편리하도록 식별자에는 ASCII 글자와 숫자만 쓰는 것이 일반적이나 이는 관습일 뿐이며, 유니코드 글자, 숫자, 상형 문자 모두를 허용(이모지 허용X)
   - **유니코드와 ASCII 차이점**
      - ASCII: 7비트, 128개의 고유한 값만 사용, 1비트(Parity Bit): 통신 에러 검출을 위해 사용 / 영문 키보드로 입력할 수 있는 모든 가능성을 담음
      - 유니코드: 다른 언어를 표현하기에 부족하여 생성된 코드로 65536(2의 16승)개의 값을 갖는다 → 유니코드 3.0(현재는 이 갯수도 부족하여 1024*1024개 값을 갖는다)

3. **유니코드 이스케이프 시퀀스**
   - ASCII 문자만으로 유니코드 문자를 표현하는 방식
   - 유니코드 이스케이프 시퀀스 사용 이유: 자바스크립트는 구형 기술(유니코드 문자 전체를 정확히 표시, 입력, 처리 못하는 하드웨어, 소프트웨어)을 사용하는 프로그래머와 시스템을 지원하기 위해

   - 유니코드 이스케이프
      - `\u`문자로 시작하고 그 뒤에 정확히 네 개의 16진수 숫자(0-9, A-F)를 쓰거나, 1-6개의 16진수 숫자를 중괄호 안에 쓰는 형태
      - 자바스크립트 문자열 리터럴, 정규 표현식 리터럴, 식별자에 사용(키워드에는 사용 안함)

   - 유니코드 정규화
      - 자바스크립트 프로그램에 ASCII 문자가 아닌 문자를 사용할 때는 유니코드에 그 문자를 인코딩 방법이 하나 이상 있음을 반드시 인지해야 한다 (ex. `café → caf\u{9}`, `café → cafe\u{301}`)
      - **자바스크립트는 해석하고 있는 소스 코드가 이미 정규화를 마친 상태라고 가정하여 스스로 유니코드 정규화를 수행하지 않는다** ⇒ 에디터나 다른 도구에서 소스코드를 유니코드 정규화(서로 다르지만 눈으로는 구별할 수 없는 식별자가 생기지 않도록)하는 작업이 필요하다.



4. **세미콜론**
   - 세미콜론은 코드의 의미를 명확히 하는 데 중요한 역할을 하지만, 자바스크립트에서 필수는 아니다.
   - 자바스크립트가 줄바꿈을 세미콜론으로 취급하는 경우: 세미콜론을 추가하지 않고서는 코드를 분석할 수 없을 때

    ```js
        let a 
        a 
        = 
        3 
        console.log(a)
        
        // let a; a = 3; console.log(a);
    ```

   - 자바스크립트는 세미콜론 없이 `let a a`라는 코드를 분석할 수 없으므로 첫 번째 줄바꿈을 세미콜론으로 취급한 것
   - 세미콜론을 생략가능할 때
      - 프로그램의 끝일 때
      - 토큰이 닫는 중괄호일 때
      - 두 문 사이에 줄바꿈이 있을 때
     
    <br/>
   
   - 세미콜론을 생략하지 못할 때
      - 명시적인 세미콜론을 써주지 않고서는 줄바꿈이 되어 있어도 코드가 이어진다고 해석될 가능성이 있을때

       ```jsx
           let y = x + f
           (a+b).toString()
          
           // let y = x + f(a+b).toString(); 함수호출로 해석
       ```
        
    <br/>
   
   - 줄바꿈을 해서는 안될때
       1. return, throw, yield, break, continue의 경우, 이 뒤의 식별자와 줄바꿈이 일어난다면 항상 그 줄바꿈을 세미콜론으로 해석한다.

       ```jsx
           return
           true;
          
           // return; true; 
       ```

       2. ++와 —연산자로 이들을 후위 연산자로 사용한다면 반드시 적용할 표현식과 같은 행에 써야한다.
       3.  화살표 함수로, ⇒ 는 반드시 매개변수 리스트와 같은 행에 있어야 한다