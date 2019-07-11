# java のインスタンスについて

## 概要
クラスは設計書のようなもので、設計書から動くものを作ることをインスタンスという。  
インスタンスを作成すると、インスタンスごとにインスタンス変数として状態が確保される。  

## example

```java
/**
 * 運転手
 */
class Driver {
  private String name;

  Driver(String name) {
    this.name = name;
  }

  public String getName() {
    return this.name;
  }
}

/**
 * 車
 */
class Car {
  /**
   * 運転する
   * @param Driver 運転手
   */
  void drive(driver) {
    System.out.println(driver.getName() + "は運転しています。");
  }
}

Car car = new Car(); // => 車のインスタンスを作成
Driver inoue = new Driver("inoue"); // => inoue のインスタンスを作成し、状態を確保
car.drive(inoue); // => 「inoueは運転しています。」と表示
```
