# 코틀린으로 파이프라인 패턴 구현하기 (책임연쇄패턴)

```kotlin
// Validator 구현체
interface ValidatorPipe<I, O> {
    fun validate(input: I): O
}

class SomeValidator : ValidatorPipe<String, String> {
    
    override fun validate(input: String): String {
        // something

        return input
    }

}
```

```kotlin
// pipeline 구현체

class ValidatorPipeline<I, O>(private val currentPipe: ValidatorPipe<I, O>) {

    fun <K> addPipe(pipe: ValidatorPipe<O, K>): ValidatorPipeline<I, K> {
        return ValidatorPipeline(object : ValidatorPipe<I, K> {
            override fun validate(input: I): K {
                return pipe.validate(currentPipe.validate(input))
            }
        })
    }

    fun execute(input: I): O {
        return currentPipe.validate(input)
    }
}
```


```kotlin
// client 사용 코드

ValidatorPipeline(SomeValidator())
    .addPipe(MoreValidator())
    .execute(productResult.data)
```

_참고_
- https://java-design-patterns.com/patterns/pipeline/
- https://gist.github.com/mmonti/4c27273d202f12bcb2b61cd89b759c38
