# 1.Java的基本类型、包装类以及自动装箱
byte、short、int、long、float、double、char、boolean，
其中byte <（short=char）< int < long < float < double，小转大时可以自动转换，大转小时需要强制类型转化且可能丢失数据；

int：有符号的32位，范围[-2^31, 2^31-1]；

包装类的作用：方便对对象进行操作，各种集合在使用时只允许存放包装类；

自动装箱：把基本类型包装成包装类；方便包装类直接引用基本类型；

自动拆箱：把包装类转化为基本类型，例如当Integer类型的对象与int类型的对象用==比较时，会把Integer类型自动转化为int类型。
