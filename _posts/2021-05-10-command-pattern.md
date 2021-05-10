---
layout: post
title:  "Command Pattern"
categories: design-pattern
tags: java, design_pattern
---

## 커맨드 패턴
 - 요청을(행위들을) 캡슐화하고 매개변수를 사용해서 여러가지 다른 요구사항(다양한 command)들을 수행하는데 용이한 디자인 패턴이다.
 - 구성요소
   - invoker
   - receiver
   - command
   - concrete-command

### 다이어그램

### 예제 [Java]

 - Command interface `command`
{% highlight java %}
interface Command {
  void execute();
}
{% endhighlight %}

 - Save Command `concrete-command`
{% highlight java %}
public class SaveCommand implements Command {
  private Receiver receiver;

  public SaveCommand(Receiver receiver) {
    this.receiver = receiver;
  }

  @Override
  public void execute() {
    receiver.saveReceive();
  }
}
{% endhighlight %}

 - Update Command `concrete-command`
{% highlight java %}
public class UpdateCommand implements Command {
  private Receiver receiver;

  public UpdateCommand(Receiver receiver) {
    this.receiver = receiver;
  }

  @Override
  public void execute() {
    receiver.updateReceive();
  }
}
{% endhighlight %}

- Receiver `receiver`
{% highlight java %}
public class Receiver {
  public void saveReceive() {
    System.out.println("saveReceive!!"); 
  }

  public void updateReceive() {
    System.out.println("updateReceive!!!"); 
  }
}
{% endhighlight %}


 - Executor `invoker`
{% highlight java %}
public class Executor {
  private final Command operation;

  public Executor(Command operation) {
    this.operation = operation;
  }

  public void invoke() {
    this.operation.execute();
  }
}
{% endhighlight %}

 - `main` 메소드
{% highlight java %}
public static void main(String[] args) {
  Receiver receiver = new Receiver();
  Executor executor = new Executor(new SaveCommand(receiver));
  executor.invoke();
  
  executor = new Executor(new UpdateCommand(receiver));
  executor.invoke();
}
{% endhighlight %}
