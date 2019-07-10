## java编程技巧学习（最佳实践初学者）

## 1.return 一个空的集合，而不是 null

如果一个程序返回一个没有任何值的集合，请确保一个空集合返回，而不是空的元素，这样可以避免写一大堆的“if-else” 判断null元素。java标准库设计在collections类中设置了List的常量EMPTY_LIST,除此之外，还有EMPTY_MAP， EMPTY_SET等；

```
package com.demo;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class CollectionEmptyTest {
	
	public static void main(String[] args) {
		int i = 0;
		List<String> ls = getname(i);
		for (String string : ls) {
			System.out.println(string);
		}
	}
	
	private static List<String> getname(int limit) {
		List<String> list = new ArrayList<String>();
		for (Integer i = 0; i < limit; i++) {
			list.add(i.toString());
		}
		if(list.size()==0){
			//return null,在数据为null,遍历会有空指针异常问题
			return Collections.emptyList();
		}else {
			return list;
		}
	}
}

```

## 2.小心使用string

因为字符串相加相减或者拼接的方式在对象池中查找字符串是否存在，如果不存在则创建，这样在拼接的过程中产生大量的中间过程字符串，占用内存资源；StringBuilder效率优于stringbuffer,但stringbuffer线程安全

（1）如果你要求字符串不可变，那么应该选择String类

（2）如果你需要字符串可变并且是线程安全的，那么你应该选择StringBuffer类

（3）如果你要求字符串可变并且不存在线程安全问题，那么你应该选择StringBuilder类

## 3.避免不必要的对象

一个最昂贵的操作是java对象的创建，因此，建议只在必要的时候创建或初始化对象。

```
private List Employees;
public List getEmployees(){
    if(null == Employees){
       Employees = new ArrayList(); 
    }
    retrun Employees;
}
```

### 4.Array 和ArrayList 选择 

ArrayList和Array是我们在实际编程中经常使用的容器，而且因为ArrayList相当于动态化的数组，所以它们之间有太多的相似，以至于我们在选择哪种来存储元素的时候，会有小小的迷惑，他们都有注解的优缺点，选择真的取决于你的真实场景。 

4.1.Array 有固定大小但 ArrayList 的大小不同。由于Array 的大小是固定的，在Array 类型变量声明的时候，内存被分配。因此，Array 是非常快的。另一方面， 使用ArrayList的最大缺点就是当我们添加新的元素的时候，它是先检查内部数组的容量是否足够，如果不够，它会创建一个容量为原来数组两倍的新数组，后将所有元素复制到新数组里，接着抛掉旧数组。这点真的很麻烦，因为每次都要这么搞，尤其是元素足够多的时候，这就真的是既影响内存又影响效率的问题，但通过单独测试它们的运行时间，发现其实差不多，最大的影响就是如果是有其他代码也需要使用到内存，那么Array依然不变，但是ArrayList就会变得慢多，相同情况下所花的时间是Array的四倍多(实际情况是远远不止)。

4.2.这是添加或删除元素从ArrayList 比Array更容易。

4.3.数组可以多维但ArrayList只有一个维度。

4.4.ArrayList因为内部是一个数组，所以它是可以转化为数组的。

4.5 两者的适用场合：

List list = new ArrayList();

虽然我们想要的确实是ArrayList而不是list,但是我们知道，父类是可以获得子类的引用并且使用子类的方法，所以这样我们就能同时使用List和ArrayList的方法而不用害怕出错了。

首先，一个重要的约束就是，List的声明区域一般是在main方法里(当然静态list也可以，但是我们一般使用的时候都只是当做存储元素的临时容器)，而Array是可以在外部进行声明的，这时它就是全局数组。所以，单看这点，它们的使用已经有区别，如果想要保存一些在整个程序运行期间都会存在而且不变的数据，我们可以将它们放进一个全局数组里，但是如果我们单纯只是想要以数组的形式保存数据，方便我们进行查找，那么，我们就选择ArrayList。而且还有一个地方是必须知道的，就是如果我们需要对元素进行频繁的移动或删除，或者是处理的是超大量的数据，那么，使用ArrayList就真的不是一个好的选择，因为它的效率很低，使用数组进行这样的动作就很麻烦，那么，我们可以考虑选择LinkedList。