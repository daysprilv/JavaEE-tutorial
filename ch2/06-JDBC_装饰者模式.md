静态代理使用步骤:

1. 要求装饰者和被装饰者实现同一个接口或者继承同一个类
2. 要求装饰者要有被装饰者的引用
3. 对需要加强的方法进行加强
4. 对不需要加强的方法调用原来的方法





Car 类：

``` java
public interface Car {
	void run();
	void stop();
}
```

QQ 类：（被装饰者） ——被代理类

``` java
public class QQ implements Car {

	@Override
	public void run() {
		System.out.println("qq在跑");
	}

	@Override
	public void stop() {
		System.out.println("刹得住车");
	}
}
```

CarWarp 类：（装饰者）——代理类  //这里改装了 QQ 的 `run( )` 方法，`stop( )` 方法没改装
``` java
public class CarWarp implements Car {
	private Car car;

	public CarWarp(Car car){
		this.car=car;
	}
	
    @Override  
	public void run() {
        System.out.println("加上电池");
        System.out.println("我终于可以5秒破百了..");
	}
	@Override
	public void stop() {
		car.stop();
	}
}
```

TTT 类：（测试）

``` java
public class TTT {
	public static void main(String[] args) {
		//如果只有 QQ 这个，正常
        QQ qq = new QQ();
		/*qq.run();
		qq.stop();*/
		
        //现在需要改装，加强需要加强的方法
		CarWarp warp = new CarWarp(qq);
		warp.run();		//这里调用了加强的方法
		warp.stop();	//这里的方法没加强
	}
}
```

TTT 运行结果：

``` html
加上电池
我终于可以5秒破百了..
刹得住车
```



关于以上的理解我口述试试：

首先 Car 车是个接口，存在 QQ 车（被装饰者）需要改装（即需要对方法加强），比如对 QQ 车的 run( ) 方法加强。这里使用装饰者 CarWarp 可以达到目的。

我们可以再看一个代码例子；

``` java
//静态代理模式
//接口
interface ClothFactory {
    void productCloth();
}

// 被代理类
class NikeColthFactory implements ClothFactory {

    @Override
    public void productCloth() {
        System.out.println("Nike工厂生产了一批衣服");
    }
}

// 代理类
class ProxFactory implements ClothFactory {
    ClothFactory cf;

    public ProxFactory(ClothFactory cf) {
        this.cf = cf;
    }

    @Override
    public void productCloth() {
        System.out.println("代理类开始执行，收代理费$1000");
        cf.productCloth();
    }
}

public class TestClothProduct {
    public static void main(String[] args) {
        NikeColthFactory nike = new NikeColthFactory();// 创建被代理类的对象
        ProxFactory prxoy = new ProxFactory(nike);// 创建代理类的对象
        prxoy.productCloth();
    }
}
```

运行结果：

``` xml
代理类开始执行，收代理费$1000
Nike工厂生产了一批衣服
```

