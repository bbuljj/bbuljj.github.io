---
layout: post
title:  "QueryDsl Projection List<Dto> 쿼리 생성"
categories: querydsl
tags: jpa, querydsl
---

#### QueryDsl `Projections` 을 사용해서 **1:N 관계의** `List<Object>` 를 추출하는 코드를 <br /> 작성해보자.
 - 부모 DTO
{% highlight java %}
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class ParentDto {
    private UUID id;
    private String name;

    private List<ChildDto> children = new ArrayList<>();
}
{% endhighlight %}

 - 자식 DTO
{% highlight java %}
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class ChildDto {
    private UUID id;
    private int name;
}
{% endhighlight %}

 - 1:N DTO Projection Query 작성
{% highlight java %}
List<ParentDto> results = new JPAQueryFactory(em)
	.from(parent)
    .innerJoin(child)
    .on(parent.id.eq(child.parent.id))
    .transform(
        groupBy(parent.id).list(
            Projections.fields(
                ParentDto.class,
                parent.id,
                parent.name,
                list( // `GroupByExpression` 의 static method `list()`
                    Projections.fields(
                        ChildDto.class,
                        child.id,
                        child.name)
                ).as("children")
            )
        )
    );
{% endhighlight %}
