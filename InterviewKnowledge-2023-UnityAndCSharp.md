本文将整理的面试题由于大部分常用的面试题在网上基本上已经有比较标准的答案了，所以说面试题类的文章基本上大同小异。
所以本篇文章中的部分内容也是直接从网上摘选来的
如果有不对的地方也欢迎指正 (尽力不会出现这种情况)，某个模块的内容不够也欢迎在评论区指出，我去重新添加上。

- **C# 基础**
- **计算机基础**
- **Unity 基础**
- **物理系统**
- **UI 部分** & **2D**
- **协程**
- **动画系统**
- **数据持久化** & **资源管理**
- **Lua 语言和 Xlua 热更**
- **网络**
- **渲染 & Shader**
- **优化部分**
- **算法**

本文部分内容参考文章如下：
https://blog.csdn.net/u014361280/article/details/100135129
https://blog.csdn.net/u014361280/article/details/121499143
https://blog.csdn.net/u014361280/article/details/121534764
https://blog.csdn.net/qq_21407523/article/details/108814300

------

## 🎬Unity 面试题大全

### ❤️C# 基础

#### 1. 重载和重写的区别

1. 封装、继承、多态所处位置不同，重载在同类中，重写在父子类中。
2. 定义方式不同，重载方法名相同参数列表不同，重写方法名和参数列表都相同。
3. 调用方式不同，重载使用相同对象以不同参数调用，重写用不同对象以相同参数调用。
4. 多态时机不同，重载时编译时多态，重写是运行时多态。

#### 2. 面向对象的三大特点

1. 继承： 提高代码重用度，增强软件可维护性的重要手段，符合开闭原则。继承最主要的作用就是把子类的公共属性集合起来，便与共同管理，使用起来也更加方便。你既然使用了继承，那代表着你认同子类都有一些共同的特性，所以你把这些共同的特性提取出来设置为父类。继承的传递性：传递机制 a▶b; b▶c; c 具有 a 的特性 。继承的单根性：在 C# 中一个类只能继承一个类，不能有多个父类。
2. 封装： 封装是将数据和行为相结合，通过行为约束代码修改数据的程度，增强数据的安全性，属性是 C# 封装实现的最好体现。就是将一些复杂的逻辑经过包装之后给别人使用就很方便，别人不需要了解里面是如何实现的，只要传入所需要的参数就可以得到想要的结果。封装的意义在于保护或者防止代码（数据）被我们无意中破坏。
3. 多态性： 多态性是指同名的方法在不同环境下，自适应的反应出不同得表现，是方法动态展示的重要手段。多态就是一个对象多种状态，子类对象可以赋值给父类型的变量。

#### 3. 简述值类型和引用类型有什么区别

**值类型**：包含了所有简单类型（整数、浮点、bool、char）、struct、enum。
继承自 System.ValueTyoe
**引用类型**包含了 string，object，class，interface，delegate，array
继承自 System.Object

1. 值类型存储在内存栈中，引用类型数据存储在内存堆中，而内存单元中存放的是堆中存放的地址。
2. 值类型存取快，引用类型存取慢。
3. 值类型表示实际数据，引用类型表示指向存储在内存堆中的数据的指针和引用。
4. 栈的内存是自动释放的，堆内存是.NET 中会由 GC 来自动释放。
5. 值类型继承自 System.ValueType, 引用类型继承自 System.Object。
6. 值类型的变ᰁ直接存放实际的数据，⽽引⽤类型的
   变ᰁ存放的则是数据的地址，即对象的引⽤。
7. 值类型变量直接把变量的值保存在堆栈中，引⽤类
   型的变量把实际数据的地址保存在堆栈中。

#### 4. 请简述 private，public，protected，internal 的区别

- public：对任何类和成员都公开，无限制访问
- private：仅对该类公开
- protected：对该类和其派生类公开
- internal：只能在包含该类的程序集中访问该类
- protected internal：protected + internal

#### 5.C# 中所有引用类型的基类是什么

引用类型的基类是 System.Object 值类型的基类是 System.ValueType

同时，值类型也隐式继承自 System.Object

#### 6. 请简述 ArrayList 和 List 的主要区别

- ArrayList 不带泛型 数据类型丢失
- List 带泛型 数据类型不丢失
- ArrayList 需要装箱拆箱 List 不需要

ArrayList 存在不安全类型（ArrayList 会把所有插 ⼊其中的数据都当做 Object 来处理）装箱拆箱的 操作（费时）IList 是接⼝，ArrayList 是⼀个实现了 该接⼝的类，可以被实例化

List 类是 ArrayList 类的泛型等效类。它的大部分用法都与 ArrayList 相似，因为 List 类也继承了 IList 接口。最关键的区别在于，在声明 List 集合时，我们同时需要为其声明 List 集合内数据的对象类型。

#### 7. 请简述 GC（垃圾回收）产生的原因，并描述如何避免？

GC 为了避免内存溢出而产生的回收机制

避免：
1）减少 new 产生对象的次数
2）使用公用的对象（静态成员）
3）将 String 换为 StringBuilder

#### 8. 请描述 Interface 与抽象类之间的不同

1. 接口不是类 不能实例化 抽象类可以间接实例化
2. 接口是完全抽象 抽象类为部分抽象
3. 接口可以多继承 抽象类是单继承

#### 9. 请简述关键字 Sealed 用在类声明和函数声明时的作用

类声明时可防止其他类继承此类，在方法中声明则可防止派生类重写此方法。

#### 10. 反射的实现原理？

可以在加载程序运行时，动态获取和加载程序集，并且可以获取到程序集的信息反射即在运行期动态获取类、对象、方法、对象数据等的一种重要手段

主要使用的类库：System.Reflection

**核心类：**

1. Assembly 描述了程序集
2. Type 描述了类这种类型
3. ConstructorInfo 描述了构造函数
4. MethodInfo 描述了所有的方法
5. FieldInfo 描述了类的字段
6. PropertyInfo 描述类的属性

通过以上核心类可在运行时动态获取程序集中的类，并执行类构造产生类对象，动态获取对象的字段或属性值，更可以动态执行类方法和实例方法等。

#### 11. .Net 与 Mono 的关系？

.Net 是一个语言平台，Mono 为.Net 提供集成开发环境，集成并实现了.NET 的编译器、CLR 和基础类库，使得.Net 既可以运行在 windows 也可以运行于 linux，Unix，Mac OS 等。

#### 12. 在类的构造函数前加上 static 会报什么错？为什么？

构造函数格式为 public + 类名如果加上 static 会报错（静态构造函数不能有访问、型的对象，静态构造函数只执行一次；
运行库创建类实例或者首次访问静态成员之前，运行库调用静态构造函数；
静态构造函数执行先于任何实例级别的构造函数；
显然也就无法使用 this 和 base 来调用构造函数。
一个类只能有一个静态函数，如果有静态变量，系统也会自动生成静态函数

#### 13.C# String 类型比 stringBuilder 类型的优势是什么？

如果是处理字符串的话，用 string 中的方法每次都需要创建一个新的字符串对象并且分配新的内存地址，而 stringBuilder 是在原来的内存里对字符串进行修改，所以在字符串处理

方面还是建议用 stringBuilder 这样比较节约内存。但是 string 类的方法和功能仍然还是比 stringBuilder 类要强。

string 类由于具有不可变性（即对一个 string 对象进行任何更改时，其实都是创建另外一个 string 类的对象），所以当需要频繁的对一个 string 类对象进行更改的时候，建议使用 StringBuilder 类，StringBuilder 类的原理是首先在内存中开辟一定大小的内存空间，当对此 StringBuilder 类对象进行更改时， 如果内存空间大小不够， 会对此内存空间进行扩充，而不是重新创建一个对象，这样如果对一个字符串对象进行频繁操作的时候，不会造成过多的内存浪费，其实本质上并没有很大区别，都是用来存储和操作字符串的，唯一的区别就在于性能上。

String 主要用于公共 API，通用性好、用途广泛、读取性能高、占用内存小。

StringBuilder 主要用于拼接 String，修改性能好。

不过现在的编译器已经把 String 的 + 操作优化成 StringBuilder 了， 所以一般用 String 就可以了

String 是不可变的，所以天然线程同步。

StringBuilder 可变，非线程同步。

#### 14.C# 函数 Func (string a, string b) 用 Lambda 表达式怎么写？

```csharp
(a,b) => {};
```

#### 15. 数列 1,1,2,3,5,8,13… 第 n 位数是多少？用 C# 递归算法实现

```csharp
解释public int CountNumber(int num) {
       if (num == 1 || num == 2) {
           return 1;
       } else {
           return CountNumber(num -1) + CountNumber(num-2);
       }
  }
```

#### 16. 冒泡排序（手写代码）

```csharp
解释public static void BubblingSort(int[]array) {

      for (int i = 0; i < array.Length; i++){
      
          for (int j = array.Length - 1; j > 0; j--){
          
              if (array[j] < array[i]) {
              
                  int temp = array[j];
                  array[j] = array[j-1];
                  array[j - 1] = temp;
              }
          }
      }
  }
```

#### 17. C# 中有哪些常用的容器类，各有什么特点。

List，HashTable，Dictionary，Stack，Queue

- **Stack 栈**：先进后出，入栈和出栈，底层泛型数组实现，入栈动态扩容 2 倍
- **Queue 队列**：先进先出，入队和出队，底层泛型数组实现，表头表尾指针，判空还是满通过 size 比较
  Queue 和 Stack 主要是用来存储临时信息的
- **Array 数组**：需要声明长度，不安全
- **ArrayList 数组列表**：动态增加数组，不安全，实现了 IList 接口（表示可按照索引进行访问的非泛型集合对象），Object 数组实现
- **List 列表**：底层实现是泛型数组，特性，动态扩容，泛型安全
  将泛型数据（对值类型来说就是数据本身，对引用类型来说就是引用）存储在一个泛型数组中，添加元素时若超过当前泛型数组容量，则以 2 倍扩容，进而实现 List 大小动态可变。（注：大小指容量，不是 Count）
- **LinkList 链表**
  1、数组和 List、ArrayList 集合都有一个重大的缺陷，就是从数组的中间位置删除或插入一个元素需要付出很大的代价，其原因是数组中处于被删除元素之后的所有元素都要向数组的前端移动。
  2、LinkedList（底层是由链表实现的）基于链表的数据结构，很好的解决了数组删除插入效率低的问题，且不用动态的扩充数组的长度。
  3、LinkedList 的优点：插入、删除元素效率比较高；缺点：访问效率比较低。
- **HashTable 哈希表（散列表）**
  概念：不定长的二进制数据通过哈希函数映射到一个较短的二进制数据集，即 Key 通过 HashFunction 函数获得 HashCode
  装填因子：α=n/m=0.72 , 存储的数据 N 和空间大小 M
  然后通过哈希桶算法，HashCode 分段，每一段都是一个桶结构，一般是 HashCode 直接取余。
  桶结构会加剧冲突，解决冲突使用拉链法，将产生冲突的元素建立一个单链表，并将头指针地址存储至 Hash 表对应桶的位置。这样定位到 Hash 表桶的位置后可通过遍历单链表的形式来查找元素。
  1、Key—Value 形式存取，无序，类型 Object，需要类型转换。
  2、Hashtable 查询速度快，而添加速度相对慢
  3、Hashtable 中的数据实际存储在内部的一个数据桶里（bucket 结构体数组），容量固定，根据数组索引获取值。

```csharp
解释//哈希表结构体
private struct bucket {
   public Object key;//键
    public Object val;//值
    public int hash_col;//哈希码
}
//字典结构体
private struct Entry {
    public int hashCode;    // 除符号位以外的31位hashCode值, 如果该Entry没有被使用，那么为-1
    public int next;        // 下一个元素的下标索引，如果没有下一个就为-1
    public TKey key;        // 存放元素的键
    public TValue value;    // 存放元素的值
}

private int[] buckets;      // Hash桶
private Entry[] entries;    // Entry数组，存放元素
private int count;          // 当前entries的index位置
private int version;        // 当前版本，防止迭代过程中集合被更改
private int freeList;       // 被删除Entry在entries中的下标index，这个位置是空闲的
private int freeCount;      // 有多少个被删除的Entry，有多少个空闲的位置
private IEqualityComparer<TKey> comparer;   // 比较器
private KeyCollection keys;     // 存放Key的集合
private ValueCollection values;     // 存放Value的集合
```

**性能排序：**

- 插入性能： LinkedList > Dictionary > HashTable > List
- 遍历性能：List > LinkedList > Dictionary > HashTable
- 删除性能： Dictionary > LinkedList > HashTable > List

#### 18. C# 中常规容器和泛型容器有什么区别，哪种效率高？

不带泛型的容器需要装箱和拆箱操作速度慢所以泛型容器效率更高数据类型更安全

#### 19. 有哪些常见的数值类？

简单值类型：包括 整数类型、实数类型、字符类型、布尔类型

复合值类型：包括 结构类型、枚举类型

#### 20. C# 中委托 和 接口有什么区别？各用在什么场合？

**接口（interface）**是约束类应该具备的功能集合，约束了类应该具备的功能，使类从千变万化的具体逻辑中解脱出来，便于类的管理和扩展，同时又合理解决了类的单继承问题。

**C# 中的委托** 是约束方法集合的一个类，可以便捷的使用委托对这个方法集合进行操作。

在以下情况中使用接口：

\1. 无法使用继承的场合
\2. 完全抽象的场合
\3. 多人协作的场合

以上等等

在以下情况中使用委托：多用于事件处理中

#### 21. C# 中 unsafe 关键字是用来做什么的？什么场合下使用？

非托管代码才需要这个关键字一般用在带指针操作的场合。
项目背包系统的任务装备栏使用到

#### 22. C# 中 ref 和 out 关键字有什么区别？

**ref 修饰引用参数**。参数必须赋值，带回返回值，又进又出
**out 修饰输出参数**。参数可以不赋值，带回返回值之前必须明确赋值，
引用参数和输出参数不会创建新的存储位置

如果 ref 参数是值类型，原先的值类型数据，会随着方法里的数据改变而改变，
如果 ref 参数值引用类型，方法里重新赋值后，原对象堆中数据会改变，如果对引用类型再次创建新对象并赋值给 ref 参数，引用地址会重新指向新对象堆数据。方法结束后形参和新对象都会消失。实参还是指向原始对象，值不够数据改变了

#### 23. For，foreach，Enumerator.MoveNext 的使用，与内存消耗情况

for 循环可以通过索引依次进行遍历，foreach 和 Enumerator.MoveNext 通过迭代的方式进行遍历。
内存消耗上本质上并没有太大的区别。
但是在 Unity 中的 Update 中，一般不推荐使用 foreach 因为会遗留内存垃圾。

#### 24. 函数中多次使用 string 的 += 处理，会产生大量内存垃圾（垃圾碎片），有什么好的方法可以解决。

通过 StringBuilder 那进行 append，这样可以减少内存垃圾

#### 25. 当需要频繁创建使用某个对象时，有什么好的程序设计方案来节省内存？

设计单例模式进行创建对象或者使用对象池

#### 26. JIT 和 AOT 区别

Just-In-Time - 实时编译

执行慢安装快占空间小一点

Ahead-Of-Time - 预先编译

执行快安装慢占内存占外存大

#### 27. 给定一个存放参数的数组，重新排列数组

void SortArray(Array arr){Array.Sort(arr);}

#### 28. Foreach 循环迭代时，若把其中的某个元素删除，程序报错，怎么找到那个元素？以及具体怎么处理这种情况？(注：Try…Catch 捕捉异常，发送信息不可行)

foreach 不能进行元素的删除，因为迭代器会锁定迭代的集合，解决方法：记录找到索引或者 key 值，迭代结束后再进行删除。

#### 29. GameObject a=new GameObject () GameObject b=a 实例化出来了 A，将 A 赋给 B，现在将 B 删除，问 A 还存在吗？

存在，b 删除只是将它在栈中的内存删除，而 A 对象本身是在堆中，所以 A 还存在

#### 30. C# 中 委托和事件的区别

大致来说，委托是一个类，该类内部维护着一个字段，指向一个方法。事件可以被看作一个委托类型的变量，通过事件注册、取消多个委托或方法。

○ 委托就是一个类，也可以实例化，通过委托的构造函数来把方法赋值给委托实例
○ 触发委托有 2 种方式：委托实例.Invoke (参数列表)，委托实例 (参数列表)
○ 事件可以看作是一个委托类型的变量
○ 通过 += 为事件注册多个委托实例或多个方法
○ 通过 -= 为事件注销多个委托实例或多个方法
○ EventHandler 就是一个委托

#### 31. 结构体和类有何区别？

结构体是一种值类型，而类是引用类型。(值类型、引用类型是根据数据存储的⻆度来分的) 就是值类型用于存储数据的值，引用类型用于存储对实际数据的引用。

那么结构体就是当成值来使用的，类则通过引用来对实际数据操作

#### 32. C# 的委托是什么？有何用处？

委托类似于一种安全的指针引用，在使用它时是 当做类来看待而不是一个方法，相当于对一组方 法的列表的引用。

用处：使用委托使程序员可以将方法引用封装在 委托对象内。然后可以将该委托对象传递给可调 用所引用方法的代码，而不必在编译时知道将调 用哪个方法。与 C 或 C++ 中的函数指针不同，委托 是面向对象，而且是类型安全的。

#### 33. foreach 迭代器遍历和 for 循环遍历的区别

如果集合需要 foreach 遍历，是否可行，存在一定问题
foreach 中的迭代变量 item 是的只读，不能对其进行修改，比如 list.Remove（item）操作
foreach 只读的时候记录下来，在对记录做操作，或者直接用 for 循环遍历
foreach 对 int [] 数组循环已经不产生 GC，避免对 ArrayList 进行遍历

for 语句中初始化变量 i 的作用域，循环体内部可见。
通过索引进行遍历，可以根据索引对所遍历集合进行修改
unity 中 for 循环使用 lambda 表达式注意闭包问题

foreach 遍历原理
任何集合类（Array）对象都有一个 GetEnumerator () 方法，该方法可以返回一个实现了 IEnumerator 接口的对象。
这个返回的 IEnumerator 对象既不是集合类对象，也不是集合的元素类对象，它是一个独立的类对象。
通过这个实现了 IEnumerator 接口对象 A，可以遍历访问集合类对象中的每一个元素对象
对象 A 访问 MoveNext 方法，方法为真，就可以访问 Current 方法，读取到集合的元素。

```csharp
解释	    List<string> list = new List<string>() { "25", "哈3", "26", "花朵" };
 		IEnumerator listEnumerator = list.GetEnumerator();
        while (listEnumerator.MoveNext())
        {
            Console.WriteLine(listEnumerator.Current);
        }
```

#### 34. C# 和 C++ 的区别？

简单的说：C# 与 C++ 比较的话，最重要的特性 就是 C# 是一种完全面向对象的语言，而 C++ 不 是，另外 C# 是基于 IL 中间语言
和.NET Framework CLR 的，在可移植性，可维 护性和强壮性都比 C++ 有很大的改进。C# 的设 计目标是用来开发快速稳定可扩展的应用程序， 当然也可以通过 Interop 和 Pinvoke 完成一些底层操作

**具体对比**：

1. 继承：C++ 支持多继承，C# 类只能继承一个基类中的实现但可以实现多个接口。
2. 数组：声明 C# 数组和声明 C++ 数组的语法不同。在 C# 中，“[]” 标记出现在数组类型的后面。
3. 数据类型：在 C++ 中 bool 类可以与整型转换，但 C# 中 bool 类型和其他类型（特别是 int）之间没有转换。long 类型：在 C# 中，long 数据类型为 64 位，而在 C++ 中为 32 位。
4. struct 类型：在 C# 中，类和结构在语义上不同。struct 是值类型，而 class 是引用类型。
5. switch 语句：与 C++ 中的 switch 语句不同，C# 不支持从一个 case 标签贯穿到另一个 case 标签。
6. delegate 类型：委托与 C++ 中的函数指针基本相似，但前者具有类型安全，是安全的。
7. 从派生类调用重写基类成员。 base
8. 使用 new 修饰符显式隐藏继承成员。
9. 重写方法需要父类方法中用 virtual 声名，子类方法用 override 关键字。
10. 预处理器指令用于条件编译。C# 中不使用头文件。 C# 预处理器指令
11. 异常处理：C# 中引入了 finally 语句，这是 C++ 没有的。
12. C# 运算符：C# 支持其他运算符，如 is 和 typeof。它还引入了某些逻辑运算符的不同功能。
13. static 的使用，static 方法只能由类名调用，改变 static 变量。
14. 在构造基类上替代 C++ 初始化列表的方法。
15. Main 方法和 C++ 及 Java 中的 main 函数的声明方式不同，Main 而不能用 main
16. 方法参数：C# 支持 ref 和 out 参数，这两个参数取代指针通过引用传递参数。
17. 在 C# 中只能在 unsafe 不安全模式下才使用指针。
18. 在 C# 中以不同的方式执行重载运算符。
19. 字符串：C# 字符串不同于 C++ 字符串。
20. foreach:C# 從 VB 中引入了 foreach 关键字使得以循环访问数组和集合。
21. C# 中没有全局方法和全局变量：方法和变量必须包含在类型声明（如 class 或 struct）中。
22. C# 中没有头文件和 #include 指令：using 指令用于引用其他未完全限定类型名的命名空间中的类型。
23. C# 中的局部变量在初始化前不能使用。
24. 析构函数：在 C# 中，不能控制析构函数的调用时间，原因是析构函数由垃圾回收器自动调用。 析构函数
25. 构造函数：与 C++ 类似，如果在 C# 中没有提供类构造函数，则为您自动生成默认构造函数。该默认构造函数将所有字段初始化为它们的默认值。
26. 在 C# 中，方法参数不能有默认值。如果要获得同样的效果，需使用方法重载。

#### 35. C# 引用和 C++ 指针的区别

C# 不支持指针，但可以使用 Unsafe，不安全模式，CLR 不检测
C# 可以定义指针的类型、整数型、实数型、struct 结构体
C# 指针操作符、C# 指针定义
使用 fixed，可以操作类中的值类型
相同点：都是地址
指针指向一块内存，它的内容是所指内存的地址；而引用则是某块内存的别名。

**不同点**：
指针是个实体，引用是个别名。
sizeof 引用” 得到的是所指向的变量 (对象) 的大小，而 “sizeof 指针” 得到的是指针本身的大小；
引用是类型安全的，而指针在不安全模式下

#### 36. 堆和栈的区别？

通常保存着我们代码执行的步骤，如在代码段 1 中 AddFive () 方法，int pValue 变量，int result 变量等等。 而堆上存放的则多是对象，数据等。(译者注：忽略编 译器优化) 我们可以把栈想象成一个接着一个叠放在 一起的盒子。当我们使用的时候，每次从最顶部取走 一个盒子。栈也是如此，当一个方法 (或类型) 被调 用完成的时候，就从栈顶取走 (called a Frame，译 注：调用帧)，接着下一个。堆则不然，像是一个仓 库，储存着我们使用的各种对象等信息，跟栈不同的 是他们被调用完毕不会立即被清理掉。

#### 37. Heap 与 Stack 有何区别？

1. heap 是堆，stack 是栈。
2. stack 的空间由操作系统自 动分配和释放，heap 的空间是手动申请和释放的， heap 常用 new 关键字来分配。
3. stack 空间有限，heap 的空间是很大的自由区。

#### 38. Mock 和 Stub 有何区别？

Mock 与 Stub 的区别：Mock: 关注行为验证。细粒度的 测试，即代码的逻辑，多数情况下用于单元测试。 Stub: 关注状态验证。粗粒度的测试，在某个依赖系 统不存在或者还没实现或者难以测试的情况下使用， 例如访问文件系统，数据库连接，远程协议等。

#### 39. 为什么 dynamic font 在 unicode 环境下优于 staticfont（字符串编码）

Unicode 是国际组织制定的可以容纳世界上所有⽂字和符号的字符编码⽅案。
使⽤动态字体时，Unity 将不会预先⽣成⼀个与所有字体的字符纹理。
当需要⽀持亚洲语⾔或者较⼤的字体的时候，若使⽤正常纹理，则字体的纹理将⾮常⼤。

------

作者：[呆呆敲代码的小 Y](https://xiaoy.blog.csdn.net/)
来源：https://blog.csdn.net/zhangay1998/article/details/122626334

#### 40. 简述 StringBuilder 和 String 的区别？（字符串处理）

String 是字符串常量。StringBuffer 是字符串变量 ，线程安全。StringBuilder 是字符串变量，线程不安全。

String 类型是个不可变的对象，当每次对 String 进⾏改变时都需要⽣成⼀个新的 String 对象，然后将指针指向⼀个新的对象，如果在⼀个循环⾥⾯，不断的改变⼀个对象，就要不断的⽣成新的对象，所以效率很低，建议在不断更改 String 对象的地⽅不要使⽤ String 类型。

StringBuilder 对象在做字符串连接操作时是在原来的字符串上进⾏修改，改善了性能。这⼀点我们平时使⽤中也许都知道，连接操作频繁的时候，使⽤ StringBuilder 对象。

#### 41. string、stringBuilder、stringBuffer

- **String** 不变性，字符序列不可变，对原管理中实例对象赋值，会重新开一个新的实例对象赋值，新开的实例对象会等待被 GC。
  string 拼接要重新开辟空间，因为 string 原值不会改变，导致 GC 频繁，性能消耗大
- **StringBuffer** 是字符串可变对象，可通过自带的 StringBuffer. 方法来改变并生成想要的字符串。对原实例对象做拼接的实例，不会生成新的实例对象。
  拼接使用 StringBuilder 和 StringBuffer，只开辟一个内存空间，这是性能优化的点。
- **StringBuilder** 是字符串可变对象，基本和 StringBuilder 相同。
  唯一的区别是 StringBuffer 是线程安全，相关方法前带 synchronized 关键字，一般用于多线程
  StringBuilder 是非线程安全，所以性能略好，一般用于单线程



三者性能比较 StringBuilder>StringBuffer>String

1. 如果要操作少量的数据 =string
2. 单线程操作字符串缓冲区 下操作大量数据 = StringBuilder
3. 多线程操作字符串缓冲区 下操作大量数据 = StringBuffer

#### 42. 字典 Dictionary 的内部实现原理

泛型集合命名空间 using System.Collections.Generic;
任何键都必须是唯一

该类最大的优点就是它查找元素的时间复杂度接近 O (1)，实际项目中常被用来做一些数据的本地缓存，提升整体效率。

**实现原理**

1. 哈希算法：将不定长度的二进制数据集给映射到一个较短的二进制长度数据集一个 Key 通过 HashFunc 得到 HashCode
2. Hash 桶算法：对 HashCode 进行分段显示，常用方法是对 HashCode 直接取余
3. 解决碰撞冲突算法（拉链法）：分段会导致 key 对应的桶会相同，拉链法的思想就像对冲突的元素，建立一个单链表，头指针存储到对应的哈希桶位置。反之就是通过确定 hash 桶位置后，遍历单链表，获取对应的 value

#### 43. 泛型是什么

多个代码对 【不同数据类型】 执行 【相同指令】的情况
泛型：多个类型共享一组代码
泛型允许类型参数化，泛型类型是类型的模板
5 种泛型：类、结构、接口、委托、方法
类型占位符 T 来表示泛型

泛型类不是实际的类，而是类的模板
从泛型类型创建实例
声明泛型类型》通过提供【真实类型】创建构造函数类型》从构造类型创建实例
类 <T1,T2> 泛型类型参数

**性能**：泛型不会强行对值类型进行装箱和拆箱，或对引用类型进行向下强制类型转换，所以性能得到提高

**安全**：通过知道使用泛型定义的变量的类型限制，编译器可以在一定程度上验证类型假设，所以泛型提高了程序的类型安全。

#### 44. Mathf.Round 和 Mathf.Clamp 和 Mathf.Lerp 含义？

- Mathf.Round：四舍五入
- Mathf.Clamp：左右限值
- Mathf.Lerp：插值

#### 45. 能用 foreach 遍历访问的对象需要实现___***接⼝或声明\***______⽅法的类型（C# 遍历）

IEnumerable；GetEnumerator

List 和 Dictionary 类型可以用 foreach 遍历，他们都实现了 IEnumerable 接口，申明了 GetEnumerator 方法。
![img](D:\Unity资料和笔记\个人笔记\Picture-2023\1710213572-500831-920c06dd780f4f0f81db839c2a2032ee.png)

#### 46. 什么是里氏替换原则？（C# 多态）

里氏替换原则 (Liskov Substitution Principle LSP) ⾯向对象设计的基本原则之⼀。

- 里氏替换原则中说，任何基类可以出现的地⽅，⼦类⼀定可以出现，作⽤⽅便扩展功能能
- 子类可以实现父类的抽象方法，但是不能覆盖父类的非抽象方法。
- 子类中可以增加自己特有的方法。
- 当子类覆盖或实现父类的方法时，方法的前置条件（即方法的形参）要比父类方法的输入参数更宽松。
- 当子类的方法实现父类的抽象方法时，方法的后置条件（即方法的返回值）要比父类更严格。

#### 47. 反射的实现原理？

可以在加载程序运行时，动态获取和加载程序集，并且可以获取到程序集的信息反射即在运行期动态获取类、对象、方法、对象数据等的一种重要手段

主要使用的类库：System.Reflection

**核心类：**

1. Assembly 描述了程序集
2. Type 描述了类这种类型
3. ConstructorInfo 描述了构造函数
4. MethodInfo 描述了所有的方法
5. FieldInfo 描述了类的字段
6. PropertyInfo 描述类的属性

通过以上核心类可在运行时动态获取程序集中的类，并执行类构造产生类对象，动态获取对象的字段或属性值，更可以动态执行类方法和实例方法等。

审查元数据并收集关于它的类型信息的能⼒。

```csharp
解释实现步骤：
1. 导⼊using System.Reflection;
2. Assembly.Load("程序集")加载程序集,返回类型是
⼀个Assembly
3.  foreach (Type type in assembly.GetTypes())
         {
            string t = type.Name;
         }
得到程序集中所有类的名称
4. Type type = assembly.GetType("程序集.类名");获取
当前类的类型
5. Activator.CreateInstance(type); 创建此类型实例
6. MethodInfo mInfo = type.GetMethod("⽅法名");获取
当前⽅法
7. mInfo.Invoke(null,⽅法参数);
```

#### 48. 概述 c# 中代理和事件？

代理就是⽤来定义指向⽅法的引⽤。
C＃事件本质就是对消息的封装，⽤作对象之间的通信；发送⽅叫事件发送器，接收⽅叫事件接收器；

#### 49. 哈希表与字典对比

**字典**：内部用了 Hashtable 作为存储结构

- 如果我们试图找到一个不存在的键，它将返回 / 抛出异常。
- 它比哈希表更快，因为没有装箱和拆箱，尤其是值类型。
- 仅公共静态成员是线程安全的。
- 字典是一种通用类型，这意味着我们可以将其与任何数据类型一起使用（创建时，必须同时指定键和值的数据类型）。
- Dictionay 是 Hashtable 的类型安全实现， Keys 和 Values 是强类型的。
- Dictionary 遍历输出的顺序，就是加入的顺序

**哈希表**：

- 如果我们尝试查找不存在的键，则返回 null。
- 它比字典慢，因为它需要装箱和拆箱。
- 哈希表中的所有成员都是线程安全的，
- 哈希表不是通用类型，
- Hashtable 是松散类型的数据结构，我们可以添加任何类型的键和值。
- HashTable 是经过优化的，访问下标的对象先散列过，所以内部是无序散列的

#### 50. C# 中四种访问修饰符是哪些？各有什么区别？

1. **属性修饰符**
2. **存取修饰符**
3. **类修饰符**
4. **成员修饰符**

**属性修饰符：**
Serializable：按值将对象封送到远程服务器。
STATread：是单线程套间的意思，是⼀种线程模型。
MATAThread：是多线程套间的意思，也是⼀种线程模
型。

**存取修饰符：**
public：存取不受限制。
private：只有包含该成员的类可以存取。
internal：只有当前⼯程可以存取。
protected：只有包含该成员的类以及派⽣类可以存
取。

**类修饰符：**
abstract：抽象类。指示⼀个类只能作为其它类的基
类。
sealed：密封类。指示⼀个类不能被继承。理所当
然，密封类不能同时⼜是抽象类，因为抽象总是希望
被继承的。

**成员修饰符：**
abstract：指示该⽅法或属性没有实现。
sealed：密封⽅法。可以防⽌在派⽣类中对该⽅法的
override（᯿载）。不是类的每个成员⽅法都可以作为
密封⽅法密封⽅法，必须对基类的虚⽅法进⾏᯿载，
提供具体的实现⽅法。所以，在⽅法的声明中，
sealed 修饰符总是和 override 修饰符同时使⽤。
delegate：委托。⽤来定义⼀个函数指针。C# 中的事
件驱动是基于 delegate + event 的。
const：指定该成员的值只读不允许修改。
event：声明⼀个事件。
extern：指示⽅法在外部实现。
override：᯿写。对由基类继承成员的新实现。
readonly：指示⼀个域只能在声明时以及相同类的内
部被赋值。
static：指示⼀个成员属于类型本身，⽽不是属于特定
的对象。即在定义后可不经实例化，就可使⽤。
virtual：指示⼀个⽅法或存取器的实现可以在继承类中
被覆盖。
new：在派⽣类中隐藏指定的基类成员，从⽽实现᯿
写的功能。 若要隐藏继承类的成员，请使⽤相同名称
在派⽣类中声明该成员，并⽤ new 修饰符修饰它。

#### 51. 下列代码在运行中会发生什么问题？如何避免？

```csharp
解释List<int> ls = new List<int>(new int[]{ 1, 2, 3, 4, 5 });
         foreach (int item in ls)
         {
             Console.WriteLine(item * item);
             ls.Remove(item);
         }
```

会产⽣运⾏时错误，因为 foreach 是只读的。不能⼀边遍历⼀边修改。

使用 For 循环遍历可以解决。

#### 52. 什么是装箱拆箱，怎样减少操作

C# 装箱是将值类型转换为引用类型；
拆箱是将引用类型转换为值类型。
牵扯到装箱和拆箱操作比较多的就是在集合中，例如：ArrayList 或者 HashTable 之类。

#### 53. MVC

MVC 全名是 Model View Controller，是模型 (model)－视图 (view)－控制器 (controller) 的缩写，一种软件设计典范。

用一种业务逻辑、数据、界面显示分离的方法，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC 被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。

- Model（模型）是应用程序中用于处理应用程序数据逻辑的部分。
  　　通常模型对象负责在数据库中存取数据。
- View（视图）是应用程序中处理数据显示的部分。
  　　通常视图是依据模型数据创建的。
- Controller（控制器）是应用程序中处理用户交互的部分。
  　　通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据

### 🧡Unity 基础知识

```csharp
微信搜索：呆呆敲代码的小Y
后台回复：白嫖
免费获取很多的编程资料哦！
```

#### 1. Image 和 RawImage 的区别

- Imgae 比 RawImage 更消耗性能
- Image 只能使用 Sprite 属性的图片，但是 RawImage 什么样的都可以使用
- Image 适合放一些有操作的图片，裁剪平铺旋转什么的，针对 Image Type 属性
- RawImage 就放单独展示的图片就可以，性能会比 Image 好很多

#### 2. Unity3D 中的碰撞器和触发器的区别？

答：碰撞器是触发器的载体，而触发器只是碰撞器身上的一个属性。

当 Is Trigger=false 时，碰撞器根据物理引擎引发碰撞，产生碰撞的效果，可以调用 OnCollisionEnter/Stay/Exit 函数；

当 Is Trigger=true 时，碰撞器被物理引擎所忽略，没有碰撞效果，可以调用 OnTriggerEnter/Stay/Exit 函数。

如果既要检测到物体的接触又不想让碰撞检测影响物体移动或要检测一个物件是否经过空间中的某个区域这时就可以用到触发器。

#### 3. 物体发生碰撞的必要条件？

答：两个物体都必须带有碰撞器 Collider，其中一个物体还必须带有 Rigidbody 刚体。

#### 4. 简述四元数 Quaternion 的作用，四元数对欧拉角的优点？

答：四元数用于表示旋转

相对欧拉角的优点：能进行增量旋转、避免万向锁、给定方位的表达方式有两种，互为负（欧拉角有无数种表达方式）

#### 5. 如何安全的在不同工程间安全地迁移 asset 数据？三种方法

1. 将 Assets 和 Library 一起迁移
2. 导出包 package
3. 用 unity 自带的 assets Server 功能

#### 6. OnEnable、Awake、Start 运行时的发生顺序？哪些可能在同一个对象周期中反复的发生？

答：Awake–>OnEnable->Start

OnEnable 在同一周期中可以反复地发生！

#### 7. MeshRender 中 material 和 sharedmaterial 的区别？

答：修改 sharedMaterial 将改变所有物体使用这个材质的外观，并且也改变储存在工程里的材质设置。 不推荐修改由 sharedMaterial 返回的材质。如果你想修改渲染器的材质，使用 material 替代。

#### 8. TCP/IP 协议栈各个层次及分别的功能？

网络接口层：这是协议栈的最低层，对应 OSI 的物理层和数据链路层，主要完成数据帧的实际发送和接收。

网络层：处理分组在网络中的活动，例如路由选择和转发等，这一层主要包括 IP 协议、ARP、ICMP 协议等。

传输层：主要功能是提供应用程序之间的通信，这一层主要是 TCP/UDP 协议。

应用层：用来处理特定的应用，针对不同的应用提供了不同的协议，例如进行文件传输时用到的 FTP 协议，发送 email 用到的 SMTP 等。

#### 9. Unity 提供了几种光源，分别是什么？

四种。

- 平行光：Directional Light
- 点光源：Point Light
- 聚光灯：Spot Light
- 区域光源：Area Light

#### 10. 简述一下对象池，你觉得在 FPS 里哪些东西适合使用对象池？

对象池就存放需要被反复调用资源的一个空间

比如游戏中要常被大量复制的对象，子弹，敌人，以及任何重复出现的对象。

**特点**：用内存换取 cpu 的优化

#### 11. CharacterController 和 Rigidbody 的区别？

Rigidbody 具有完全真实物理的特性，而 CharacterController 可以说是受限的的 Rigidbody，具有一定的物理效果但不是完全真实的。

#### 12. 移动相机动作在哪个函数里，为什么在这个函数里？

**LateUpdate**，是在所有的 Update 结束后才调用，比较适合用于命令脚本的执行。
官网上例子是摄像机的跟随，都是所有的 Update 操作完才进行摄像机的跟进，不然就有可能出现摄像机已经推进了，但是视角里还未有角色的空帧出现。

#### 13. 简述 prefab 的用处

在游戏运行时实例化，prefab 相当于一个模板，对你已经有的素材、脚本、参数做一个默认的配置，以便于以后的修改，同事 prefab 打包的内容简化了导出的操作，便于团队的交流。

#### 14. GPU 的工作原理？

简而言之，GPU 的图形（处理）流水线完成如下的工作：（并不一定是按照如下顺序）。

**顶点处理**：这阶段 GPU 读取描述 3D 图形外观的顶点数据并根据顶点数据确定 3D 图形的形状及位置关系，建立起 3D 图形的骨架。在支持 DX8 和 DX9 规格的 GPU 中，这些工作由硬件实现的 Vertex Shader（定点着色器）完成。

**光栅化计算**：显示器实际显示的图像是由像素组成的，我们需要将上面生成的图形上的点和线通过一定的算法转换到相应的像素点。把一个矢量图形转换为一系列像素点的过程就称为光栅化。例如，一条数学表示的斜线段，最终被转化成阶梯状的连续像素点。

**纹理帖图**：顶点单元生成的多边形只构成了 3D 物体的轮廓，而纹理映射（texture mapping）工作完成对多变形表面的帖图，通俗的说，就是将多边形的表面贴上相应的图片，从而生成 “真实” 的图形。TMU（Texture mapping unit）即是用来完成此项工作。

**像素处理**：这阶段（在对每个像素进行光栅化处理期间）GPU 完成对像素的计算和处理，从而确定每个像素的最终属性。在支持 DX8 和 DX9 规格的 GPU 中，这些工作由硬件实现的 Pixel Shader（像素着色器）完成。

**最终输出**：由 ROP（光栅化引擎）最终完成像素的输出，1 帧渲染完毕后，被送到显存帧缓冲区。

总结：GPU 的工作通俗的来说就是完成 3D 图形的生成，将图形映射到相应的像素点上，对每个像素进行计算确定最终颜色并完成输出。

#### 15. 什么是渲染管道？

是指在显示器上为了显示出图像而经过的一系列必要操作。 渲染管道中的很多步骤，都要将几何物体从一个坐标系中变换到另一个坐标系中去。

主要步骤有： 本地坐标 -> 视图坐标 -> 背面裁剪 -> 光照 -> 裁剪 -> 投影 -> 视图变换 -> 光栅化。

#### 16. 如何优化内存？

1. 压缩自带类库；
2. 将暂时不用的以后还需要使用的物体隐藏起来而不是直接 Destroy 掉；
3. 释放 AssetBundle 占用的资源；
4. 降低模型的片面数，降低模型的骨骼数量，降低贴图的大小；
5. 使用光照贴图，使用多层次细节 (LOD)，使用着色器 (Shader)，使用预设 (Prefab)
6. 代码中少产生临时变量

#### 18. 动态加载资源的方式？

- instantiate：最简单的一种方式，以实例化的方式动态生成一个物体。
- Assetsbundle：即将资源打成 asset bundle 放在服务器或本地磁盘，然后使用 WWW 模块 get 下来，然后从这个 bundle 中 load 某个 object，unity 官方推荐也是绝大多数商业化项目使用的一种方式。
- Resource.Load: 可以直接 load 并返回某个类型的 Object，前提是要把这个资源放在 Resource 命名的文件夹下，Unity 不管有没有场景引用，都会将其全部打入到安装包中
- AssetDatabase.loadasset ：这种方式只在 editor 范围内有效，游戏运行时没有这个函数，它通常是在开发中调试用的。

#### 19. 使用 Unity3d 实现 2d 游戏，有几种方式？

1. 使用本身的 GUI、UGUI
2. 把摄像机的 Projection (投影) 值调为 Orthographic (正交投影)，不考虑 z 轴；
3. 使用 2d 插件，如：2DToolKit、NGUI

#### 20. 在物体发生碰撞的整个过程中，有几个阶段，分别列出对应的函数 三个阶段

答：OnCollisionEnter、 OnCollisionStay、 OnCollisionExit

#### 21. Unity3d 的物理引擎中，有几种施加力的方式，分别描述出来

- rigidbody.AddForce
- rigidbody.AddForceAtPosition

#### 22. 什么叫做链条关节？

Hinge Joint，可以模拟两个物体间用一根链条连接在一起的情况，能保持两个物体在一个固定距离内部相互移动而不产生作用力，但是达到固定距离后就会产生拉力。

#### 23. 物体自身旋转使用的函数？

Transform.Rotate()

#### 24. Unity3d 提供了一个用于保存和读取数据的类 (PlayerPrefs)，请列出保存和读取整形数据的函数

PlayerPrefs.SetInt()、 PlayerPrefs.GetInt()

#### 25. Unity3d 脚本从唤醒到销毁有着一套比较完整的生命周期，请列出系统自带的几个重要的方法。

答：Awake——>Start——>Update——>FixedUpdate——>LateUpdate——>OnGUI——>OnDisable——>OnDestroy

主要执行顺序

编辑器 -> 初始化 -> 物理系统 -> 输入事件 -> 游戏逻辑 -> 场景渲染 ->GUI 渲染 -> 物体激活或禁用 -> 销毁物体 -> 应用结束

主要函数介绍

> - Reset 是在用户点击检视面板的 Reset 按钮或者首次添加该组件时被调用。此函数只在编辑模式下被调用。Reset 最常用于在检视面板中给定一个最常用的默认值。
> - Awake 用于在游戏开始之前初始化变量或游戏状态。在脚本整个生命周期内它仅被调用一次.Awake 在所有对象被初始化之后调用，所以你可以安全的与其他对象对话或用诸如 GameObject.FindWithTag 这样的函数搜索它们。每个游戏物体上的 Awke 以随机的顺序被调用。因此，你应该用 Awake 来设置脚本间的引用，并用 Start 来传递信息，Awake 总是在 Start 之前被调用。它不能用来执行协同程序。
> - OnDisable 不能用于协同程序。当对象变为不可用或非激活状态时此函数被调用。
> - Start 在 behaviour 的生命周期中只被调用一次。它和 Awake 的不同是 Start 只在脚本实例被启用时调用。你可以按需调整延迟初始化代码。Awake 总是在 Start 之前执行。这允许你协调初始化顺序。
> - FixedUpdate 当 MonoBehaviour 启用时，其在每一帧被调用。处理 Rigidbody 时，需要用 FixedUpdate 代替 Update。例如：给刚体加一个作用力时，你必须应用作用力在 FixedUpdate 里的固定帧，而不是 Update 中的帧。(两者帧长不同)。
> - OnTriggerEnter 可以被用作协同程序，在函数中调用 yield 语句。当 Collider (碰撞体) 进入 trigger (触发器) 时调用 OnTriggerEnter。
> - OnCollisionEnter 相对于 OnTriggerEnter，传递的是 Collision 类而不是 Collider。Collision 包含接触点，碰撞速度等细节。如果在函数中不使用碰撞信息，省略 collisionInfo 参数以避免不必要的运算。注意如果碰撞体附加了一个非动力学刚体，只发送碰撞事件。可以被用作协同程序。
>   当鼠标在 GUIElement (GUI 元素) 或 Collider (碰撞体) 上点击时调用 OnMouseDown。
> - Update 是实现各种游戏行为最常用的函数。
> - yield 一个协同程序在执行过程中，可以在任意位置使用 yield 语句。yield 的返回值控制何时恢复协同程序向下执行。协同程序在对象自有帧执行过程中堪称优秀。协同程序在性能上没有更多的开销。StartCoroutine 函数是立刻返回的，但是 yield 可以延迟结果。直到协同程序执行完毕。
> - LateUpdate 是在所有 Update 函数调用后被调用。这可用于调整脚本执行顺序。例如：当物体在 Update 里移动时，跟随物体的相机可以在 LateUpdate 里实现。
>   渲染和处理 GUI 事件时调用。这意味着你的 OnGUI 程序将会在每一帧被调用。要得到更多的 GUI 事件的信息查阅 Event 手册。如果 Monobehaviour 的 enabled 属性设为 false，OnGUI () 将不会被调用。
> - OnApplicationQuit，当用户停止运行模式时在编辑器中调用。当 web 被关闭时在网络播放器中被调用。

#### 26. 物理更新一般放在哪个系统函数里？

**FixedUpdate**，每固定帧绘制时执行一次，和 Update 不同的是 FixedUpdate 是渲染帧执行，如果你的渲染效率低下的时候 FixedUpdate 调用次数就会跟着下降。

FixedUpdate 比较适用于物理引擎的计算，因为是跟每帧渲染有关。 Update 就比较适合做控制。

#### 27. 在场景中放置多个 Camera 并同时处于活动状态会发生什么？

游戏界面可以看到很多摄像机的混合。

#### 28. 如何销毁一个 UnityEngine.Object 及其子类？

使用 Destroy () 方法；

#### 29. 请描述游戏动画有哪几种，以及其原理？

主要有**关节动画**、**骨骼动画**、**单一网格模型动画 (关键帧动画)**。

- 关节动画：把角色分成若干独立部分，一个部分对应一个网格模型，部分的动画连接成一个整体的动画，角色比较灵活，Quake2 中使用这种动画；
- 骨骼动画，广泛应用的动画方式，集成了以上两个方式的优点，骨骼按角色特点组成一定的层次结构，有关节相连，可做相对运动，皮肤作为单一网格蒙在骨骼之外，决定角色的外观；
- 单一网格模型动画由一个完整的网格模型构成，在动画序列的关键帧里记录各个顶点的原位置及其改变量，然后插值运算实现动画效果，角色动画较真实。



#### 30. 请描述为什么 Unity3d 中会发生在组件上出现数据丢失的情况

一般是组件上绑定的物体对象被删除了

#### 31. alpha blend 工作原理？

Alpha Blend 实现透明效果，不过只能针对某块区域进行 alpha 操作，透明度可设。

#### 32. 写出光照计算中的 diffuse 的计算公式？

diffuse = Kd x colorLight x max (N*L,0)；Kd 漫反射系数、colorLight 光的颜色、N 单位法线向量、L 由点指向光源的单位向量、其中 N 与 L 点乘，如果结果小于等于 0，则漫反射为 0。

#### 33. LOD 是什么，优缺点是什么？

**LOD (Level of detail) 多层次细节**，是最常用的游戏优化技术。
它按照模型的位置和重要程度决定物体渲染的资源分配，降低非重要物体的面数和细节度，从而获得高效率的渲染运算。

缺点：增加了内存

LOD 简单示例：[【100 个 Unity 踩坑小知识点】| Unity 的 LOD 技术（多细节层次）](https://xiaoy.blog.csdn.net/article/details/122425837)

#### 33. 两种阴影判断的方法、工作原理？

**本影**和**半影**：

- 本影：景物表面上那些没有被光源直接照射的区域（全黑的轮廓分明的区域）。
- 半影：景物表面上那些被某些特定光源直接照射但并非被所有特定光源直接照射的区域（半明半暗区域） 工作原理：从光源处向物体的所有可见面投射光线，将这些面投影到场景中得到投影面，再将这些投影面与场景中的其他平面求交得出阴影多边形，保存这些阴影多边形信息，然后再按视点位置对场景进行相应处理得到所要求的视图（利用空间换时间，每次只需依据视点位置进行一次阴影计算即可，省去了一次消隐过程）

#### 34. Vertex Shader 是什么，怎么计算？

**顶点着色器** 是一段执行在 GPU 上的程序，用来取代 fixed pipeline 中的 transformation 和 lighting，Vertex Shader 主要操作顶点。

> Vertex Shader 对输入顶点完成了从 local space 到 homogeneous space（齐次空间）的变换过程，homogeneous space 即 projection space 的下一个 space。在这其间共有 world transformation, view transformation 和 projection transformation 及 lighting 几个过程。

#### 35. MipMap 是什么，作用？

**MipMapping**：在三维计算机图形的贴图渲染中有常用的技术，为加快渲染进度和减少图像锯齿，贴图被处理成由一系列被预先计算和优化过的图片组成的文件，这样的贴图被称为 MipMap。

#### 36. 请描述 Interface 与抽象类之间的不同

**语法不同处**：

1. 抽象类中可以有字段，接口没有。
2. 抽象类中可以有实现成员，接口只能包含抽象成员。
3. 抽象类中所有成员修饰符都可以使用，接口中所有的成员都是对外的，所以不需要修饰符修饰。

**用法不同处**：

1. 抽象类是概念的抽象，接口关注于行为。
2. 抽象类的子类与父类的关系是泛化关系，耦合度较高，而实现类和接口之间是实现的关系，耦合度比泛化低。
3. 一个类只能继承一个类，但是可以实现多个接口。

#### 37. .Net 与 Mono 的关系？

mono 是.net 的一个开源跨平台工具，就类似 java 虚拟机，java 本身不是跨平台语言，但运行在虚拟机上就能够实现了跨平台。
.net 只能在 windows 下运行，mono 可以实现跨平台编译运行，可以运行于 Linux，Unix，Mac OS 等。

#### 38. 简述 Unity3D 支持的作为脚本的语言的名称？

Unity 的脚本语言基于 Mono 的.Net 平台上运行，可以使用.NET 库，这也为 XML、数据库、正则表达式等问题提供了很好的解决方案。

Unity 里的脚本都会经过编译，他们的运行速度也很快。这三种语言实际上的功能和运行速度是一样的，区别主要体现在语言特性上。

Unity 支持的语言：C#，JavaScrip (不在使用)

#### 39. Unity3D 是否支持写成多线程程序？如果支持的话需要注意什么？

支持：如果同时你要处理很多事情或者与 Unity 的对象互动小可以用 thread, 否则使用 coroutine。

Unity3d 没有多线程的概念，不过 unity 也给我们提供了 StartCoroutine（协同程序）和 LoadLevelAsync（异步加载关卡）后台加载场景的方法。

注意：仅能从主线程中访问 Unity3D 的组件，对象和 Unity3D 系统调用。C# 中有 lock 这个关键字，以确保只有一个线程可以在特定时间内访问特定的对象

#### 40. 如何让已经存在的 GameObject 在 LoadLevel 后不被卸载掉？

```csharp
DontDestroyOnLoad(transform.gameObject);
```

#### 41. U3D 中用于记录节点空间几何信息的组件名称，及其父类名称

Transform 父类是 Component

#### 42. 向量的点乘、叉乘以及归一化的意义？

- 叉乘 几何意义：得到一个与这两个向量都垂直的向量，这个向量的模是以两个向量为边的平行四边形的面积
- 点乘 几何意义：可以用来表征或计算两个向量之间的夹角，以及在 b 向量在 a 向量方向上的投影
- 点乘描述了两个向量的相似程度，结果越大两向量越相似，还可表示投影
- 叉乘得到的向量垂直于原来的两个向量
- 标准化向量：用在只关系方向，不关心大小的时候

#### 43. 矩阵相乘的意义及注意点？

用于表示线性变换：旋转、缩放、投影、平移、仿射

注意矩阵的蠕变：误差的积累

#### 44. 当一个细小的高速物体撞向另一个较大的物体时，会出现什么情况？如何避免？

穿透（碰撞检测失败）（例如 CS 射击游戏，可以使用开枪时发射射线，射线碰撞到则掉血击中）

#### 45. 为何大家都在移动设备上寻求 U3D 原生 GUI 的替代方案

不美观，OnGUI 很耗费时间，使用不方便

#### 46. 请简述如何在不同分辨率下保持 UI 的一致性

多屏幕分辨率下的 UI 布局一般考虑两个问题：

1. 布局元素的位置，即屏幕分辨率变化的情况下，布局元素的位置可能固定不动，导致布局元素可能超出边界；
2. 布局元素的尺寸，即在屏幕分辨率变化的情况下，布局元素的大小尺寸可能会固定不变，导致布局元素之间出现重叠等功能。

为了解决这两个问题，在 Unity GUI 体系中有两个组件可以来解决问题，分别是布局元素的 Rect Transform 和 Canvas 的 Canvas Scaler 组件。

#### 47. 请简述 OnBecameVisible 及 OnBecameInvisible 的发生时机，以及这一对回调函数的意义？

当物体是否可见切换之时。可以用于只需要在物体可见时才进行的计算。

#### 48. 什么叫动态合批？跟静态合批有什么区别？

如果动态物体共用着相同的材质，那么 Unity 会自动对这些物体进行批处理。
动态批处理操作是自动完成的，并不需要你进行额外的操作。

**区别**：动态批处理一切都是自动的，不需要做任何操作，而且物体是可以移动的，但是限制很多。静态批处理：自由度很高，限制很少，缺点可能会占用更多的内存，而且经过静态批处理后的所有物体都不可以再移动了。

#### 49. Unity 提供了几种光源，分别是什么？

四种。
平⾏光：Directional Light
点光源：Point Light
聚光灯：Spot Light
区域光源：Area Light

#### 50. 什么是 LightMap？

LightMap：就是指在三维软件里实现打好光，然后渲染把场景各表面的光照输出到贴图上，最后又通过引擎贴到场景上，这样就使物体有了光照的感觉。

#### 51. Unity 和 cocos2d 的区别

Unity3D 支持 C#、javascript 等，cocos2d-x 支持 c++、Html5、Lua 等。

cocos2d 开源 并且免费

Unity3D 支持 iOS、Android、Flash、Windows、Mac、Wii 等平台的游戏开发，cocos2d-x 支持 iOS、Android、WP 等。

#### 52. Unity3D Shader 分哪几种，有什么区别？

1. 表面着色器的抽象层次比较高，它可以轻松地以简洁方式实现复杂着色。表面着色器可同时在前向渲染及延迟渲染模式下正常工作。
2. 顶点片段着色器可以非常灵活地实现需要的效果，但是需要编写更多的代码，并且很难与 Unity 的渲染管线完美集成。
3. 固定功能管线着色器可以作为前两种着色器的备用选择，当硬件无法运行那些酷炫 Shader 的时，还可以通过固定功能管线着色器来绘制出一些基本的内容。

#### 53. 获取、增加、删除组件的命令分别是什么？

- 获取：GetComponent
- 增加：AddComponent
- 删除：Destroy

#### 54. Unity 中，照相机的 Clipping Planes 的作用是什么？调整 Near、Far 两个值时，应该注意什么？

剪裁平面 。从相机到开始渲染和停止渲染之间的 距离。

#### 55. 简述 prefab 的用处

在游戏运行时实例化，prefab 相当于一个模板， 对你已经有的素材、脚本、参数做一个默认的配 置，以便于以后的修改，同时 prefab 打包的内容 简化了导出的操作，便于团队的交流。

#### 56. 请描述为什么 Unity3d 中会发生 在组件上出现数据丢失的情况

在 Unity3D 中，组件上出现数据丢失的情况可能是由于多种原因，其中包括：

1. 对象被删除：当组件所绑定的物体对象被删除时，与之关联的数据也会一并丢失。
2. 内存不足：如果设备的内存不足以容纳所有数据，可能会导致数据丢失或损坏。
3. 文件损坏：如果 Unity3D 项目的文件受到损坏，可能会导致组件上的数据出现问题。
4. 未正确保存：如果在编辑器中未正确保存对组件所做的更改，可能会导致数据丢失。
5. 使用了不兼容的插件或脚本：某些第三方插件或脚本可能与 Unity3D 编辑器或游戏引擎不兼容，从而导致数据丢失或组件功能异常。

为了避免在 Unity3D 中出现数据丢失的情况，建议定期备份项目、及时保存更改、确保足够的内存和磁盘空间、以及避免使用未经测试或不信任的第三方插件或脚本。

#### 57. 如何在 Unity3D 中查看场景的面数，顶点数和 Draw Call 数？如何降低 Draw Call 数？

在 Game 视图右上角点击 Stats。降低 Draw Call 的技术是 Draw Call Batching

#### 58. 请问 alpha test 在何时使用？能达到什么效果？

Alpha Test, 中文就是透明度测试。
简而言之就是 V&F shader 中最后 fragment 函数输出的该点颜色值（即上一讲 frag 的输出 half4）的 alpha 值与固定值进行比较。Alpha Test 语句通常于 Pass {} 中的起始位置。Alpha Test 产生的效果也很极端，要么完全透明，即看不到，要么完全不透明。

#### 60. 四元数有什么作用？

对旋转角度进行计算时用到四元数

#### 61. 将 Camera 组件的 ClearFlags 选项选成 Depth only 是什么意思？有何用处？

仅深度，该模式用于对象不被裁剪。

#### 62. 如何让已经存在的 GameObject 在 LoadLevel 后不被卸载掉？

```csharp
解释void Awake()
{
    DontDestroyOnLoad(transform.gameObject);
}
```

#### 63. 在编辑场景时将 GameObject 设置为 Static 有何作用？

设置游戏对象为 Static 将会剔除（或禁用）网格对象当这些部分被静态物体挡住而不可见时。因此，在你的场景中的所有不会动的物体都应该标记为 Static。

#### 64. 有 A 和 B 两组物体，有什么办法能够保证 A 组物体永远比 B 组物体先渲染？

把 A 组物体的渲染对列大于 B 物体的渲染队列

#### 65. 将图片的 TextureType 选项分别选为 Texture 和 Sprite 有什么区别

Sprite 作为 UI 精灵使用，Texture 作用模型贴图使用。

#### 66. 问一个 Terrain，分别贴 3 张，4 张，5 张地表贴图，渲染速度有什么区别？为什么？

答：没有区别，因为不管几张贴图只渲染一次。

#### 67. 什么是 DrawCall？DrawCall 高了又什么影响？如何降低 DrawCall？

Unity 中，每次引擎准备数据并通知 GPU 的过程称为一次 Draw Call。DrawCall 越高对显卡的消耗就越大。降低 DrawCall 的方法：

- Dynamic Batching
- Static Batching
- 高级特性 Shader 降级为统一的低级特性的 Shader。

#### 68. 实时点光源的优缺点是什么？

可以有 cookies – 带有 alpha 通道的立方图 (Cubemap) 纹理。点光源是最耗费资源的。

#### 69. 简述四元数的作用，四元数对欧拉⻆的优点？

四元数⽤于表示旋转，对旋转⻆度进⾏计算时⽤到四元数
相对欧拉⻆的优点：
1）能进⾏增量旋转
2）避免万向锁
3）给定⽅位的表达⽅式有两种，互为负（欧拉⻆有⽆
数种表达⽅式）

#### 70. Addcomponent 后哪个生命周期函数会被调用

对于 AddComponent 添加的脚本，其 Awake，Start，OnEnable 是在 Add 的当前帧被调用的
其中 Awake，OnEnable 与 AddComponent 处于同一调用链上
Start 会在当前帧稍晚一些的时候被调用，Update 则是根据 Add 调用时机决定何时调用：如果 Add 是在当前帧的 Update 前调用，那么新脚本的 Update 也会在当前帧被调用，否则会被延迟到下一帧调用。

#### 72. 层剔除

用 layermask ，通过位运算的方式去设置
在代码中使用时如何开启某个 Layers？

**LayerMask mask = 1 << 你需要开启的 Layers 层。**

**LayerMask mask = 0 << 你需要关闭的 Layers 层。**

举几个例子：

```csharp
解释LayerMask mask = 1 << 2; 表示开启Layer2。

LayerMask mask = 0 << 5;表示关闭Layer5。

LayerMask mask = 1<<2|1<<8;表示开启Layer2和Layer8。

LayerMask mask = 0<<3|0<<7;表示关闭Layer3和Layer7。
```

#### 73. 分别解释顶点着色器和像素着色器是什么

顶点着⾊器是⼀段执⾏在 GPU 上的程序，⽤来取代 fixed pipeline 中的 transformation 和 lighting，Vertex Shader 主要操作顶点。‘’

像素着色器实际上就是对每一个像素进行光栅化的处理期间，在 GPU 上运算的一段程序。

不同与顶点着色器，像素着色器不会以软件的形式来模拟像素着色器。

像素着色器实质上是取代了固定功能流水线中多重纹理的环节，而且赋予了我们访问单个像素以及访问每一个像素纹理坐标的能力

#### 74. 画布的三种模式。缩放模式

- 屏幕空间 - 覆盖模式 (Screen Space-Overlay)

  ，Canvas 创建出来后，默认就是该模式，该模式和摄像机无关，即使场景内没有摄像机，UI 游戏物体照样渲染

  - 屏幕空间：电脑或者手机显示屏的 2D 空间，只有 x 轴和 y 轴
  - 覆盖模式：UI 元素永远在 3D 元素的前面

- **屏幕空间 - 摄像机模式 (Screen Space-Camera)**，设置成该模式后需要指定一个摄像机游戏物体，指定后 UGUI 就会自动出现在该摄像机的 “投射范围” 内，和 NGUI 的默认 UI Root 效果一致，如果隐藏掉摄像机，UGUI 当然就无法渲染

- **世界空间模式 (WorldSpace)**，设置成该模式后 UGUI 就相当于是场景内的一个普通的 “Cube 游戏模型”，可以在场景内任意的移动 UGUI 元素的位置，通常用于怪物血条显示和 VR 开发

缩放模式：

| Property:                                                    | Function:                                      |
| :----------------------------------------------------------- | :--------------------------------------------- |
| UI Scale Mode                                                | Canvas 中 UI 元素的缩放模式                    |
| Constant Pixel Size                                          | 使 UI 保持自己的尺寸，与屏幕尺寸无关。         |
| Scale With Screen Size                                       | 屏幕尺寸越大，UI 越大                          |
| Constant Physical Size                                       | 使 UI 元素保持相同的物理大小，与屏幕尺寸无关。 |
| Constant Pixel Size、Constant Physical Size 实际上他们本质是一样的，只不过 Constant Pixel Size 通过逻辑像素大小调节来维持缩放，而 Constant Physical Size 通过物理大小调节来维持缩放。 |                                                |

#### 75. FSM 有限状态机

FSM 是一种数据结构，它由以下几个部分组成：

1. 内在的所有状态（必须是有限个）
2. 输入条件
3. 状态之间起到连接性作用的转换函数

为什么要用 FSM？

因为它编程快速简单，易于调试，性能高，与人类思维相似从而便于梳理，灵活且容易修改

FSM 的描述性定义：
一个有限状态机是一个设备，或是一个模型，具有有限数量的状态。它可以在任何给定时间根据输入进行操作，使得系统从一个状态转换到另一个状态，或者是使一个输出或者一种行为的发生，一个有限状态机在任何瞬间只能处于一种状态。

State 状态基类，定义了基本的 Enter，Update，Exit 三种状态行为，通常在这三种状态行为的方法里会写一些逻辑。每个 State 都会有 StateID（状态 id，可以是枚举等），FSMControl（控制该状态的状态控制器的引用），Check 方法（用来进行状态判断，并返回 StateID，通过 FSMControl 驱动）

FSMControl，包含了一下 FSMMachine，封装层。

FSMMachine，驱动它的 State 列表，Update 方法调用当前 State 的 Check 方法来获得 StateID，当 currentState 的 Check 方法返回的 StateID 和当前 StateID 不同，则切换状态。

这是一个简单的 FSM 状态机系统，根据需要自己写个 Control 继承 FSMControl 来驱动状态。因为 Check 是 State 的职责，所以每一个不同对象的行为如 Human 的 Idle 和 Dog 的 Idel 区分肯定也不同。因此需要分别去写 HumanIdleState 和 DogIdleState。如果还有 Cat，Fish，可想而知代码量会有多么庞大。

因此我将 FSMControl 抽象为一个公共基类，把 State 的 Check 具体实现作为 FSMControl 的 Virtual 方法。这样在 IdleState 里的 Check 方法就不用写具体的状态切换判断逻辑，而是调用它 FSMControl 子类（自己写的继承自 FSMControl 的 Control 类）的重写方法

这样每次添加的新对象只要有 Idle 这个状态，就可以用一个公用的 StateIdle，状态切换的逻辑差异放在 Control 层

#### 76. 使用过哪些 Unity 插件

因人而异，可以去简单了解一下要说的插件，没用过也可以，至少你知道这个插件了！

| 插件名                                   | 作用                 |
| :--------------------------------------- | :------------------- |
| shader graph                             | 制作 shader 光影效果 |
| cinemachine+timeline+postprocessingstack | 制作过场动画         |
| nodecanvas                               | 制作怪物 ai          |
| easytouch                                | 手游触摸控制         |
| DoTween                                  | 动画插件             |
| Fungus                                   | 对话插件             |
| 3D WebView                               | 浏览器插件           |
| Vectrosity                               | 划线插件             |
| AVPro Video                              | 视频播放插件         |

### 💛物理系统

#### 1. CharacterController 和 Rigidbody 的区别

Rigidbody 具有完全真实物理的特性，⽽ CharacterController 可以说是受限的
Rigidbody，具有⼀定的物理效果但不是完全真实的。

#### 2. 射线检测碰撞物的原理是？

答：射线是 3D 世界中一个点向一个方向发射的一条无终点的线，在发射轨迹中与其他物体发生碰撞时，它将停止发射 。

#### 3. 什么叫做链条关节？

Hinge Joint，可以模拟两个物体间用一根链条连 接在一起的情况，能保持两个物体在一个固定距 离内部相互移动而不产生作用力，但是达到固定 距离后就会产生拉力。

#### 4. 物体发生碰撞的必要条件？

两个物体都必须带有碰撞器 Collider，其中一个物体还必须带有 Rigidbody 刚体

#### 5. 在物体发生碰撞的整个过程 中，有几个阶段，分别列出对 应的函数 三个阶段

1. OnCollisionEnter
2. OnCollisionStay
3. OnCollis ionExit

#### 6. Unity3d 中的碰撞器和触发器的 区别？

碰撞器是触发器的载体，而触发器只是碰撞器身 上的一个属性。
当 Is Trigger=false 时，碰撞器根据物理引擎引发 碰撞，产生碰撞的效果，可以调用 OnCollisionEnter/Stay/Exit 函数；
当 Is Trigger=true 时，碰撞器被物理引擎所忽略， 没有碰撞效果，可以调用 OnTriggerEnter/Stay/ Exit 函数。
如果既要检测到物体的接触又不想让碰撞检测影 响物体移动或要检测一个物件是否经过空间中的 某个区域这时就可以用到触发器

#### 7. 射线检测碰撞物的原理是？

射线是 3D 世界中一个点向一个方向发射的一条无 终点的线，在发射轨迹中与其他物体发生碰撞 时，它将停止发射 。

#### 8. Unity3d 的物理引擎中，有几种 施加力的方式，分别描述出来

rigidbody.AddForce/AddForceAtPosition，都在 rigidbody 系列函数中。

#### 9. 当一个细小的高速物体撞向另一个较大的物体时，会出现什么情况？如何避免？

穿透 (碰撞检测失败)

#### 10. 射线 Raycast 原理

从一个起点向一个方向发射一条物理射线，返回碰撞到的物体的碰撞信息

------

### 💚UI & 2D 部分

#### 1. UGUI 合批的一些问题

简单来说在一个 Canvas 下，需要相同的 material，相同的纹理以及相同的 Z 值。例如 UI 上的字体 Texture 使用的是字体的图集，往往和我们自己的 UI 图集不一样，因此无法合批。还有 UI 的动态更新会影响网格的重绘，因此需要动静分离。

#### 2. Image 和 RawImage 的区别

- Imgae 比 RawImage 更消耗性能
- Image 只能使用 Sprite 属性的图片，但是 RawImage 什么样的都可以使用
- Image 适合放一些有操作的图片，裁剪平铺旋转什么的，针对 Image Type 属性
- RawImage 就放单独展示的图片就可以，性能会比 Image 好很多

#### 3. 使用 Unity3d 实现 2d 游戏，有几种方式？

1. 使用本身 UGUI，UGUI 是 duUnity 官方推出 zhi 的最新 UI 系统，UI 就是 UserInterface。
2. 把摄像机的投影改为正交投影，不考虑 Z 轴.
3. 使用 Untiy 自身的 2D 模式，在 2d 模式中，层级视图中只有一个正交摄像机，场景视图选择的是 2D 模式。
4. 使用 2D TooKit 插件，2D Toolkit 是一组与 Unity 环境无缝集成的工具，提供高效的 2D 精灵和文本系统。

#### 4. 将图片的 TextureType 选项分别选为 Texture 和 Sprite 有什么区别

Sprite 作为 UI 精灵使用，Texture 作用模型贴图使用。

#### 5. 请简述如何在不同分辨率下保 持 UI 的一致性

屏幕分辨率的自适应 性，原理就是计算出屏幕的宽高比跟原来的预设 的屏幕分辨率求出一个对比值，然后修改摄像机 的 size。

------

### 💙动画系统

#### 1. 请描述游戏动画有哪几种，以及其原理？

主要有关节动画、⻣骼动画、单一网格模型动画 (关键 帧动画)。

- 关节动画：把⻆色分成若干独立部分，一个 部分对应一个网格模型，部分的动画连接成一个整体 的动画，⻆色比较灵活，Quake2 中使用这种动画；
- ⻣骼动画，广泛应用的动画方式，集成了以上两个方 式的优点，⻣骼按⻆色特点组成一定的层次结构，有 关节相连，可做相对运动，皮肤作为单一网格蒙在⻣ 骼之外，决定⻆色的外观；
- 单一网格模型动画由一个完整的网格模型构成，在动 画序列的关键帧里记录各个顶点的原位置及其改变量，然后插值运算实现动画效果，⻆色动画较真实。

#### 2. Avator 的作用

用户提供的模型骨架和 Unity 的骨架结构进行适配，是一种骨架映射关系。
方便动画的重定向

AnimationType 有三种类型
Humanoid 人型：可以动画重定向，游戏对象挂载 animator，子类原始模型 + 重定向模型，设置原始模型和使用模型的 AnimationType 为 Humanoid 类型
Generic 非人型
Legacy 旧版
Avator Mask 身体遮罩，身体某一部分是否受到动画影响
反向动力学 IK，通过手或脚来控制身体其他部分

#### 3. 反向旋转动画的方法是什么？

1. 将动画速度调成 - 1
2. 改代码 animation.speed=-1

#### 4. Animation.CrossFade 是什么？

动画淡入淡出

#### 5. 写出 Animation 的五个方法

- AddClip 将 clip 添加到名称为 newName 的动画中。
- Blend 在后续 time 秒中将名称为 animation 的动画向 targetWeight 混合。
- CrossFade 在后续 time 秒的时间段内，使名称为 animation 的动画淡入，使其他动画淡出。
- CrossFadeQueued 使动画在上一个动画播放完成后交叉淡入淡出。
- IsPlaying 名称为 name 的动画是否正在播放？
- PlayQueued 在先前的动画播放完毕后再播放动画。
- RemoveClip 从动画列表中移除剪辑。
- Sample 对当前状态的动画进行采样。
- Stop 停止所有使用该动画启动的正在播放的动画。

#### 6. 简述 SkinnedMesh 的实现原理

SkinnedMesh 蒙皮网格动画
分为骨骼和蒙皮两部分
骨骼是一个层次结构，存储了骨骼的 Transform 数据
蒙皮是 mesh 顶点附着在骨骼之上，顶点可以被多个骨骼影响，决定了其权重等，
还有将顶点从 Mesh 空间变换到骨骼空间～

#### 7. 动画层 (Animation Layers) 的作用是什么？

**动画分层**

身体部位动画分层，比如我只想动动头，身体其他部分不发生动画，可以方便处理动画区分

------

### 💜协程

#### 1. Unity 协程 Coroutine 的作用

协程 Coroutine 在 Unity 中一直扮演者重要的角色。
可以实现简单的计时器、将耗时的操作拆分成几个步骤分散在每一帧去运行等等，而尽量不阻塞主线程运行。

#### 2. 什么是协同程序？

在主线程运行时同时开启另一段逻辑处理，来协助当前程序的执行。
换句话说，开启协程就是开启一个线程。可以用来控制运动、序列以及对象的行为。

#### 3. Unity3D 的协程和 C# 线程 之间的区别是什么？

多线程程序同时运行多个线程 ，而在任一指定时刻只 有一个协程在运行，并且这个正在运行的协同程序只 在必要时才被挂起。
除主线程之外的线程无法访问 Unity3D 的对象、组件、 方法。
Unity3d 没有多线程的概念，不过 unity 也给我们提供了 StartCoroutine (协同程序) 和 LoadLevelAsync (异步 加载关卡) 后台加载场景的方法。
StartCoroutine 为什 么叫协同程序呢，所谓协同，就是当你在 StartCoroutine 的函数体里处理一段代码时，利用 yield 语句等待执行结果，这期间不影响主程序的继续执 行，可以协同工作。

#### 4. 协同程序的执行代码是什么？有何用处，有何缺点？

```csharp
解释// 官方案例)
function Start() {
    // - After 0 seconds, prints "Starting 0.0"
    // - After 0 seconds, prints "Before WaitAndPrint
    Finishes 0.0"
    // - After 2 seconds, prints "WaitAndPrint 2.0" // 先打印"Starting 0.0"和"Before WaitAndPrint
    Finishes 0.0"两句,2秒后打印"WaitAndPrint 2.0" print ("Starting " + Time.time );
    // Start function WaitAndPrint as a coroutine. And continue execution while it is running
    // this is the same as WaintAndPrint(2.0) as the compiler does it for you automatically
    // 协同程序WaitAndPrint在Start函数内执行,可以视 同于它与Start函数同步执行.
    StartCoroutine(WaitAndPrint(2.0));
    print ("Before WaitAndPrint Finishes " + Time.time );
}

function WaitAndPrint (waitTime : float) {
    // suspend execution for waitTime seconds // 暂停执行waitTime秒
    yield WaitForSeconds (waitTime);
    print ("WaitAndPrint "+ Time.time );
}
```

**作用**：一个协同程序在执行过程中，可以在任意位置使
用 yield 语句。yield 的返回值控制何时恢复协同程序向 下执行。协同程序在对象自有帧执行过程中堪称优 秀。协同程序在性能上没有更多的开销。 缺点：协同程序并非真线程，可能会发生堵塞。

更多协程内容：[Unity 零基础到入门 ☀️| 小万字教程 对 Unity 中的 协程 ❤️全面解析 + 实战演练❤️](https://xiaoy.blog.csdn.net/article/details/119830269)

### 🤎数据持久化 & 资源管理

#### 1. unity 常用资源路径有哪些

```csharp
解释//获取的目录路径最后不包含  /
//获得的文件路径开头包含 /
Application.dataPath; //Asset文件夹的绝对路径
//只读
Application.streamingAssetsPath;  //StreamingAssets文件夹的绝对路径（要先判断是否存在这个文件夹路径）
Application.persistentData ; //可读写

//资源数据库 (AssetDatabase) 是允许您访问工程中的资源的 API
AssetDatabase.GetAllAssetPaths; //获取所有的资源文件（不包含meta文件）
AssetDatabase.GetAssetPath(object) //获取object对象的相对路径
AssetDatabase.Refresh(); //刷新
AssetDatabase.GetDependencies(string); //获取依赖项文件


Directory.Delete(p, true); //删除P路径目录
Directory.Exists(p);  //是否存在P路径目录
Directory.CreateDirectory(p); //创建P路径目录

AssetDatabase //类库，对Asset文件夹下的文件进行操作，获取相对路径，获取所有文件，获取相对依赖项
Directory //类库，相关文件夹路径目录进行操作，是否存在，创建目录，删除等操作
```

#### 2. 如何安全的在不同工程间安全 地迁移 asset 数据？三种方法

1. 将 Assets 目录和 Library 目录一起迁移
2. 导出包
3. 用 unity 自带的 assets Server 功能

#### 3. unity 提供了一个用于保存读取数据的类，（playerPrefs），请列出保存读取整形数据的函数

**PlayerPrefs 类**是一个本地持久化保存与读取数据的类
PlayerPrefs 类支持 3 中数据类型的保存和读取，浮点型，整形，和字符串型。

分别对应的函数为：

SetInt (); 保存整型数据；GetInt (); 读取整形数据；
SetFloat (); 保存浮点型数据； GetFlost (); 读取浮点型数据；
SetString (); 保存字符串型数据； GetString (); 读取字符串型数据；

#### 4. 动态加载资源的方式？

- instantiate：最简单的一种方式，以实例化的方式动态生成一个物体。
- Assetsbundle：即将资源打成 asset bundle 放在服务器或本地磁盘，然后使用 WWW 模块 get 下来，然后从这个 bundle 中 load 某个 object，unity 官方推荐也是绝大多数商业化项目使用的一种方式。
- Resource.Load: 可以直接 load 并返回某个类型的 Object，前提是要把这个资源放在 Resource 命名的文件夹下，Unity 不管有没有场景引用，都会将其全部打入到安装包中
- AssetDatabase.loadasset ：这种方式只在 editor 范围内有效，游戏运行时没有这个函数，它通常是在开发中调试用的。

#### 5. AssetsBundle 打包

```csharp
解释using UnityEditor;
using System.IO;

public class CreateAssetBundles //进行AssetBundle打包
{

    [MenuItem("Assets/Build AssetBundles")]
    static void BuildAllAssetBundles()
 {
    string dir = "AssetBundles";
    if (Directory.Exists(dir) == false)
    {
      Directory.CreateDirectory(dir);
    }
    
    BuildPipeline.BuildAssetBundles(dir, //路径必须创建
    BuildAssetBundleOptions.ChunkBasedCompression, //压缩类型***
    BuildTarget.StandaloneWindows64);//平台***
 }
}
```

| None                    | Build assetBundle without any special option.（LAMA 压缩，压缩率高，解压久） |
| :---------------------- | :----------------------------------------------------------- |
| UncompressedAssetBundle | Don’t compress the data when creating the asset bundle.（不压缩，解压快） |
| ChunkBasedCompression   | Use chunk-based LZ4 compression when creating the AssetBundle. |

（压缩率比 LZMA 低，解压速度接近无压缩）|

#### 6. AssetBundle 加载

1. LoadFromMemory(LoadFromMemoryAsync)
2. LoadFromFile（LoadFromFileAsync）
3. UnityWebRequest
4. LoadAssetsByWWW(LoadFromCacheOrDownload)

**第一种**

```csharp
解释IEnumerator Start()
{
  string path = "AssetBundles/wall.unity3d";

  AssetBundleCreateRequest request =AssetBundle.LoadFromMemoryAsync(File.ReadAllBytes(path));

  yield return request;
  
  AssetBundle ab = request.assetBundle;
  
  GameObject wallPrefab = ab.LoadAsset<GameObject>("Cube");
  
  Instantiate(wallPrefab);
}
```

**第二种**

```csharp
解释IEnumerator Start()
{
string path = "AssetBundles/wall.unity3d";

  AssetBundleCreateRequest request = AssetBundle.LoadFromFileAsync(path);

  yield return request;

  AssetBundle ab = request.assetBundle;

  GameObject wallPrefab = ab.LoadAsset<GameObject>("Cube");

  Instantiate(wallPrefab);
}
```

**第三种**

```csharp
解释IEnumerator Start()
{

  string uri = @"http://localhost/AssetBundles/cubewall.unity3d";

  UnityWebRequest request =   UnityWebRequest.GetAssetBundle(uri);

  yield return request.Send();

  AssetBundle ab = DownloadHandlerAssetBundle.GetContent(request);

  GameObject wallPrefab = ab.LoadAsset<GameObject>("Cube");

  Instantiate(wallPrefab);
}
```

**第四种 WWW（无依赖）**

```csharp
解释private IEnumerator LoadNoDepandenceAsset()
    {
        string path = "";
        
        if (loadLocal)
        {
#if UNITY_EDITOR_WIN || UNITY_STANDALONE_WIN
            path += "File:///";
#endif
#if UNITY_EDITOR_OSX || UNITY_STANDALONE_OSX
            path += "File://";
#endif
            path += assetBundlePath + "/" + assetBundleName;
            
            //www对象
            WWW www = new WWW(path);
 
            //等待下载【到内存】
            yield return www;
 
            //获取到AssetBundle
            AssetBundle bundle = www.assetBundle;
 
            //加载资源
            GameObject prefab = bundle.LoadAsset<GameObject>(assetRealName);
 
            //Test:实例化
            Instantiate(prefab);
        }
```

**第四种 WWW（有依赖）**

```csharp
解释using System.Collections;
using System.IO;
using UnityEngine;
using UnityEngine.Networking;
 
public class LoadAssetsDemo : MonoBehaviour
{
    [Header("版本号")]
    public int version = 1;
    
    [Header("加载本地资源")]
    public bool loadLocal = true;
    [Header("资源的bundle名称")]
    public string assetBundleName;
    [Header("资源的真正的文件名称")]
    public string assetRealName;
    
    //bundle所在的路径
    private string assetBundlePath;
    //bundle所在的文件夹名称
    private string assetBundleRootName;
 
    private void Awake()
    {
        assetBundlePath = Application.dataPath + "/OutputAssetBundle";
        assetBundleRootName = assetBundlePath.Substring(assetBundlePath.LastIndexOf("/") + 1);
        
        Debug.Log(assetBundleRootName);
    }
 
IEnumerator LoadAssetsByWWW()
 
{
   string path="";
//判断是不是本地加载
   if(loadLocal)// loadLocal=true为本地资源
   {
#if UNITY_EDITOR_WIN || UNITY_STANDALONE_WIN
        path+="File:///";
#endif
#if UNITY_EDITOR_OSX || UNITY_STANDALONE_OSX
        path+="File://";
#edif
   }
    //获取要加载的资源路径【bundle的总说明文件】
   path+=assetBundle+"/"+assetBundleRootName;
   //加载
   WWW www=WWW.LoadFromCacheOrDownload(path,version);
   yield return www;
   //拿到其中的bundle
   AssetBundle manifestBundle=www.assetsBundle;
   //获取到说明文件
   AssetBundleManifest manifest=manifest.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
   //获取资源的所有依赖
   string[] dependencies=manifest.GetAllDependencies(assetBundleName);
   //卸载Bubdle和解压出来的manifest对象
   manifestBundle.Unload(true);
   //获取到相对路径
   path =path.Remove(path.LastIndexOf("/")+1);
   //声明依赖的Bundle数组
   AssetBundle[] depAssetBundle=new AssetBundle[dependencies.Length];
   
   //遍历加载所有的依赖
   for(int i=0;i<dependencies.Length;i++)
{  //获取到依赖Bundle的路径
   string depPath=path+ dependencies[i];
   //获取新的路径进行加载
   www=WWW.LoadFromCacheOrDownload(depPath,version);
   yield return www;
   //将依赖临时保存
   depAssetBundles[i]=www.assetsBundle;
 
}
   //获取路径
   path+=assBundleName;
   //加载最终资源
   www=WWW.LoadFromCacheOrDownload(path,version);
   //等待下载
   yield return www;
   //获取到真正的AssetBundle
   AssetBundle realAssetBundle=www.assBunle;
   //加载真正的资源
   GameObject prefab=realAssetBundle.LoadAsset<GameObject>(assetBundle);
   //生成
   Instantiate(prefab);
 
   //卸载依赖
   for(int i-0;i<depAssetBundle.Length;i++)
{
   depAssetBundle[i].Unload(true);
}
   realAssetBundle.Unload(true);
}
 
}
```

#### 7. AssetBundle 卸载流程

AssetBundle.Unload(bool)，T
true 卸载所有资源

false 只卸载没使用的资源，而正在使用的资源与 AssetBundle 依赖关系会丢失，调用 Resources.UnloadUnusedAssets 可以卸载。

或者等场景切换的时候自动调用 Resources.UnloadUnusedAssets。

------

### 🖤Lua 语言和 Xlua 热更

#### 1. Lua 如何调用 C

三种方式
第一种：官方不推荐
第二种：如果 Resource 文件下的 Lua 文件，使用 Lua 的 Require 函数即可
第三种：如果 Lua 文件是下载的，使用自定义 Loader 可满足

#### 2. 资源如何打包？依赖项列表如何生成？

1. 查找指定文件夹 ABResource 里的资源文件
   - Directory.GetFile (资源路径)
   - 新建 AssetBundleBuild 对象
   - 获取资源名称，并赋值对应 AB 名称
   - 获取各个资源的依赖项：通过 UnityEditor.AssetDataBase 类获取各个资源的依赖项
2. 使用 Unity 自带的 BuildPipeline 进行构建 AB 包
   - BuildPipeLine.BuildAssetBundles (输出 AB 包路径)
   - File.WriteAllLines (将依赖项写入文件里)

#### 3. 如何解析版本文件？如何加载 AB 包资源？具体流程是怎么样的？

1. 解析版本文件列表
   - File.ReadAllLines (读取文件列表资源路径 URL)
   - 获取资源名称，获取 AB 包名称，获取依赖项，字典容器存储
   - 获取 Lua 文件
2. 加载资源
   - 异步加载资源 AB 包，AssetBundleRequest 请求，AssetBundle.LoadFromFileAsync
   - 先检查依赖项，再异步加载 AB 包依赖项
   - 加载成功后都有对应的回调方法，将资源作为参数传入

#### 4. 热更新方案有哪些？以及具体热更流程

1. 整包：存放在上 SteamingAssets 里
   —— 策略：完整更新资源放在包里
   —— 优点：首次更新少
   —— 缺点：安装包下载时间长，首次安装久
2. 分包
   —— 策略：少部分资源放在包里，大部分更新资源存放在更新资源器中
   —— 优点：安装包小，安装时间短，下载快
   —— 缺点：首次更新下载解压缩包时间旧
3. 适用性
   —— 海外游戏大部分是使用分包策略，平台规定
   —— 国内游戏大部分是使用整包策略
4. 文件可读写路径
   ——Application.streamingAssestsPath 只读目录
   ——Application.persistentDatapath 可读写目录
   —— 资源服务器地址 URL
5. 【从资源服务器】下载单个文件或多个文件
   ——NetWorking.UnityWebRequest 获取 URL , HTTP GET , 连接资源服务器
   —— 获取到 downloadHander 的文件数据 Data，完成后会回调方法，将文件 Data 作为参数传出
6. 检查是否初次安装

#### 5. 简述 Lua 实现面向对象的原理

1. 表 table 就是一个对象，对象具有了标识 self，状态等相关操作
2. 使用参数 self 表示方法的该接受者是对象本身，是面向对象的核心点，冒号操作符可以隐藏该 self 参数
3. 类（Class）：每个对象都有一个原型，原型 (lua 类体系) 可以组织多个对象间共享行为
4. setmetatable (A,{__index=B}) 把 B 设为 A 的原型
5. 继承（Inheritance）：Lua 中类也是对象，可以从其他类（对象）中获取方法和没有的字段
6. 继承特性：可以重新定义（修改实现）在基类继承的任意方法
7. 多重继承：一个函数 function 用作__Index 元方法，实现多重继承，还需要对父类列表进行查找方法，但多继承复杂性，性能不如单继承，优化，将继承的方法赋值到子类当中
8. 私有性（很少用）基本思想：两个表表示一个对象，第一个表保存对象的状态在方法的闭包中，第二个表用来保存对象的操作（或接口），用来访问对象本身。使第一个表完成内容私有性。

#### 6. 简述 Lua 有哪 8 个类型？简述用途

- nil 空 —— 可以表示无效值，全局变量（默认赋值为 nil），赋值 nil ，使其被删除
- number 整数
- table 表 ——
- string 字符
- userdata 自定义
- function 函数
- bool 布尔
- thread 线程

#### 7.lua 中 pairs 和 ipairs 的区别，从 table 内存分布方面来考虑

在 Lua 中，`pairs` 和 `ipairs` 是两个用于遍历表（table）的内置函数。从内存分布的角度来看，它们的主要区别在于如何处理表中的数字键。

1. **pairs**:
   - `pairs` 遍历表中的所有键，包括字符串键和数字键。
   - 当数字键作为迭代器返回时，它们会被转换为字符串形式。例如，如果表中有数字键 `1`, `2`, `3`，使用 `pairs` 遍历时，它们会被返回为字符串键 `"1"`, `"2"`, `"3"`。
   - 因此，使用 `pairs` 遍历表时，字符串和数字键都会被考虑，并且数字键会被转换为字符串。
2. **ipairs**:
   - `ipairs` 专门用于遍历表的数字键（从 1 到 n 的整数）。
   - 当使用 `ipairs` 遍历表时，只有数字键会被考虑，并且这些数字键不会被转换为字符串。
   - 如果表中有非数字的键或没有数字键，`ipairs` 将不会返回任何值。

**内存分布方面的考虑**：

- 在遍历表的字符串键时，使用 `pairs` 和 `ipairs` 的内存消耗是相似的，因为它们都需要存储这些键的值。
- 但是，对于数字键，由于 `ipairs` 只考虑数字键，所以它通常会使用更少的内存。特别是当表中有大量数字键时，使用 `ipairs` 可以节省内存。
- 另一方面，如果表中有大量的非数字键或混合类型的键（字符串和数字），使用 `pairs` 可能更为合适，因为它可以处理所有类型的键。

总结：从内存分布的角度来看，选择 `pairs` 或 `ipairs` 取决于你的具体需求。如果你主要关心数字键并且确信表中只有数字键，那么使用 `ipairs` 可以节省内存。如果你需要处理所有类型的键或不确定表中是否只有数字键，那么使用 `pairs` 可能更为合适。

### 🤍网络

#### 1. 客户端与服务器交互方式有几种？

1. socket 通常也称作 "套接字", 实现服务器和客户端之间的物理连接，并进行数据传输，主要有 UDP 和 TCP 两个协议。Socket 处于网络协议的传输层。
2. 协议传输的主要有 http 协议 和基于 http 协议的 Soap 协议（web service）, 常见的方式是 http 的 post 和 get 请求，web 服务。



#### 2. 概述序列化

**序列化** 简单理解成把对象转换为容易传输的格式的过程。
⽐如，可以序列化⼀个对象，然后使⽤ HTTP 通过 Internet 在客户端和服务器端之间传输该对象

#### 3. UDP/TCP 含义，区别

UDP 协议全称是用户数据报协议

1. 面向无连接
2. 面向报文，
3. 不可靠性，
4. 有单播，多播，广播的功能
5. 头部开销小，传输数据报文时是很高效的。

TCP 协议全称是传输控制协议是一种面向连接的、可靠的、基于字节流的传输层通信协议。三次握手、四次挥手

TCP：

1. 面向连接
2. 仅支持单播传输
3. 面向字节流
4. 可靠传输
5. 提供拥塞控制
6. TCP 提供全双工通信

#### 4. TCP/IP 协议栈各个层次及分别的功能？

网络接口层：这是协议栈的最低层，对应 OSI 的物理层和数据链路层，主要完成数据帧的实际发送和接收。

网络层：处理分组在网络中的活动，例如路由选择和转发等，这一层主要包括 IP 协议、ARP、ICMP 协议等。

传输层：主要功能是提供应用程序之间的通信，这一层主要是 TCP/UDP 协议。

应用层：用来处理特定的应用，针对不同的应用提供了不同的协议，例如进行文件传输时用到的 FTP 协议，发送 email 用到的 SMTP 等。

#### 5. 写出 WWW 的几个方法

- WWW.LoadFromCacheOrDownload：可被用于将 Assets Bundles 自动缓存到本地磁盘
- WWW.Dispose ：释放现有的 WWW 对象。
- WWW.isDone：是否完成下载？（只读）
- WWW.progress：下载进度（只读）。

#### 6. Socket 粘包

**什么是粘包？**
答：顾名思义，其实就是多个独立的数据包连到一块儿。

**什么情况下需要考虑粘包？**
答：实际情况如下：

1. 如果利用 tcp 每次发送数据，就与对方建立连接，然后双方发送完一段数据后，就关闭连接，这样就不会出现粘包问题。
2. 如果发送的数据无结构，比如文件传输，这样发送方只管发送，接收方只管接收存储就 ok，也不用考虑粘包。
3. 如果双方建立连接，需要在连接后一段时间内发送不同结构数据，如连接后，有好几种结构：
   1)”good good study”
   2)”day day up”
   那这样的话，如果发送方连续发送这个两个包出去，接收方一次接收可能会是”good good studyday day up” 这样接收方就傻了，因为协议没有规定这么奇怪的字符串，所以要把它分包处理，至于怎么分也需要双方组织一个比较好的包结构，所以一般可能会在头加一个数据长度之类的包，以确保接收。

所以说：Tcp 连续发送消息的时候，会出现消息一起发送过来的问题，这时候需要考虑粘包的问题。

**粘包出现的原因** (在流传输中，UDP 不会出现粘包，因为它有消息边界。)

1. 发送端需要等缓冲区满才发送出去，造成粘包 (发送端出现粘包)
2. 接收端没有及时接收缓冲区包数据，造成一次性接收多个包，出现粘包 (接收端出现粘包)

**解决粘包**

1. 缓冲区过大造成了粘包，所以在发送 / 接收消息时先将消息的长度作为消息的一部分发出去，这样接收方就可以根据接收到的消息长度来动态定义缓冲区的大小。（这种方法就是所谓的自定义协议，这种方法是最常用的）
2. 对发送的数据进行处理，每条消息的首尾加上特殊字符，然后再把要发送的所有消息放入一个字符串中，最后将这个字符串发送出去，接收方接收到这个字符串之后，再通过特殊标记操作字符串，把每条消息截出来。(这种方法只适合数据量较小的情况)

注：要记住这一点：TCP 对上层来说是一个流协议，所谓流，就是没有界限的一串数据。大家可以想想河里的流水，是连成一片的，其间是没有分界线的，也就是没有包的概念。所以我们必须自己定义包长或者分隔符来区分每一条消息。

#### 7. Socket 的封包、拆包

1. 为什么基于 TCP 的通信程序需要封包、拆包？
   答：TCP 是流协议，所谓流，就是没有界限的一串数据。但是程序中却有多种不同的数据包，那就很可能会出现如上所说的粘包问题，所以就需要在发送端封包，在接收端拆包。
2. 那么如何封包、拆包？
   答：封包就是给一段数据加上包头或者包尾。比如说我们上面为解决粘包所使用的两种方法，其实就是封包与拆包的具体实现。

#### 8. Socket 客户端 队列 的问题

项目中采用了 socket 通信，通过 TCP 发送数据给服务器端，因为项目需要，要同时开启大量的线程去发送不同的数据给服务器端，然后服务器端返回不同的数据。由于操作频繁，经常会阻塞，或没有接收到服务器端返回的数据；

因此考虑到使用一个队列：将同一 ip 下的数据存入一个队列中，通过队列协调发送；当第一条数据发送出去没有收到服务器端返回的数据时，让第二条数据插入队列中排队，当第三条数据也发送出来后，继续排队，以此类推；
如果当第四条数据发出来的时候，存入队列中，第一条数据收服务器端返回数据后，队列中的第二条第三条数据就扔掉，直接发送第四条数据

------

### 💖渲染 & Shader

#### 1. 什么是 LightMap？

LightMap：就是指在三维软件⾥实现打好光，然后渲染把场景各表⾯的光照输出到贴图上，最后⼜通过引擎贴到场景上，这样就使物体有了光照的感觉。

#### 2. MipMap 是什么，作用？

MipMapping：在三维计算机图形的贴图渲染中有常⽤的技术，为加快渲染进度和减少图像锯⻮，贴图被处理成由⼀系列被预先计算和优化过的图⽚组成的⽂件，这样的贴图被称为 MipMap。

#### 3. 请问 alpha test 在何时使用？能达到什么效果？

Alpha Test，中文就是透明度测试。
简而言之就是 V&F shader 中最后 fragment 函数输出的该点颜色值（即上一讲 frag 的输出 half4）的 alpha 值与固定值进行比较。Alpha Test 语句通常于 Pass {} 中的起始位置。Alpha Test 产生的效果也很极端，要么完全透明，即看不到，要么完全不透明。

#### 4. 一个 Terrain，分别贴 3 张，4 张，5 张地表贴图，渲染速度有什么区别？为什么？

没有区别，因为不管几张贴图只渲染一次。

#### 5. 实时点光源的优缺点是什么？

可以有 cookies – 带有 alpha 通道的立方图 (Cubemap) 纹理。点光源是最耗费资源的。

#### 6. 简述水面倒影的渲染原理？

原理就是对水面的贴图纹理进行扰动，以产生波光玲玲的效果。用 shader 可以通过 GPU 在像素级别作扰动，效果细腻，需要的顶点少，速度快

#### 7. MeshRender 中 material 和 sharedmaterial 的区别？

修改 sharedMaterial 将改变所有物体使用这个材质 的外观，并且也改变储存在工程里的材质设置。
不推荐修改由 sharedMaterial 返回的材质。如果你 想修改渲染器的材质，使用 material 替代。

#### 8. 什么是渲染管道？

是指在显示器上为了显示出图像⽽经过的⼀系列必要 操作。 渲染管道中的很多步骤，都要将⼏何物体从⼀个坐标系中变换到另⼀个坐标系中去。

主要步骤有： 本地坐标 -> 视图坐标 -> 背⾯裁剪 -> 光照 -> 裁剪 -> 投影 -> 视图变换 -> 光栅化。

**GPU 工作流程**：顶点处理、光栅化、纹理贴图、像素处理

- **顶点处理**：这阶段 GPU 读取描述 3D 图形外观的顶点数 据并根据顶点数据确定 3D 图形的形状及位置关系，建 ⽴起 3D 图形的⻣架。
- **光栅化**：把⼀个⽮ᰁ图形转换为 ⼀系列像素点的过程就称为光栅化
- **纹理贴图**：就是将多边形的表⾯贴 上相应的图⽚，从⽽⽣成 “真实” 的图形。
- **像素处理**：这阶段（在对每个像素进⾏光栅化处理期 间）GPU 完成对像素的计算和处理，从⽽确定每个像 素的最终属性。
- **最终输出**：由 ROP（光栅化引擎）最终完成像素的输 出，1 帧渲染完毕后，被送到显存帧缓冲区。

总结：GPU 的⼯作通俗的来说就是完成 3D 图形的⽣成，将图形映射到相应的像素点上，对每个像素进⾏ 计算确定最终颜⾊并完成输出。

#### 9. 如何在 Unity3D 中查看场景的面数，顶点数和 DrawCall 数？如何降低 DrawCall 数？

在 Game 视图右上⻆点击 Stats。降低 Draw Call 的技术是 Draw Call Batching

#### 10. 写出光照计算中的 diffuse 的计算公式

diffuse = Kd x colorLight x max(N*L,0)；
Kd 漫反射系数、colorLight 光的颜⾊、N 单位法线向量、L 由点指向光源的单位向量、其中 N 与 L 点乘，如果结果⼩于等于 0，则漫反射为 0。

#### 11. 两种阴影判断的方法、工作原理。

本影和半影：

本影：景物表⾯上那些没有被光源直接照射的区域
（全⿊的轮廓分明的区域）。

半影：景物表⾯上那些被某些特定光源直接照射但并⾮被所有特定光源直接照射的区域（半明半暗区域）

⼯作原理：从光源处向物体的所有可⻅⾯投射光线，将这些⾯投影到场景中得到投影⾯，再将这些投影⾯与场景中的其他平⾯求交得出阴影多边形，保存这些阴影多边形信息，然后再按视点位置对场景进⾏相应处理得到所要求的视图（利⽤空间换时间，每次只需依据视点位置进⾏⼀次阴影计算即可，省去了⼀次消隐过程）

#### 12. 有 A 和 B 两组物体，有什么办法能够保证 A 组物体永远比 B 组物体先渲染？

把 A 组物体的渲染对列⼤于 B 物体的渲染队列。

#### 13. Unity 的 Shader 中，Blend SrcAlpha OneMinusSrcAlpha 这句话是什么意思？

作用就是 Alpha 混合。公式：最终颜色 = 源颜色*源透明值 + 目标颜色*（1 - 源透明值）

#### 14. Vertex Shader 是什么，怎么计算？

顶点着⾊器是⼀段执⾏在 GPU 上的程序，⽤来取代 fixed pipeline 中的 transformation 和 lighting，Vertex Shader 主要操作顶点。
Vertex Shader 对输⼊顶点完成了从 local space 到 homogeneous space（⻬次空间）的变换过程，homogeneous space 即 projection space 的下⼀个 space。
在这其间共有 world transformation, view transformation 和 projection transformation 及 lighting ⼏个过程

#### 15. Unity3D Shader 分哪⼏种，有什么区别？

表⾯着⾊器 的抽象层次⽐较⾼，它可以轻松地以简洁⽅式实现复杂着⾊。表⾯着⾊器可同时在前向渲染及延迟渲染模式下正常⼯作。

顶点⽚段着⾊器可以⾮常灵活地实现需要的效果，但是需要编写更多的代码，并且很难与 Unity 的渲染管线完美集成。

固定功能管线着⾊器可以作为前两种着⾊器的备⽤选择，当硬件⽆法运⾏那些酷炫 Shader 的时，还可以通过固定功能管线着⾊器来绘制出⼀些基本的内容。

------

### 💘优化部分

```csharp
解释微信搜索：呆呆敲代码的小Y
回复：白嫖
免费获取很多的编程资料哦！
123
```

更多优化知识学习文章：[【Unity 优化篇】 | 优化专栏《导航帖》，全面学习 Unity 优化技巧，让我们的 Unity 技术上升一个档次](https://xiaoy.blog.csdn.net/article/details/122168860)

#### 1. 简述⼀下对象池，你觉得在 FPS 里哪些东西适合使用对象池？

对象池就存放需要被反复调⽤资源的⼀个空间，⽐如游戏中要常被大量复制的对象，⼦弹，敌⼈，以及任何重复出现的对象。

#### 2. 什么是 DrawCall？DrawCall 高了又什么影响？如何降低 DrawCall？

Unity 中，每次引擎准备数据并通知 GPU 的过程称为一次 Draw Call。DrawCall 越高对显卡的消耗就越大。降低 DrawCall 的方法：

- Dynamic Batching
- Static Batching
- 高级特性 Shader 降级为统一的低级特性的 Shader。

#### 3. UI 优化小知识

1. 将同一画面图片放到同一图集中
2. 图片和文字尽量不要交叉，会产生多余 drawcall（相同材质和纹理的 UI 元素是可以合并的）
3. UI 层级尽量不要重叠太多
4. 取消勾选不必要的射线检测 RaycastTarget
5. 将动态的 UI 元素和静态的 UI 元素放在不同的 Canvas 中，减少 canvas 网格重构频率

#### 4. LOD 是什么，优缺点是什么？

LOD (Level of detail) 多层次细节，是最常用的游 戏优化技术。
它按照模型的位置和重要程度决定 物体渲染的资源分配，降低非重要物体的面数和 细节度，从而获得高效率的渲染运算。

#### 5. 什么叫动态合批？跟静态合批有什么区别？

如果动态物体共用着相同的材质，那么 Unity 会自动对 这些物体进行批处理。动态批处理操作是自动完成 的，并不需要你进行额外的操作。

**区别**：动态批处理一切都是自动的，不需要做任何操 作，而且物体是可以移动的，但是限制很多。
静态批 处理：自由度很高，限制很少，缺点可能会占用更多 的内存，而且经过静态批处理后的所有物体都不可以 再移动了。

#### 6. 如何优化内存？

有很多种方式，例如

1. 压缩自带类库；
2. 将暂时不用的以后还需要使用的物体隐藏起来而不 是直接 Destroy 掉；
3. 释放 AssetBundle 占用的资源；
4. 降低模型的片面数，降低模型的⻣骼数量，降低贴 图的大小；
5. 使用光照贴图，使用多层次细节 (LOD)，使用着色 器 (Shader)，使用预设 (Prefab)。

#### 7. 请简述 GC（垃圾回收）产生的原因，并描述如何避免？

GC 垃圾回收机制，避免堆内存溢出，定期回收那些没有有效引用的对象内存
GC 优化，就是优化堆内存，减少堆内存，即时回收堆内存
GC 归属于 CLR

避免：

1. 减少 new 的次数
2. 字符串拼接使用 stringbuilder，字符串比较先定义一个变量存储，防止产生无效内存
3. list，new 时候，规定内存大小
4. 如果要射线检测，应该使用避免 GC 的方法 XXXXNoAlloc 函数
5. foreach 迭代器容易导致 GC（目前 Unity5.5 已修复），使用 For 循环
6. 使用静态变量，GC 不会回收存在的对象，但静态变量的引用对象可能被回收
7. 使用枚举替代字符串变量
8. 调用 gameobject.tag=="XXX" 就会产生内存垃圾；那么采用 GameObject.CompareTag () 可以避免内存垃圾的产生：
9. 不要在频繁调用的函数中反复进行堆内存分配，比如 OnTriggerXXX，Update 等函数
10. 在 Update 函数中，运行有规律的但不需要每一帧执行的代码，可以使用计时器，比如 1 秒执行一次某些代码！！！

更多 GC 内容可查看本篇文章：
[Unity 零基础到进阶 ☀️| Unity 中的 GC 及优化 超级全面解析 ☆(ゝ ω･) v 建议收藏！](https://xiaoy.blog.csdn.net/article/details/116531700)

#### 8. 贴图透明通道分离，压缩格式设为 ETC/PVRTC

最初我们使用了 DXT5 作为贴图压缩格式，希望能减小贴图的内存占用，但很快发现移动平台的显卡是不支持的。因此对于一张 1024×1024 大小的 RGBA32 贴图，虽然 DXT5 可将它从 4MB 压缩到 1MB，但系统将它送进显卡之前，会先用 CPU 在内存里将它解压成 4MB 的 RGBA32 格式（软件解压），然后再将这 4MB 送进显存。于是在这段时间里，这张贴图就占用了 5MB 内存和 4MB 显存；而移动平台往往没有独立显存，需要从内存里抠一块作为显存，于是原以为只占 1MB 内存的贴图实际却占了 9MB！

所有不支持硬件解压的压缩格式都有这个问题。经过一番调研，我们发现安卓上硬件支持最广泛的格式是 ETC，苹果上则是 PVRTC。但这两种格式都是不带透明（Alpha）通道的。因此我们将每张原始贴图的透明通道都分离了出来，写进另一张贴图的红色通道里。这两张贴图都采用 ETC/PVRTC 压缩。渲染的时候，将两张贴图都送进显存。同时我们修改了 NGUI 的 shader，在渲染时将第二张贴图的红色通道写到第一张贴图的透明通道里，恢复原来的颜色：

```csharp
解释fixed4 frag (v2f i) : COLOR  
 
    fixed4 col;  
 
    col.rgb = tex2D(_MainTex, i.texcoord).rgb;  
 
    col.a = tex2D(_AlphaTex, i.texcoord).r;  
 
    return col * i.color;  
 
fixed4 frag (v2f i) : COLOR
 
{
 
    fixed4 col;
 
    col.rgb = tex2D(_MainTex, i.texcoord).rgb;
 
    col.a = tex2D(_AlphaTex, i.texcoord).r;
 
    return col * i.color;
 
}
```

#### 9. 关闭贴图的读写选项

Unity 中导入的每张贴图都有一个启用可读可写（Read/Write Enabled）的开关，对应的程序参数是 TextureImporter.isReadable。
选中贴图后可在 Import Setting 选项卡中看到这个开关。只有打开这个开关，才可以对贴图使用 Texture2D.GetPixel，读取或改写贴图资源的像素，但这就需要系统在内存里保留一份贴图的拷贝，以供 CPU 访问。
一般游戏运行时不会有这样的需求，因此我们对所有贴图都关闭了这个开关，只在编辑中做贴图导入后处理（比如对原始贴图分离透明通道）时打开它。
这样，上文提到的 1024×1024 大小的贴图，其运行时的 2MB 内存占用又可以少一半，减小到 1MB。

#### 10. Unity 在移动设备上的⼀些优化资源的方法

1. 使⽤ assetbundle，实现资源分离和共享，将内存控
   制到 200m 之内，同时也可以实现资源的在线更新
2. 顶点数对渲染⽆论是 cpu 还是 gpu 都是压⼒最⼤的贡
   献者，降低顶点数到 8 万以下，fps 稳定到了 30 帧左右
3. 只使⽤⼀盏动态光，不是⽤阴影，不使⽤光照探头
   粒⼦系统是 cpu 上的⼤头
4. 剪裁粒⼦系统
5. 合并同时出现的粒⼦系统
6. ⾃⼰实现轻量级的粒⼦系统
   animator 也是⼀个效率奇差的地⽅
7. 把不需要跟⻣骼动画和动作过渡的地⽅全部使⽤
   animation，控制⻣骼数ᰁ在 30 根以下
8. animator 出视ᰀ不更新
9. 删除⽆意义的 animator
10. animator 的初始化很耗时（粒⼦上能不能尽ᰁ不⽤
    animator）
11. 除主⻆外都不要跟⻣骼运动 apply root motion
12. 绝对禁⽌掉那些不带刚体带包围盒的物体（static
    collider ）运动
    NUGI 的代码效率很差，基本上 runtime 的时候对 cpu 的
    贡献和 render 不相上下
13. 每帧递归的计算 finalalpha 改为只有初始化和变动时
    计算
14. 去掉法线计算
15. 不要每帧计算 viewsize 和 windowsize
16. filldrawcall 时构建顶点缓存使⽤ array.copy
17. 代码剪裁：使⽤ strip level ，使⽤.net2.0 subset
18. 尽量减少 smooth group
19. 给美术定⼀个严格的经过科学验证的美术标准，并在 U3D ⾥⾯配以相应的检查⼯具

#### 11. CPU 端性能优化小知识点

- 逻辑和表现尽可能分离开，这样逻辑层的更新频率可以适当降低些.
- 对于一些热点函数，如 mmo 的实体更新、实例化，使用分帧处理，分摊单帧时间消耗.
- 做好同屏实体数量、特效数量、距离显隐等优化.
- 完善日志输出，避免没必要的日志输出，同时警惕日志字符串拼接.
- 使用骨骼烘焙 + GPUSkinning + Instance 降低 CPU 蒙皮骨骼消耗和 drawcall.
- 开启模型的 Optimize GameObjects 减少节点数量和蒙皮更新消耗.
- UI 拼预制做好动静分离，对于像血条名字这种频繁变动的 ui，做好适当的分组.
- 减少 C# 和 lua 的频繁交互，尽量精简两者传递的参数结构.
- 使用 stringbuilder 优化字符串拼接的 gc 问题.
- 删除非必要的脚本功能函数，特别是 Update/LateUpdate 类高频执行函数，因为会产生 C++ 到 C# 层的调用开销。对于 Update 里需要用到的组件、节点等提前 Cache 好.
- 场景里频繁使用的资源或数据结构做好资源复用和对象池.
- 对于频繁显示隐藏的 UI，可以先移出到屏幕外，如果长时间不显示再进行 Deactive.
- 合理拆分 UI 图集，区分共用图集和非共用图集，共用图集可以常驻内存，非共用图集优先按功能分类，避免资源冗余.
  使用 IL2CPP, 编译成 C++ 版本能极大的提升整体性能.
- 避免直接使用 Material.Setxxx/Getxxx 等调用，这些调用会触发材质实例化消耗，可以考虑使用 SharedMaterial / MaterialPropertyBlock 代替.
- 合并 Shader 里的 Uniform 变量.

#### 12. GPU 端性能优化小知识点

- 合理规划好渲染顺序，避免不必要的 overdraw，如：地形（容易被其他物件遮挡）、天空盒放到较后渲染.
- 分辨率缩放，对于填充率出现瓶颈时，这个是最简单高效的.
- 避免使用 GrabPass 抓屏，不是所有硬件都支持，加之数据回拷和没法控制分辨率性能很差，可考虑使用 CommandBuffer.blit 去优化.
- 控制好地形的 Blend 层数，控制在 4 层以内，考虑到地形一般屏占面积大、贴图采样次数多，对于中低画质考虑不用 normalmap.
- 做好物件、树、角色的 LOD.
- 避免使用 RenderWithShader 类方式来定制 DepthTexture, 可以考虑 Camera 的 public void SetTargetBuffers (RenderBuffer colorBuffer, RenderBuffer depthBuffer); 进行优化.
- 检查 Shader 的 VertexInput 和 VertexOutput 是否存在冗余数据。如：顶点色、多套 UV.
- 警惕项目里非必要的双面材质，对于需要局部双面的地方通过加面解决.
- Shader 里使用 fixed、half 代替 float，理论上除 position、uv、一些涉及 depth 相关计算使用 float 外，其他都应该使用 fixed（主要是颜色值）、half.
- 对于角色皮肤这种不是特别明显的效果，考虑使用预积分这种低成本的方案.
- 对于 frag 里的计算过程，如果可以抽出来放到 CPU 应用层、顶点阶段的优先放这里计算。需要注意放到顶点阶段引起的平滑过渡问题。如: eyeVec 导致高光过渡问题.
- 镜面反射类效果避免使用反射相机 + RT 的实现，考虑使用 SSR、CubeMap 类实现.
- 避免使用实时阴影，如若使用要合理控制下分辨率和阴影距离。考虑使用 Projector.
- 使用统一的后处理框架代替多个 Image Effect，可以共用模糊函数，减少 blit 操作。另外 Unity 自带的 Postprocessing V2 支持 Volume，性能还是不错的.
- Shader 里避免使用分支、循环，sin、tan、pow、log 等复杂数学运算.
- Unity 自带的遮挡剔除因为 CPU 消耗和内存占用较高，加之不能 Instancing，不太适合移动平台，可以考虑静态预计算 (缺点是不支持动态物体)、Hi-Z 等优化方案.
- 减少 alpha test 材质的使用，如若使用注意减小面积、控制渲染顺序.

#### 13. 内存优化小知识点

- 警惕配置表的内存占用.
- 检查 ShaderLab 内存占用:
- 避免使用 Standard 材质，做好相应的 variant skip.
- 排查项目冗余的 Shader.
- 使用 shader_feature 替代 multi_compile, 这样只会收集项目里真正使用的变体组合，避免变体翻倍.
- 检查纹理资源的尺寸、格式、压缩方式、mipmap、Read & Write 选项使用是否合理.
- 检查 Mesh 资源的 Read & Write 选项、顶点属性使用是否合理.
  代码级别的检查，如 Cache 预分配空间、容器的 Capacity、GC 等.
- 使用 Profiler 定位下 GC，特别是 Update 类函数里的。如：字符串拼接、滥用容器等.
- 合理控制 RenderTexture 的尺寸.
  优化动画 Animation 的压缩方式、浮点精度、去除里面的 Scale 曲线数据.
- 减少场景 GameObject 节点的数量，最好支持工具监控.

------

### 💞算法

#### 1. 请写出求斐波那契数列任意一位的值得算法

```csharp
解释static int Fn(int n) {
if (n <= 0) {
throw new ArgumentOutOfRangeException();
}
if (n == 1||n==2) 
{
return 1; 
}
return checked(Fn(n - 1) + Fn(n - 2)); // when n>46 memory will overflow
}
```

#### 2. 下列代码在运行中会产生几个临时对象？

```csharp
string a = new string("abc"); 
a = (a.ToUpper() +"123").Substring(0,2);
```

实在 C# 中第⼀⾏是会出错的（Java 中倒是可⾏）。
应该这样初始化：

```csharp
string b = new string(new char[] {'a','b','c'});
```

三个临时对象：abc、ABC、AB

#### 3. 冒泡排序

```csharp
解释using System;  
  
class Program  
{  
    static void Main()  
    {  
        int[] array = { 9, 2, 6, 3, 7, 1, 8, 4, 5 };  
        Console.WriteLine("原始数组:");  
        foreach (int number in array)  
        {  
            Console.Write(number + " ");  
        }  
        Console.WriteLine();  
  
        BubbleSort(array);  
  
        Console.WriteLine("排序后的数组:");  
        foreach (int number in array)  
        {  
            Console.Write(number + " ");  
        }  
    }  
  
    static void BubbleSort(int[] array)  
    {  
        int n = array.Length;  
        for (int i = 0; i < n - 1; i++)  
        {  
            for (int j = 0; j < n - i - 1)  
            {  
                if (array[j] > array[j + 1])  
                {  
                    // 交换元素  
                    int temp = array[j];  
                    array[j] = array[j + 1];  
                    array[j + 1] = temp;  
                }  
            }  
        }  
    }  
}
```