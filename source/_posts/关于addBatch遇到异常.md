---
title: 关于addBatch遇到异常
date: 2022-09-25 20:57:12
updated: 2022-09-25 20:57:12
tags:
- jdbc
---

最近在某个项目里开始手写jdbc数据库操作。一般在遇到多条数据插入时会使用addBatch来批量执行以提高效率。
但是平时用习惯了mybatis之类的ORM框架，直接使用jdbc去数据操作产生许多疑问。

## 疑问一：addBatch在某一条数据失败后会怎么样？

通过代码进行实际验证一下，在批量插入的10条数据中选择其中一条设置为超过字段长度的值。

```java
Connection connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);
PreparedStatement pstmt = connection.prepareStatement("insert into T_USER (id, name) values (?,?)");
try {

    for (int i = 0; i < 10; i++) {
        if (i == 5) {
            pstmt.setInt(1, i);
            pstmt.setString(2, "12345678901234567890");//T_USER.NAME字段长度为10
        } else {
            pstmt.setInt(1, i);
            pstmt.setString(2, "user" + i);
        }
        pstmt.addBatch();
    }

    pstmt.executeBatch();

} catch (SQLException e) {
    e.printStackTrace();
} finally {
    //一些资源关闭代码
}
```
代码执行后理所当然的抛出了异常，查看表的数据结果为：

| ID  | NAME  |
|-----|-------|
| 0   | user0 |
| 1   | user1 |
| 2   | user2 |
| 3   | user3 |
| 4   | user4 |

错误数据之前的都已经插入成功了，但一般情况下业务都是全部操作成功或者全部操作失败，这时就需要关闭自动commit进行手动的commit和rollback。

## 疑问二：如果addBatc失败了，继续使用同一个PreparedStatement对象进行addBatch操作会怎么样？

进行代码验证：

```java
Connection connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);
PreparedStatement pstmt = connection.prepareStatement("insert into T_USER (id, name) values (?,?)");
try {

    for (int i = 0; i < 10; i++) {
        if (i == 5) {
            pstmt.setInt(1, i);
            pstmt.setString(2, "12345678901234567890");//T_USER.NAME字段长度为10
        } else {
            pstmt.setInt(1, i);
            pstmt.setString(2, "user" + i);
        }
        pstmt.addBatch();
    }

    try {
        pstmt.executeBatch();
    } catch (SQLException e) {
        e.printStackTrace();
    }

    for (int i = 10; i < 20; i++) {
        pstmt.setInt(1, i);
        pstmt.setString(2, "user" + i);
        pstmt.addBatch();
    }

    try {
        pstmt.executeBatch();
    } catch (SQLException e) {
        e.printStackTrace();
    }

} catch (SQLException e) {
    e.printStackTrace();
} finally {
    if (pstmt != null) {
        pstmt.close();
    }
    if (connection != null) {
        connection.close();
    }
}
```

代码执行后得到的结果如下：

| ID  | NAME   |
|-----|--------|
| 0   | user0  |
| 1   | user1  |
| 2   | user2  |
| 3   | user3  |
| 4   | user4  |
| 10  | user10 |
| 11  | user11 |
| 12  | user12 |
| 13  | user13 |
| 14  | user14 |
| 15  | user15 |
| 16  | user16 |
| 17  | user17 |
| 18  | user18 |
| 19  | user19 |

可以发现带有错误数据的Batch操作结果与疑问一的结果一致，并且再次使用同一个PreparedStatement对象进行addBatch不会受影响。