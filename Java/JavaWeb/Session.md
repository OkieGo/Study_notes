## Session

### Session的工作机制

- 当服务器端调用了request.getSession()方法
  - 检查当前请求中是否携带了JSESSIONID这个Cookie
    - 有携带：根据这个JSESSIONID在服务器端查找对应的Session对象
      - 能找到：把找到的Session作为getSession()方法的返回值返回
      - 找不到：
        - 创建新的Session对象
        - 用新的JSESSIONID的值创建Cookie
        - 把新的Cookie返回给浏览器
    - 没携带：
      - 创建新的Session对象
      - 用新的JSESSIONID的值创建Cookie
      - 把新的Cookie返回给浏览器

### Session时效性机制

- Session从创建开始计时
- 倒计时过程中，有请求访问这个Session，重新计时
- Session倒计时结束，Session对象就会释放

### Session主要方法

- getServletContext()
- getCreationTime
- getId()
- isNew()
- getInactiveInterval()
- setInactiveInterval()
- getAttribute()
- setAttribute()
- getAttributeNames()
- invalidate()