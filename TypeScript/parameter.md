# 타입스크립트에서 유용한 함수 파라미터 정리

📌 *들어가기 전에*,

자바스크립트에서도 사용가능하지만, 타입스크립트 사용시에도 완전히 좋은 파라미터 기능들을 배워보자 ✨

## Optional parameter
`?:`
<br>파라미터의 값을 선택적으로 하여, 함수 호출시 인자를 필수로 전달하지 않아도 됨
```
function printName(firstName: string, lastName?: string) {
    console.log(firstName);
    console.log(lastName);
}
printName('Steve','Jobs');
printName('Ellie'); // <= optional parameter를 통해 lastName 인자 전달X
```

## Default parameter
파라미터에 기본 값을 설정하여 인자에 값을 전달하지 않으면 설정한 기본 값이 적용됨
```
function printMessage(message: string = 'default message') {
    console.log(message);
}
printMessage(); // <= 인자로 값을 전달 안해도 default parameter로 설정한 'default message'가 콘솔에 찍힘
```

## Rest parameter
`...rest` 문법을 사용해서 여러개의 인자가 전달되도 허용됨
```
function addNumbers(...numbers: number[]):number {
    return numbers.reduce((acc, cur) => acc + cur, 0)
}
console.log(addNumbers(1, 2));
console.log(addNumbers(1, 2, 3, 4, 5));
```