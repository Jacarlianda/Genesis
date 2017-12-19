### 对象

#### 一个精心编写的对象

* 拥有清晰的边界
* 执行一组有限的活动
* 仅知道它的数据以及它完成其活动所需的其他所有对象

实质上，对象是一个离散的实体·，它仅在其它对象上拥有必要的依赖关系来执行其任务。

### 向一个Java类添加行为

#### 访问方法

* 属性本身总是使用private访问进行声明
* getter 和 setter的访问说明符是public
* getter不接受任何参数，能返回一个与它访问的属性具有相同类型的值
* setter仅接收一个与属性具有相同类型的参数，不返回值

#### Java 语言的算术运算符

| 运算符  | 用法       | 描述                                       |
| ---- | -------- | ---------------------------------------- |
| `+`  | `a + b`  | 将 `a` 和 `b` 相加                           |
| `+`  | `+a`     | 如果 `a` 为 `byte`、`short` 或 `char`，则将它升级为 `int` |
| `-`  | `a - b`  | 从 `a` 中减去 `b`                            |
| `-`  | `-a`     | 求 `a` 的算术上的负数                            |
| `*`  | `a * b`  | 将 `a` 和 `b` 相乘                           |
| `/`  | `a / b`  | 将 `a` 除以 `b`                             |
| `%`  | `a % b`  | 返回将 `a` 除以 `b` 的余数（取模运算符）                |
| `++` | `a++`    | 将 `a` 递增 1；计算递增之前 `a` 的值                 |
| `++` | `++a`    | 将 `a` 递增 1；计算递增之后 `a` 的值                 |
| `--` | `a--`    | 将 `a` 递减 1；计算递减之前 `a` 的值                 |
| `--` | `--a`    | 将 `a` 递减 1；计算递减之后 `a` 的值                 |
| `+=` | `a += b` | `a = a + b` 的简写形式                        |
| `-=` | `a -= b` | `a = a - b` 的简写形式                        |
| `*=` | `a *= b` | `a = a * b` 的简写形式                        |
| `%=` | `a %= b` | `a = a % b` 的简写形式                        |

#### 关系运算符

| 运算符  | 用法       | 返回 true 的条件……                            |
| ---- | -------- | ---------------------------------------- |
| `>`  | `a > b`  | `a` 大于 `b`                               |
| `>=` | `a >= b` | `a` 大于或等于 `b`                            |
| `<`  | `a < b`  | `a` 小于 `b`                               |
| `<=` | `a <= b` | `a` 小于或等于 `b`                            |
| `==` | `a == b` | `a` 等于 `b`                               |
| `!=` | `a != b` | `a` 不等于 `b`                              |
| `&&` | `a && b` | 如果 `a` 和 `b` 都为 true，则有条件地计算 `b`（如果 `a` 为 false，则不计算 `b`） |
| `||` | `a || b` | 如果 `a` 或 `b` 为 true，则有条件地计算 `b`（如果 `a` 为 true，则不计算 `b`） |
| `!`  | `!a`     | `a` 为 false                              |
| `&`  | `a & b`  | 如果 `a` 和 `b` 都为 true，则始终计算 `b`           |
| `|`  | `a | b`  | 如果 `a` 或 `b` 为 true，则始终计算 `b`            |
| `^`  | `a ^ b`  | `a` 和 `b` 不同                             |



(conditional) ?statementIfTrue :statementIfFalse;

if conditional 为true 执行statementIfTrue ，否则执行statementIfFalse

### 重载方法

重载方法：创建两个具有相同名称，但是具有不同参数列表（即不同数量或类型的参数）的方法时，就拥有一个重载方法。在运行时，Java运行时环境根据传递的参数来决定调用哪个重载方法的变体。

~~~java
public void printAudit(StringBuilder buffer) {
  buffer.append("Name="); buffer.append(getName());
  buffer.append(","); buffer.append("Age="); buffer.append(getAge());
  buffer.append(","); buffer.append("Height="); buffer.append(getHeight());
  buffer.append(","); buffer.append("Weight="); buffer.append(getWeight());
  buffer.append(","); buffer.append("EyeColor="); buffer.append(getEyeColor());
  buffer.append(","); buffer.append("Gender="); buffer.append(getGender());
}
public void printAudit(Logger l) {
  StringBuilder sb = new StringBuilder();
  printAudit(sb);
  l.info(sb.toString());
}
~~~

#### 重载规则

* 不能仅通过更改一个方法的返回类型来重载该方法
* 不能有两个名称和参数列表都相同的方法

####  覆盖方法

如果一个类的另一个自雷提供父类中已定义方法的自有方法，就称为方法覆盖。































