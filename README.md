# iOS 泛型 总结
 泛型是程序设计语言的一种特性，主要是为了限制类型的。
1. 限制集合（容器）中的类型，只能检测方法的调用，因为声明的泛型只能存在方法中，比如OC中的数组，你可以限制他里面装的是NSString类型。

```
@property (nonatomic, strong) NSMutableArray<NSString *> *name;
[self.name addObject:@"first"];//由于声明了NSString类型只能添加NSString 类型
//如果不是泛型的话数组和字典等容器返回的是id类型无法调用点语法。使用泛型之后便返回声明类型具有声明类型的所有操作。
NSLog(@"%ld",self.name[0].length) ;
```
2. 当一个类在声明的时候，某个对象的属性不确定，只有创建对象的时候才能确定，就可以使用泛型。

```
//创建 在类的后面先声明，声明是随意的 ‘未知类型‘ 一般用‘ObjectType’规范
@interface ExistenceClass<未知类型> : UIView
@property (nonatomic, strong) 未知类型 unknown;//用声明去创建属性
@property (nonatomic, strong) NSArray *array;
@end
//使用时指定为NSString 那么e实例下的unknown属性就是NSString类型
ExistenceClass<NSString *> *e = [[ExistenceClass alloc]init];
e.unknown = @"123";
NSLog(@"%@====%@=====%lu",NSStringFromClass([e.unknown class]),e.unknown.class,(unsigned long)e.unknown.length);
```
3. 协变性与逆变性
* 	默认带有泛型的变量相互赋值会有警告，使用协变和逆变可以解决警告的问题
* __covariant 协变 向上转型，子类转父类
* __contravariant 逆变 向下转型，父类转子类

```
@interface ExistenceClass<__covariant 未知类型> : UIView
ExistenceClass<NSString *> *e = [[ExistenceClass alloc]init];    
ExistenceClass<NSMutableString *> *e2 = [[ExistenceClass alloc] init];
//这时候就可以把子类给父类赋值转换了
e = e2;
```
4. __kindof：相当于,表示某个类或者他的子类。设计模型中可以使用，当给某个类提供类方法，想让外界调用能看到创建什么对象，并且不报警告。

```
//用__kindof声明
@interface ExistenceClass<__covariant ObjectType> : UIView
@property (nonatomic, strong) ObjectType unknown;
@property (nonatomic, strong) NSArray *array;
- (__kindof NSArray *) returnArray;
+ (__kindof NSArray *) returnArray;

@end
//返回NSArray类型和其子类都是可以的
NSArray *array1 = [ExistenceClass returnArray];
NSMutableArray *array2 = [ExistenceClass returnArray];
@implementation ExistenceClass
- (NSArray *)returnArray {
    return [NSMutableArray array];
}
@end
	//在UITableViewCell的返回中可以看到这种使用方式
```


 泛型的基本格式：
*  定义格式:放在限制的类型后面<类型>
*  声明格式:在声明类的时候,在类型后面<泛型名称>
 泛型的好处：
*  提高程序员开发规范，让程序员一眼就可以看出该使用什么类型
*  限制类型，不允许装入其它的类型
*  可以使用点语法
## id
