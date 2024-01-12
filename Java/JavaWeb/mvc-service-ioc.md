## IOC和DI的粗略实现

### IOC

**`beanMap:`** 在**ClassPathXmlapplicationContext**中声明一个容器 **beanMap<String,Object>** 

- 通过IO流读取 ApplicationContext.xml 文件中的Dom获取到**beanId**和**className**
- 再className再反射Class.forName(className)和newInstance()获取实例对象**beanObj**
- 最后`beanMap.put(beanId,beanObj)`

###  DI  

- DOM技术获取**propertyName**和**propertyRef**  =>  
- beanClazz.getDeclaredField(propertyName)反射出**propertyField**，beanMap.get(propertyRef)获取**refObj**  =>  
- 最后`propertyField.setAttribute(beanObj,refObj)`
