## Dependencies

dependencies 를 모두 지우려면 `rm -rf ~/.gradle/caches` 로 cache 디렉토리를 삭제하거나 gradle command 에 `--refresh-dependencies` option 을 준다.

root `build.gradle` 에서 `buildscript` 로 선언하는건 plugin 에 대한 설정이다.

```groovy
buildscript {
    repositories {
        // ...
    }   
}
```

subproject 들에 대한 `repositories` 는 `subprojects` 밑에 선언한다.

```groovy
subprojects {
    repositories {
        // ...
    }
}
```