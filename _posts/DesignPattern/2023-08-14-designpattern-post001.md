---
layout: post
title: "[디자인패턴] 싱글톤패턴(Singleton Pattern)"
categories: [Design Pattern]
tags: [singleton, Design Pattern]
comments: true
---

## 싱글톤 패턴(****Singleton pattern****)

하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 사용하는 디자인 패턴이다.

하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용한다. 

- 인스턴스가 절대적으로 한 개만 존재하는 것을 보증하고 싶을 때 사용한다.

- 모듈 간의 결합을 강하게 만들어 의존성이 높아진다는 단점이 있다.
결합도가 높으면 유지보수가 힘들고, 테스트가 원활하게 진행되지 않는다.

## 구현

```java
public class SystemSpeaker {
    
    private static SystemSpeaker instance;
    private int volume;
    
    private SystemSpeaker() {
        volume = 5;
    }
    
    public static SystemSpeaker getInstance() {
        if (instance == null) {
            instance = new SystemSpeaker();
        }
        return instance;
    }
    
    public int getVolume() {
        return volume;
    }
    
    public void setVolume(int volume) {
        this.volume = volume;
    }
    
}

======================================================================================

public class Main{
     
     public static void main(String []args){
        SystemSpeaker speaker = new SystemSpeaker(); //X - 생성자에 접근이 불가능.
        
				SystemSpeaker speaker1 = SystemSpeaker.getInstance(); 
        SystemSpeaker speaker2 = SystemSpeaker.getInstance(); 
        
        System.out.println(speaker1.getVolume());    //5
        System.out.println(speaker2.getVolume());    //5
        
        speaker2.setVolume(11);

        System.out.println(speaker1.getVolume());    //11
        System.out.println(speaker2.getVolume());    //11
        
     }
}
```

- statis으로 인스턴스 변수를 만들어두고, private로 생성자를 만들어 다른 클래스(외부)에서 인스턴스를 생성하지 못하도록 막는다.
    
    ```java
        private static SystemSpeaker instance;
    
    	private SystemSpeaker() {
            volume = 5;
        }
    ```
    
- 외부에서 접근하는 경우 인스턴스가 없다면 생성을 하고, 이후에는 생성된 인스턴스를 불러오기만 한다.
    
    ```java
        public static SystemSpeaker getInstance() {
            if (instance == null) {
                instance = new SystemSpeaker();
            }
            return instance;
        }
    ```
    
- `speaker1`과 `speaker2`는 동일한 인스턴스로 하나의 volume 값을 바꾸더라고 두 개에 똑같이 적용되는 것을 볼 수 있다.