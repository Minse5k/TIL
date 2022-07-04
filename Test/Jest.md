# Jest

Jest는 페이스북에서 만든 React와 더불어 자바스크립트 개발자로부터 가장 좋은 반응을 보이는 테스팅 라이브러리이다. 출시 초기에는 FE에서 주로 쓰였지만 최근에는 BE에서도 기존의 자바스크립트 테스팅 라이브러리를 대체하고 있다.

## 테스트 코드란 무엇일까?

테스트 코드란 코드가 안정적이고 제대로 동작하는지 확인하기 위해 작성하는 코드이다. 이를 통해 특정 동작을 하는 코드가 예상과 동일하게 결과를 잘 내는지 품질 검사하는데 사용한다.

## 테스트 코드를 작성하면 뭐가 좋을까?

1. 프로그램의 안정성을 보장할 수 있다.
2. 프로그램이 변경(기능 확장 또는 리팩터링 등)이 되더라도 올바르게 작동하는지 확인할 수 있다.
3. 개발 테스트 자동화 / 수동 테스트를 최소화 할 수 있다.

그럼 Jest를 통해 간단한 테스트를 실행해 보겠다.

## 설치

```jsx
npm i -D jest
```

## 스크립트 설정

```jsx
"script": {
	"test": "jest"
},
```

이제 `npm test` `yarn test`를 통해서 테스팅을 실행할 수 있다.

## Matcher 함수

- `not` 을 앞에 사용하면 해당 조건을 만족하지 않는지 체크한다.

```jsx
test("not", () => {
	expect(1).not.toBe(2)
})
```

`.toBe()` : 숫자나 문자와 같은 객체가 아닌 기본형 값을 비교할 때 사용한다.

`.toEqual()` : 객체 단위를 비교할 때 사용한다.

`.toBeTruthy()` `.toBeFalsy()` : true(1)인지 false(0)인지 비교

```jsx
expect(0).toBeFalsly();
```

`.toBeNull()` : `null` 인지 확인

`.toBeUndefined()` : `undefined` 인지 확인

`.toHaveLength()` : 배열의 길이를 확인할 때 사용

`.toContain()` : 배열이 특정원소를 담고 있는지 검사할 때 사용

```jsx
const numbers = ['a', 'b', 'c', 'd']
expect(numbers).toHaveLength(4);
expect(numbers).toContain('b');
```

`.toMatch()` : 일반 문자열이 아닌 정규식 기반의 테스트를 할 때 사용

```jsx
expect(getEmail('gggg@naver.com')).toMatch(/.*naver.com$/);
```

`toThrow()` : 예외 발생 여부를 테스트할 때 사용

- 문자열을 넘기면 예외 메세지를 비교, 정규식을 넘기면 정규식 체크를 한다.
- `.toThrow()` 를 사용할 때는 검증 대상을 함수로 한 번 감싸줘야 한다. 그렇지 않으면 예외 발생 여부를 체크하기 전에, 테스트 실행 도중 그 예외가 발생하여 테스트가 실패한다.

```jsx
test("throw when id is non negative", () => {
	expect(() => getUser(-1).toThrow())
	expect(() => getUser(-1).toThrow("Invalid ID"))
})
```

참고

[https://velog.io/@ppohee/Jest-로-테스트-코드-작성하기](https://velog.io/@ppohee/Jest-%EB%A1%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0)

[https://github.com/kyu9341/TIL/blob/main/Test/jest/jest.md](https://github.com/kyu9341/TIL/blob/main/Test/jest/jest.md)

[https://www.daleseo.com/jest-basic/](https://www.daleseo.com/jest-basic/)