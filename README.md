# oreo_slither.io

---

### JS Basic

1. JS Function

	JavaScript의 함수는 속성과 메소드를 가지는 Function 객체  
	- `return`으로 반환값을 지정하지 않으면 `undefined`을 기본값으로 반환  
	- 함수 정의하기
		- 함수 선언
		`function` 키워드 사용  

		- 함수 표현식(function expression)
  		유명함수(named), 익명함수(anonymous) 정의 가능
		함수 표현식은 스코프 상단으로 호이스팅 되지 않으므로 코드 내에 나타나기 전에는 사용할 수 없음  
		함수를 선언하지만 function 키워드가 맨 앞에 오지 않는 것이 함수표현식

    		* anonymous function
    			```
    			var myFunction = function(param){
    				statements
    			}
    			```

    		* named function
    			```
    			var myFunction = function namedFunction(param){
    				statements
    			}
    			```

    		함수표현식에서 이름을 붙여주는 것의 장점은 stack trace에서 함수 이름을 보고 디버깅 할 수 있다는 것

    		* IIFE 즉시 실행 함수 표현식
    			```
    			(function(){
    				statements
    			})();
    			```
    		
    	- 화살표함수 (Arrow Function Expression =>)
    		함수표현식의 더 짧은 버전, 모든 상황에 적용되진 못함
			- [x] this나 super에 대한 바인딩 없음, 메소드로 사용X
			- [x] 생성자로 사용할 수 없음    

    		```
    		// Traditional Anonymous Function
    		function (a){
    		return a + 100;
    		}

    		// Arrow Function Break Down

    		// 1. Remove the word "function" and place arrow between the argument and opening body bracket
    		(a) => {
    		return a + 100;
    		}

    		// 2. Remove the body braces and word "return" -- the return is implied.
    		(a) => a + 100;

    		// 3. Remove the argument parentheses
    		a => a + 100;

    		```
		- generator function 
			이건 나중에 알아보자

	- Function Scope
		대부분의 프로그래밍 언어는 블록단위의 유효범위를 가짐 => JS는 함수를 블록대신 사용
		JS 함수는 자신의 정의된 범위 안에서 정의도니 모든 변수/ 함수에 접근
		함수 안에 정의된 함수의 경우, 부모함수의 변수 및 부모함수가 접근할수 있는 모든 다른 변수에 접근 가능
		```
		// x, y, name을 전역 변수로 선언함.
		var x = 10, y = 20;

		// sub()를 전역 함수로 선언함.
		function sub() {
			return x - y;     // 전역 변수인 x, y에 접근함.
		}

		// parentFunc()을 전역 함수로 선언함.
		function parentFunc() {
			var x = 1, y = 2; // 전역 변수와 같은 이름으로 선언하여 전역 변수의 범위를 제한함.
			function add() {  // add() 함수는 내부 함수로 선언됨.
				return x + y; // 전역 변수가 아닌 지역 변수 x, y에 접근함.
			}
			return add();
		}
		```

		- *__Function Hoisting__*
			JS 함수의 유효볌위 == 선언된 모든 변수는 함수 전체에 걸쳐 유효
			이 범위가 변수가 선언되기 전에도 똑같이 적용
			함수 안에 있는 모든 변수의 선언이 함수의 맨 처음으로 이동된 것처럼 동작
			```
			var globalNum = 10;     // globalNum을 전역 변수로 선언함.

			function printNum() {

				document.write("지역 변수 globalNum 선언 전의 globalNum의 값은 " + globalNum + "입니다.<br>"); // ①

				var globalNum = 20; // globalNum을 지역 변수로 선언함. // ②

				document.write("지역 변수 globalNum 선언 후의 globalNum의 값은 " + globalNum + "입니다.<br>");

			}

			printNum();
			```
			(1)은 `undefined`, (2)는 `20`


	- Parameter
		함수 정의보다 더 적은 매개변수가 전달되면 나머지 매개변수를 undefined로 지정
		```
		function addNum(x, y, z) {
			if(x === undefined) // 함수 호출시 x에 해당하는 인수가 전달되지 않은 경우
				x = 0;          // 변수 x의 값을 undefined에서 0으로 변경함.
			if(y === undefined) // 함수 호출시 y에 해당하는 인수가 전달되지 않은 경우
				y = 0;          // 변수 y의 값을 undefined에서 0으로 변경함.
			if(z === undefined) // 함수 호출시 z에 해당하는 인수가 전달되지 않은 경우
				z = 0;          // 변수 z의 값을 undefined에서 0으로 변경함.
			return x + y + z;
		}

		addNum(1, 2, 3); // 6
		addNum(1, 2);    // 3
		addNum(1);       // 1
		addNum();        // 0
		```

		- argument 객체
			더 많은 매개변수가 들어왔을 경우
			```
			function addNum() {
				var sum = 0;  // 합을 저장할 변수 sum을 선언함.

				for(var i = 0; i < arguments.length; i++) { // 전달받은 인수의 총 수만큼 반복함.
					sum += arguments[i];                    // 전달받은 각각의 인수를 sum에 더함.
				}
				return sum;
			}

			addNum(1, 2, 3); // 6
			addNum(1, 2);    // 3
			addNum(1);       // 1
			addNum();        // 0
			addNum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10); // 55
			```

		- default param, rest param..
			`function mul(a, b = 1) {...}`
			`function sub(firstNum, ...restArgs) {...}`
2. Callback Function
	JS에서 함수는 `first-class object`, 인자 전달 시 함수를 전달할 수 있음 
	=> callback은 다른 함수의 인자로 전달되는 함수

	- synchronous callback
		```
		function greeting(name) {
			alert('Hello ' + name);
		}

		function processUserInput(callback) {
			var name = prompt('Please enter your name.');
			callback(name);
		}

		processUserInput(greeting);
		```
	- Asynchronous callback
		```
		function download(url, callback) {
			setTimeout(() => {
				// script to download the picture here
				console.log(`Downloading ${url} ...`);
				// process the picture once it is completed
				callback(url);

			}, 3000);
		}

		let url = 'https://www.javascripttutorial.net/pic.jpg';
		download(url, function(picture) {
			console.log(`Processing ${picture}`);
		}); 
		```

	[more practice!](https://www.javascripttutorial.net/javascript-callback/)

3. Prototypal OOP


--- 
[reference]  
[function-mozila](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions)
