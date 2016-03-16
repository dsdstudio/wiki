## Java ScheduledExecutorService 관련 문제

분단위 작업 스케줄러를 만들기위해 jvm에서 제공하는 ScheduledExecutorService를 이용하였다. 

요구사항으로 

  * 매분 실행이 어떤경우라도 보장되어야함. 
  * 매분실행이 보장되어야하지만 초단위까지는 러프하게 보장, 아무 초에 실행되어도 된다는 이야기. 

Timertask에 비해 자체적으로 관리하는 Threadpool을 제공하고있고 인터페이스도 매우 간편하다. 
간단히 수도코드를 보자면 아래와 같다. 

    scheduler.scheduleWithFixedDelay(m_managerJob, 0, 1, TimeUnit.MINUTES);

그런데 OS별로 약간 다른 결과가나왔는데. 고객사의경우 Windows x64 edition 2003 영문판, Xeon cpu (intel x86)의 사양이였고 스케줄체크를 오랜시간동안 하다보면 특정 분단위를 빠뜨리고 실행을 못시키는 현상이 발견되었다. 

로그를 보고 원인을 분석해본 결과 scheduleWithFixedDelay 저메소드로 실행될때 미묘하게 몇 milliseconds 단위로 시간이 계속 틀어지는 문제가 있었고 어느 시간대가 되어서는 57분 59초 -> 59분 01초로 점프해버리는 문제가 발생하고있었고, 정확한 원인은 감지하지못하였다. 

그래서 일단 기본으로 돌아가서 Java api 문서를 읽어보고 메소드의 정확한 의미를 파악하려 노력하였다. 

2번째로 검증, 실제 코드를 작성해보고 시간대가 틀어지는 테스트를 작성하였다. 
대체품으로 scheduleAtFixedRate 가 있다는것을 발견하고 해당 메소드로 바꾸어 테스트를 돌려보니 시간대의 갭이 OSX/Linux계열에서는 거의 없고 Windows 32 / 64bit계열에서는 오차는 있지만 분단위가 점프하는 현상이 없다는것을 증명해냈다. 

Timer test Code 
```
public class ThreadTest {

    static ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
    static SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");

    public static void main(String[] args) {

        service.scheduleAtFixedRate(new Runnable() {
            @Override
            public void run() {
                System.out.println(format.format(Calendar.getInstance().getTime()));
            }
        }, 0, 1, TimeUnit.MINUTES);
    }
}
```

Linux / Unix 오차가 거의 0.01x 수준 

```
2012-07-02 17:54:33.687
2012-07-02 17:55:33.691
2012-07-02 17:56:33.690
2012-07-02 17:57:33.689
```

그런데 실제 고객사에 패치하고난 10일뒤 같은문제가 또 발생하였고.. 현재 원인을 찾지 못하고있는 상태다. 

이상태에서의 솔루션은 같은 사양의 장비를 수배한뒤 같은 환경으로 만들고 테스트를 해보는것, 그리고 jvm내부에서 어떤 매커니즘으로 scheduleAdFixedRate가 작동하는지 파악해보는것, 또 한가지는 어떤 대안책이 있는지 확인하는것이다. 
