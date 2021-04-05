# git 提交规范

## <font color="green">commit规则</font>

```text
<type>(<scope>):<subject>
```

- type
  - 用于说明 `commit` 的类型，只允许使用下面的7个标识。
  ```text
  feat: 新功能 (feature)。
  fix: fix bug。
  docs: 文档 (documentation)。
  style: 格式 (不影响代码运行的变动)。
  test: 增加测试。
  chore: 构建过程和辅助工具的变动。
  ```
- scope
  
  - 用于说明 `commit` 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。
- subject
  - 是 `commit` 目的的简短描述，不超过50个字符。
  ```text
  1. 以动词开头，使用第一人称现在时，比如change，而不是changed或changes。
  2. 第一个字母小写。
  3. 结尾不加句号（.）。
  ```
