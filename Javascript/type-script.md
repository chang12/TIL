* TypeScript 는 어떻게 JavaScript 로 transpile 되는가?
* import 는 어떻게 하는가?
* Angular 1 과 결합해서 어떤 이득을 볼 수 있는가?

## TypeScript 는 어떻게 JavaScript 로 transpile 되는가?
transpiler 가 아니라 compiler 라고 부른다. TypeScript compiler 는 npm 을 통해서 다운로드한다.

```
npm install -g typescript
```

TypeScript 파일이 존재하는 경로에서 `tsc` 커맨드를 실행하면 `.ts` 파일과 같은 이름의 `.js` 파일이 생성된다.

`tsconfig.json` 설정 파일을 작성해서, compile 된 `.js` 파일이 저장되는 디렉토리를 설정할 수 있다.

```json
{
    "compilerOptions": {
        "outDir": "./js/"
    }
}
```

`.ts` 파일에 대한 상대경로로 값을 설정한다. 해당 위치에 `.js` 파일이 생성되는걸 확인할 수 있다. 한 가지 의아한 점은 `tsc greeter.ts` 처럼 타겟 `.ts` 파일을 명시할 경우 설정값을 무시하고 `.ts` 파일과 같은 위치에 `.js` 파일을 생성해버린다.

## import 는 어떻게 하는가?
TODO

## Angular 1 과 결합해서 어떤 이득을 볼 수 있는가?
TODO