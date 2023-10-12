# 싱글톤 패턴

### 1. 싱글톤 패턴(Singleton pattern)

웹 어플리케이션은 여러 고객이 동시에 요청할 수 있다.

그러면 고객이 요청할때마다 새로운 객체를 만들어야 할까요?

고객이 요청할때마다 새로운 객체를 만든다고 하면, 트래픽이 초당 100이 발생할때 초당 100개의 객체가 생성됩니다.

성능 저하로 이어질 수도 있다.

이 문제를 해결하려면 객체를 싱글톤으로 관리하면됩니다.

그래서 클라이언트 요청에 해당 객체가 약 1개만 생성되고, 공유하도록 설계하는 것이다.

- 한마디로?

**싱글톤 패턴을 적용하면 고객의 요청이 올 때마다 객체를 생성하는 것이 아니라 , 이미 만들어진 객체를 공유해서 효율적으로 사용할 수 있다.** 

- 싱글톤 패턴의 단점 : 코드가 길어진다
- 클라이언트가 구체 클래스에 의존한다. → DIP 위반, OCP위반 가능성
- 테스트하기 어려움
- private  생성자로 자식 클래스를 만들기 어렵다.
- 유연성이 안좋음

<aside>
💡 그런데 스프링 프레임 워크는 이 모든 단점들을 전부 다 해결하면서 객체를 싱글톤으로 관리함

</aside>

---

예제 : 

싱글톤을 안 사용하지 않은 예

```java
public List<WebToonListResponse> findMostLikedWebtoons() {

        User currentUser = userFacade.getCurrentUser();

        List<WebToon> findMostLikedWebtoon = webToonRepository.findMostLikedWebtoons();

        List<WebToonListResponse> findMostLikedWebtoons = findMostLikedWebtoon
                .stream()
                .map(WebToonListResponse::new)
                .toList();

        return findMostLikedWebtoons;
    }
}
```

싱글톤을 사용한 예

```java
List<Question> questions = questionRepository.findAllByUser(user);

        return QueryQuestionListResponse.builder()
                .questions(questions.stream()
                        .map(question -> QueryQuestionListResponse.QuestionDto.builder()
                                .id(question.getId())
                                .title(question.getTitle())
                                .username(question.getUserName())
                                .isReplied(question.getIsReplied())
                                .isPublic(question.getIsPublic())
                                .createdAt(question.getCreatedAt())
                                .build())
                        .collect(Collectors.toList()))
                .build();
    }
```

예제 2 :

싱글톤을 사용하지 않은 예

```java
if (!user.getId().equals(question.getUserId())) {
            throw new WriterMisMatchedException;
        }
```

싱글톤을 사용한 예

```java
if (!user.getId().equals(question.getUserId())) {
            throw WriterMisMatchedException.EXCEPTION;
        }
```

new 로 최대한 생성을 해주지 않아야 하는 것 같다