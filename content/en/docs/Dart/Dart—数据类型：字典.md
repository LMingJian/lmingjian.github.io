---
title: Dart 数据类型：字典
date: 2021-04-28
author: LMingJian
---

## 1.声明

在 Dart 中，字典可以通过`Map()`或者`{key: value}`来进行声明创建，字典的类型为`Map`。

### a.不指定泛型

```dart
//直接赋值
    var map1 = {'aa':'aaa','bb':22,'cc':true}; 
    Map map2 = {'a':'a1','b':'b1'};  
//间接赋值
    var map3 = new Map(); 
    map3['aa'] = 'aaa'; 
    Map map4 = new Map(); 
    map4['a'] = 'aaa';
```

### b.指定泛型

```dart
//直接赋值
    var map1 = <String,String>{'aa':'aaa','bb':'22','cc':'333'};
    Map map2 = <String,String>{'a':'a1','b':'b1','c':'c1'};
    
//间接赋值
    var map3 = new Map<String,String>();
    map3['aa'] = 'aaa';
    Map map4 = new Map<String,String>();
    map4['a'] = 'a1';
```

### c.复制

```dart
// 不使用类型操作符,从另一个map中初始化新的map，此时新的map中含有另一个map中的资源
    Map map1 = {'a':'a1','b':'b1','c':'c1'};
    Map map2 = Map.castFrom(map1);

// 强制使用指定类型初始化map
    Map<int,String> map3 = {1:'a',2:'b',3:'c'};
    Map map4 = Map.castFrom<num,String>(map3);
    
// 这行代码会出错，主要原因是testMap是<dynamic,dynamic>类型的，但是这里需要的是<int,String>类型的map
    Map map5 = Map.castFrom<String,String>(map3);
    
// 这行代码也会出错，因为无法将<String,String>类型的map转换为<int,String>类型的map
    Map map6 = Map.castFrom<int,String>(map3); // 正确
```

### d.创建不可变的 map

```dart
Map map6 = const {'one':'Android','two':'IOS','three':'flutter'};
Map map7 = Map.unmodifiable(map6);
```

### e.根据 list 所提供的 key value 来创建 map

```dart
List<String> keys = ['one','two']; 
List<String> values = ['Android','IOS']; 
Map map = Map.fromIterables(keys, values); 
```

## 2.属性

```dart
print(map.length);        //2     长度
print(map.isNotEmpty);    //true  是否不为空
print(map.isEmpty);       //false   是否为空  
print(map.keys);          //(a, b)  key的集合
print(map.values);        //(1, 2)  value的集合
print(map.entries);       //(MapEntry(a: 1), MapEntry(b: 2))  map迭代的键值对集合 
```

## 3.方法

### a.增

```dart
Map<String,int> map7 = {"a":1,"b":2,"c":3,"d":4,"e":5};
// 新增一个key value
map7["f"] = 6;
```

### b.删

```dart
// 删除一个key
    Map<String,int> map9 = {"a":1,"b":2,"c":3,"d":4,"e":5};
    map9.remove("b");

// 根据条件批量删除
    Map<String,int> map10 = {"a":1,"b":2,"c":3,"d":4,"e":5};
    map10.removeWhere((key,value)=>(value>3));
```

### c.查

```dart
// 是否包含key
   Map<String,int> map11 = {"a":1,"b":2,"c":3,"d":4,"e":5};
   print(map11.containsKey("a"));//true   是否包含key
   print(map11.containsKey("aa"));//false  是否包含key
   
// 是否包含value值
   Map<String,int> map17 = {"a":1,"b":2,"c":3};
   print(map17.containsValue(1));//true
   print(map17.containsValue(4));//false
   
// 遍历
    Map<String,int> map12 = {"a":1,"b":2,"c":3,"d":4,"e":5};
    map12.forEach((String key,int value){
        print("$key  $value");
    });
    
// 遍历时修改value值，PS:遍历时，新增或删除key，都会报错
     Map<String,int> map13 = {"a":1,"b":2,"c":3};
    map13.forEach((String key,int value){
    print("$key  $value");
        map13["c"] = 4;
    });
```

### d.改

```dart
// 修改一个key的value
Map<String,int> map8 = {"a":1,"b":2,"c":3,"d":4,"e":5};
map8["a"] = 11;

// 对指定的key的value做出修改
Map<String,int> map23 = {"a":1,"b":2,"c":3};
int result3 = map23.update("a", (value)=>(value*2));
int result4 = map23.update("d", (value)=>(value*2));
int result4 = map23.update("d", (value)=>(value*2),ifAbsent: ()=>(10));

// 根据参数函数的规则，批量修改map
Map<String,int> map24 = {"a":1,"b":2,"c":3};
map24.updateAll((String key,int value){
   return value*2;
});
Map<String,int> map25 = {"a":1,"b":2,"c":3};
map25.updateAll((String key,int value){
   if(key=="a"){return 10;}
   if(key=="b"){return 20;}
   return value*2;
});
```

### e.其他

```dart
// 遍历每个元素 根据参数函数，对keyvalue做出修改，可转换成其他泛型的Map
  Map<String,int> map19 = {"a":1,"b":2,"c":3};
    Map<int,String> map20 = map19.map((String key,int value){
      return new MapEntry(value, key);
    });

// 清空map
   Map<String,int> map15 = {"a":1,"b":2,"c":3};
   map15.clear();

// 整体合并另一个map 泛型要一致
   Map<String,int> map16 = {"a":1,"b":2,"c":3};
   Map<String,int> other = {"a":1,"c":4,"d":7};
   map16.addAll(other);//key相同时value值后者覆盖前者，前者不存在时则添加进来

// 合并两个map 如果key有重复，被合并的map的value覆盖前者
    Map<String,int> map26 = {"a":1,"b":2,"c":3};
    Map<String,int> map27 = {"a":1,"b":4,"d":3,"e":5};
    map26.addEntries(map27.entries);

// 存在key就获取值，不存在则添加到map 然后返回值
    Map<String,int> map18 = {"a":1,"b":2,"c":3};
    int result = map18.putIfAbsent("a", ()=>(2));//存在
    int result2 = map18.putIfAbsent("d", ()=>(2));//不存在

// 泛型类型提升为其父祖类
    Map<String,int> map21 = {"a":1,"b":2,"c":3};
    Map<Object,Object> map22 = map21.cast();
    map22["d"]=33;
```

