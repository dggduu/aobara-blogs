+++
author = "aobara"
title = "vue相关的笔记"
date = "2025-05-24"
categories = [
    "笔记",
    "前端"
]
+++
## 数据库
### mongoDB
### mysql
#### 登录
```sh
mysql -u name -p ...
```
#### 忘记密码
```sh
sudo systemctl stop mysql
sudo mysqld_safe --skip-grant-tables &
```
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '...';
```
#### 创建数据库
```sql
//创建数据库
create database test_wzh character set utf8mb4 collate utf8mb4_general_ci;
//utf8mb4_general_ci 不区分字符串大小写
//utf8mb4_unicode_ci 区分字符串大小写

//删除数据库
drop database test;

//选择数据库
USE database_name;
mysql -u your_username -p -D your_database

//创建数据库
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

show columns from user_list;

//插入数据
//显式插入数据
INSERT INTO user_list (id, name, years, is_auth)
VALUES (NULL, 'test', '2006-05-17', TRUE);
//必须提供所有数据
INSERT INTO user_list
VALUES (NULL, 'test', '2006-05-17', TRUE);
//可以同插入多条数据
INSERT INTO users (username, email, birthdate, is_active)
VALUES
    ('test1', 'test1@runoob.com', '1985-07-10', true),
    ('test2', 'test2@runoob.com', '1988-11-25', false),
    ('test3', 'test3@runoob.com', '1993-05-03', true);

//查询数据库
SELECT name, is_auth FROM user_list;
SELECT * FROM user_list;
-- 添加 ORDER BY 子句，按照某列的升序排序
SELECT * FROM users ORDER BY birthdate;
-- 添加 ORDER BY 子句，按照某列的降序排序
SELECT * FROM users ORDER BY birthdate DESC;
-- 添加 LIMIT 子句，限制返回的行数
SELECT * FROM users LIMIT 10;
//ASC 表示升序（默认），DESC 表示降序。
-- 使用 AND 运算符和通配符
SELECT * FROM users WHERE username LIKE 'j%' AND is_active = TRUE;
-- 使用 OR 运算符
SELECT * FROM users WHERE is_active = TRUE OR birthdate < '1990-01-01';
-- 使用 IN 子句
SELECT * FROM users WHERE birthdate IN ('1990-01-01', '1992-03-15', '1993-05-03');

//更新数据
update user_list set name = 'wzh' where name = 'aobara';

UPDATE customers
SET total_purchases = (
    SELECT SUM(amount)
    FROM orders
    WHERE orders.customer_id = customers.customer_id
)
WHERE customer_type = 'Premium';

//删除数据
DELETE FROM students
WHERE graduation_year = 2021;

DELETE FROM students；

//UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合，并去除重复的行。
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
//不剔除重复项
SELECT country, name FROM Websites
WHERE country='CN'UNION ALL
SELECT country, app_name FROM apps
WHERE country='CN'
ORDER BY country;

//排序
多列排序：
SELECT * FROM employees
ORDER BY department_id ASC, hire_date DESC;

使用数字表示列的位置：
SELECT first_name, last_name, salary
FROM employees
ORDER BY 3 DESC, 1 ASC;

使用表达式排序：
SELECT product_name, price * discount_rate AS discounted_price
FROM products
ORDER BY discounted_price DESC;

//分组
按客户和产品分组，统计每个客户购买每种产品的次数
SELECT customer, product, COUNT(*) AS times_bought
FROM orders
GROUP BY customer, product;

aggregate_function()：常用的聚合函数有：
COUNT()：计数
SUM()：求和
AVG()：平均值
MAX()：最大值
MIN()：最小值

//事务
START TRANSACTION;

-- 执行一些SQL语句
UPDATE accounts SET balance = balance - 100 WHERE user_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE user_id = 2;

-- 判断是否要提交还是回滚
IF (条件) THEN
    COMMIT; -- 提交事务
ELSE
    ROLLBACK; -- 回滚事务
END IF;-- 开始事务
START TRANSACTION;

-- 执行一些SQL语句
UPDATE accounts SET balance = balance - 100 WHERE user_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE user_id = 2;

-- 判断是否要提交还是回滚
IF (条件) THEN
    COMMIT; -- 提交事务
ELSE
    ROLLBACK; -- 回滚事务
END IF;

//创建外键约束
ALTER TABLE blog_article 
ADD FOREIGN KEY(user_id) REFERENCES blog_auth(id);
```
#### MySQL 常用数据类型

##### 数值类型
| 类型         | 描述                                                         |
|--------------|--------------------------------------------------------------|
| `TINYINT`    | 非常小的整数（带符号范围为 -128 到 127，无符号范围为 0 到 255） |
| `SMALLINT`   | 小整数（带符号范围为 -32,768 到 32,767，无符号范围为 0 到 65,535） |
| `MEDIUMINT`  | 中等大小的整数（带符号范围为 -8,388,608 到 8,388,607，无符号范围为 0 到 16,777,215） |
| `INT`        | 普通大小的整数（带符号范围为 -2,147,483,648 到 2,147,483,647，无符号范围为 0 到 4,294,967,295） |
| `BIGINT`     | 大整数（带符号范围为 -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807，无符号范围为 0 到 18,446,744,073,709,551,615） |
| `DECIMAL(M,D)`| 定点数，精度由用户指定（M 是总位数，D 是小数点后的位数）      |
| `FLOAT`      | 单精度浮点数                                                 |
| `DOUBLE`     | 双精度浮点数                                                 |

##### 日期和时间类型
| 类型         | 描述                                                         |
|--------------|--------------------------------------------------------------|
| `DATE`       | 'YYYY-MM-DD' 格式的日期                                       |
| `TIME`       | 'HH:MM:SS' 格式的时间                                         |
| `DATETIME`   | 'YYYY-MM-DD HH:MM:SS' 格式的日期和时间                       |
| `TIMESTAMP`  | 与 DATETIME 类似，但存储的是从 Unix 纪元以来的秒数          |
| `YEAR`       | 以两位或四位数字表示的年份                                    |

##### 字符串类型
| 类型         | 描述                                                         |
|--------------|--------------------------------------------------------------|
| `CHAR(N)`    | 固定长度字符串，N 表示字符的最大数量，默认为 1               |
| `VARCHAR(N)` | 可变长度字符串，N 表示最大长度                               |
| `TEXT`       | 用于存储大文本块                                             |
| `TINYTEXT`   | 类似于 TEXT，但最大长度仅为 255 字节                         |
| `MEDIUMTEXT` | 最大长度为 16 MB 的文本                                      |
| `LONGTEXT`   | 最大长度为 4 GB 的文本                                       |
| `BINARY`     | 类似于 CHAR，但是存储二进制字符串                             |
| `VARBINARY`  | 类似于 VARCHAR，但是存储二进制字符串                          |
| `BLOB`       | 类似于 TEXT，但是存储二进制数据                              |

##### 枚举和集合类型
| 类型         | 描述                                                         |
|--------------|--------------------------------------------------------------|
| `ENUM`       | 一个枚举类型，允许从预定义的值列表中选择一个值               |
| `SET`        | 一个集合类型，允许从预定义的值列表中选择零个或多个值         |

##### JSON 类型
| 类型         | 描述                                                         |
|--------------|--------------------------------------------------------------|
| `JSON`       | 存储 JSON 文档                                               |
#### where子语句
| 编号 | 条件类型       | 示例 SQL 语句                                                                 | 说明                                     |
|------|----------------|--------------------------------------------------------------------------------|------------------------------------------|
| 1    | 等于条件       | `SELECT * FROM users WHERE username = 'test';`                               | 查找用户名为 'test' 的用户               |
| 2    | 不等于条件     | `SELECT * FROM users WHERE username != 'runoob';`                            | 排除用户名为 'runoob' 的记录             |
| 3    | 大于条件       | `SELECT * FROM products WHERE price > 50.00;`                                | 查找价格大于 50 的商品                   |
| 4    | 小于条件       | `SELECT * FROM orders WHERE order_date < '2023-01-01';`                     | 查找早于 2023 年的订单                   |
| 5    | 大于等于条件   | `SELECT * FROM employees WHERE salary >= 50000;`                             | 查找工资不低于 50000 的员工              |
| 6    | 小于等于条件   | `SELECT * FROM students WHERE age <= 21;`                                    | 查找年龄不超过 21 岁的学生               |
| 7    | AND 组合条件   | `SELECT * FROM products WHERE category = 'Electronics' AND price > 100.00;` | 同时满足两个条件的商品                   |
| 8    | OR 组合条件    | `SELECT * FROM orders WHERE order_date >= '2023-01-01' OR total_amount > 1000.00;` | 满足任一条件的订单                       |
| 9    | 模糊匹配 LIKE  | `SELECT * FROM customers WHERE first_name LIKE 'J%';`                        | 匹配以 J 开头的名字                      |
| 10   | IN 条件        | `SELECT * FROM countries WHERE country_code IN ('US', 'CA', 'MX');`          | 匹配多个指定值中的记录                   |
| 11   | NOT 条件       | `SELECT * FROM products WHERE NOT category = 'Clothing';`                    | 排除类别为 Clothing 的商品               |
| 12   | BETWEEN 范围   | `SELECT * FROM orders WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';` | 在某个范围内的记录                         |
| 13   | IS NULL 条件   | `SELECT * FROM employees WHERE department IS NULL;`                          | 查找部门为空的员工                       |
| 14   | IS NOT NULL 条件| `SELECT * FROM customers WHERE email IS NOT NULL;`                           | 查找邮箱不为空的客户                     |
LIKE 支持的通配符：  
%   匹配任意数量的字符（包括零个）  
_   匹配一个单一字符  
e.g. '_r%' 匹配第二个字母为 'r' 的任何字符串。  
#### 连接的使用
##### INNER JOIN  内部联接
```sql
SELECT column1, column2, ...
FROM table1
INNER JOIN table2 ON table1.column_name = table2.column_name;
```
##### LEFT JOIN  左联接
LEFT JOIN 返回左表的所有行，并包括右表中匹配的行，如果右表中没有匹配的行，将返回 NULL 值  
```sql
SELECT column1, column2, ...
FROM table1
LEFT JOIN table2 ON table1.column_name = table2.column_name;
```
##### RIGHT JOIN  右联接
RIGHT JOIN 返回右表的所有行，并包括左表中匹配的行，如果左表中没有匹配的行，将返回 NULL 值  
```sql
SELECT customers.customer_id, orders.order_id
FROM customers
RIGHT JOIN orders ON customers.customer_id = orders.customer_id;
```
#### 复制表
##### 复制表结构 + 数据(不会复制原表的主键、索引、自增属性等)
```sql
create table `copy_name` as select * from ori_list;
```
##### 复制表结构并保留原表的约束和索引（用于完整备份）
```sql
create table `copy_name` like `ori_name`;
insert into `copy_name` select * from `ori_name`;
```
#### 处理重复数据
##### 查询重复数据
```sql
DELETE w1
FROM wzh w1
JOIN wzh w2
ON w1.title = w2.title
WHERE w1.id > w2.id;
```
##### 使用临时表备份 + 替换原表
```sql
-- 创建一个临时表，保存去重后的数据
CREATE TABLE wzh_temp AS
SELECT *
FROM wzh
GROUP BY title;

-- 删除原表
DROP TABLE wzh;

-- 将临时表重命名为原表名
RENAME TABLE wzh_temp TO wzh;
```
##### 使用自连接删除重复数据
```sql
DELETE w1
FROM wzh w1
JOIN wzh w2
ON w1.title = w2.title
WHERE w1.id > w2.id;
```
#### 导出
```sql
SELECT id, name, email
INTO OUTFILE '/tmp/user_data.csv'
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
FROM users;
```
#### 导入
```sql
mysqlimport -u root -p --local mytbl dump.txt

LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl;

create database abc;
use abc; 
set names utf8;
source /home/abc/abc.sql
```
## axios
## echart
## animeJS
## ElementPlus
## nodeJS
### 创建一个http服务器
```js
const http = require('http');

http.createServer(function(req,res) {
    res.writeHead(200, {'Content-Type': 'text/plain'} );
    res.end("healloworld\n");
}).listen(3000);

console.log('Server running at http://127.0.0.1:3000/');
````
#### HTTP 状态码
- `1xx（信息性状态码）`：表示接收的请求正在处理。
- `2xx（成功状态码）`：表示请求正常处理完毕。
- `3xx（重定向状态码）`：需要后续操作才能完成这一请求。
- `4xx（客户端错误状态码）`：表示请求包含语法错误或无法完成。
- `5xx（服务器错误状态码）`：服务器在处理请求的过程中发生了错误。
#### promise
```js
const mypromise = new Promise((resolve,reject)=>{
    const success = true;
    if(success) {
        resolve('ok!\n');
    } else {
        reject('no\n');
    }
});

mypromise
.then((result) => {
    console.log(result);
})
.catch((error)=>{
    console.log(error);
});


function asyncOperation1() {
  return new Promise((resolve) => {
    setTimeout(() => resolve('第一步完成'), 1000);
  });
}

function asyncOperation2(data) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(`${data}, 第二步完成`), 1000);
  });
}

asyncOperation1()
  .then((result) => asyncOperation2(result))
  .then((finalResult) => {
    console.log(finalResult); // 输出："第一步完成, 第二步完成"
  })
  .catch((error) => {
    console.error('链式中出错:', error);
  });

//promise.all()
//等待所有 Promise 完成，或任意一个 Promise 失败：
const promise1 = Promise.resolve('第一个');
const promise2 = Promise.resolve('第二个');

Promise.all([promise1, promise2])
  .then((results) => {
    console.log(results); // 输出：['第一个', '第二个']
  });

//Promise.race()
//返回最先完成或失败的 Promise
const promise1 = new Promise((resolve) => setTimeout(resolve, 500, '第一个'));
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, '第二个'));

Promise.race([promise1, promise2])
  .then((result) => {
    console.log(result); // 输出："第二个"
  });
```
#### async/await
```js
async function getUserAndPosts(userId) {
  try {
    const user = await User.findById(userId);
    const posts = await Post.find({ userId });
    return { user, posts };
  } catch (error) {
    console.error('Database error:', error);
    throw error;
  }
}
````
### EventEmitter
```js
var events = require('events'); 
var emitter = new events.EventEmitter(); 
emitter.on('someEvent', function(arg1, arg2) { 
        console.log('listener1', arg1, arg2); 
}); 
emitter.on('someEvent', function(arg1, arg2) { 
        console.log('listener2', arg1, arg2); 
}); 
emitter.emit('someEvent', 'arg1 参数', 'arg2 参数');
````

| 方法名                  | 参数说明                                      | 描述 |
|-----------------------|---------------------------------------------|------|
| on(event, listener)    | event: 事件名；listener: 回调函数              | 绑定一个事件监听器（多次触发时会多次执行） |
| once(event, listener)  | event: 事件名；listener: 回调函数              | 绑定一个只执行一次的事件监听器 |
| emit(event[, ...args]) | event: 要触发的事件名；...args: 传给监听器的参数 | 触发指定事件，并传递参数给监听器 |
| off(event, listener)   | event: 事件名；listener: 要移除的回调函数       | 移除指定事件的一个监听器（与 on 对应） |
| removeAllListeners([event]) | event（可选）: 指定要移除监听器的事件名     | 移除所有或某个事件的所有监听器 |
| setMaxListeners(n)     | n: 最大监听数（默认是10）                     | 设置可以添加的监听器最大数量，避免内存泄漏 |
| getMaxListeners()      | 无                                          | 获取当前最大监听器数量限制 |
| listeners(event)       | event: 事件名                                | 返回指定事件的所有监听器数组 |
| prependListener(event, listener) | event: 事件名；listener: 回调函数        | 添加监听器到监听器队列的最前面 |
| prependOnceListener(event, listener) | event: 事件名；listener: 回调函数    | 添加一次性监听器到队列最前面 |
#### error 事件
公用事件  
```js
emitter.emit('error'); 
````
#### 继承 EventEmitter
```js
class MyEmitter extends EventEmitter {}
const customEmitter = new MyEmitter();

customEmitter.on('customEvent', () => {
  console.log('Custom event fired');
});

customEmitter.emit('customEvent');
````
### 模块导出
```js
// hello.js 
function Hello() { 
        var name; 
        this.setName = function(thyName) { 
                name = thyName; 
        }; 
        this.sayHello = function() { 
                console.log('Hello ' + name); 
        }; 
}; 
module.exports = Hello;
// main.js
var Hello = require('./hello');

//ES 模块
// myModule.mjs
export function greet(name) {
  return `Hello, ${name}!`;
}
// main.mjs
import { greet } from './myModule.mjs';
console.log(greet('Bob')); // 输出：Hello, Bob!


//调用
var server = require("./server");
var router = require("./router");
server.start(router.route);
```
### 路由
```js
const http = require('http');

// 创建服务器并定义路由
const server = http.createServer((req, res) => {
  const { url, method } = req;

  if (url === '/' && method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Home Page');
  } else if (url === '/about' && method === 'GET') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('About Page');
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('404 Not Found');
  }
});

server.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
```
#### 请求参数
```js
const myUrl = new URL("http://localhost:8888/start?foo=bar&hello=world");
// 提取路径名
console.log(myUrl.pathname); // 输出: /start
// 提取查询参数
console.log(myUrl.searchParams.get("foo"));   // 输出: bar
console.log(myUrl.searchParams.get("hello")); // 输出: world
```
#### 使用express
```js
const express = require('express');
const app = express();
const port = 3000;
// 创建一个路由器实例
const userRouter = express.Router();
// 定义用户相关的路由
userRouter.get('/', (req, res) => {
    res.send('List of users');
});
userRouter.get('/:id', (req, res) => {
    const userId = req.params.id;
    res.send(`User ID: ${userId}`);
});
// 挂载用户路由器
app.use('/users', userRouter);
// 启动服务器
app.listen(port, () => {
    console.log(`Server is running on http://localhost:${port}`);
});
```
#### 使用cors处理跨域请求
```js
const cors = require('cors');
app.use(cors());
```
#### 使用mysql2来连接
```js
const mysql = require('mysql2');
const pool = mysql.createPool({
  host: 'localhost',
  user: '...',
  password: '...',
  database: 'test_db',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});
const query = (sql, values) => {
  return new Promise((resolve, reject) => {
    pool.getConnection((err, connection) => {
      if (err) return reject(err);
      connection.query(sql, values, (error, results) => {
        connection.release();
        if (error) return reject(error);
        resolve(results);
      });
    });
  });
};

app.post('/api/register', async (req, res) => {
  const { username, password } = req.body;
  if (!username || !password) {
    return res.status(400).json({ message: '用户名和密码不能为空' });
  }
  try {
    const existingUser = await query('SELECT * FROM users WHERE username = ?', [username]);
    if (existingUser.length > 0) {
      return res.status(400).json({ message: '用户名已存在' });
    }
    const hashedPassword = await bcrypt.hash(password, 10);
    await query('INSERT INTO users (username, password) VALUES (?, ?)', [username, hashedPassword]);
    res.status(201).json({ message: '注册成功' });
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: '服务器内部错误' });
  }
});
```
### route vue 路由
```js
import { createRouter, createWebHistory } from 'vue-router'
import LoginPage from '../components/login-page.vue'
import Register from '../components/redgister.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      redirect: '/login'
    },
    {
      path: '/login',
      name: 'Login',
      component: LoginPage
    },
    {
      path: '/register',
      name: 'Register',
      component: Register
    }
  ]
})

export default router
```
## jQuery
## swiper(轮播图)
```html
<template>
  <div class="swiper-container">
    <swiper
      :modules="modules"
      :slides-per-view="1"
      :space-between="50"
      navigation
      :pagination="{ clickable: true }"
      @swiper="onSwiper"
      @slideChange="onSlideChange"
    >
      <swiper-slide><img src="@/assets/img/thumb1.jpg" alt="Image Slide"></swiper-slide>
      <swiper-slide><img src="@/assets/img/thumb2.jpg" alt="Image Slide"></swiper-slide>
      <swiper-slide><img src="@/assets/img/thumb3.jpg" alt="Image Slide"></swiper-slide>
    </swiper>
  </div>
</template>

<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation, Pagination } from 'swiper/modules'

import 'swiper/css'
import 'swiper/css/navigation'
import 'swiper/css/pagination'

const modules = [Navigation, Pagination]

const onSwiper = (swiper) => {
  console.log('Swiper instance:', swiper)
}

const onSlideChange = () => {
  console.log('Slide changed')
}
</script>

<style scoped>
.swiper-container {
  width: 100%;
  height: 300px;
}

.swiper-slide {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  background-color: #f0f0f0;
  text-align: center;
}

.swiper-slide img {
  width: 100%;
  height: auto;
}
</style>
````
## uniapp
## 微信小程序