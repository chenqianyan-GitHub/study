# 1.访问修饰符
1.public：访问权限最广的修饰符，可以跨类跨包访问；

2.protect：只允许同一个包下或者该类的子类访问；

3.default：默认情况，只允许同一个包下的对象访问，不允许不同包的子类访问；

4.private：只允许在该类下访问，不允许其子类访问。

# 2.this
1.可以对当前类下的成员变量直接引用，如this.name;

2.在构造函数中，对与成员变量重名的形参引用，如this.name=name；

3.直接代指本类中的构造函数，如this(name)，并且必须放在构造函数的第一行；

# 3.static
被static修饰的方法或变量被所有该类的实例对象共享，即不管该类被new多少次，所有的对象都共用一个static。
static修饰的静态代码块只会在类加载时执行一次。

