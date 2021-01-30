jdbc连接数据库的步骤
1. 注册驱动
2. 获取链接
3. 构建statement对象或侯建preparedStatement对象
4. 执行sql语句
5. 有结果集处理结果集，没有结果集执行第6步
6. 关闭资源

Statement和preparedStatement的区别：

1. 执行sql的位置不一样
2. statement 每执行sql语句都需要编译一次
3. PreparedStatement 只编译一次。(针对同构的sql)
4. statement 不支持占位