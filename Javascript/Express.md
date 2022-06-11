# Express

## Middleware

### Request Processing Pipline

```mermaid
graph LR
1((Request))
2[Middleware]
3[Middleware]
1 --> 2 --> 3
```

### Custom Middleware

`next`代表下一个中间件。

中间件是按需调用的。

一般写在独立文件。

`/logger.js`

```javascript
function log(req, res, next){
  console.log('Logging...');
  next();
};

modlue.exports = log;
```

### Built-in Middleware

* `express.json()`：将body中的JSON转换成Object, 设置req.body
* `express.urlencoded()`: 读取url的queries, 设置req.body
* `express.static('public')`：提供静态内容

### Third-party Middleware

* `helmet`
* `morga`: 日志

```javascript
var helmet = require('helmet');
var morgan = require('morgan');

app.use(helmet());
app.use(morgan('tiny'));
```

```shell
yarn add helmet
```

### 环境、配置

```javascript
process.env.NODE_ENV  // 可能未定义
app.get('env')  // 默认development
```

#### config

```javascript
config.get('name')
```

### Debug

```shell
npm i debug
```

```javascript
const startupDebugger = require('debug')('app:startup');  // 命名空间

startupDebugger("...");
```

```shell
export DEBUG=app:startup, app:db
export DEBUG=app:*  # all
DEBUG=app:startup node index.js
```

### 模版引擎

### 数据库集成

### Structuring Project

```javascript
app.use('/api/courses', courses);
```

## 异步

异步不是多线程

```javascript
console.log("before");
setTimeout(() => {
  console.log("Reading from  db");
}, 2000);
console.log("After");
```

### Async Pattern

**不能**像这样得到结果：

```javascript
console.log("before");
const user = getUser(1);  // undefined
console.log("After");

// 函数无返回
function getUser(id) {
  setTimeout(() => {
    console.log("Reading from  db");
    return { id: id, name: "mosh" };
  }, 2000);
}
```

3 patterns：

* Callbacks
* Promises
* Async/await

### Callbacks

```javascript
console.log("before");
getUser(1, (user) => {
  console.log("User", user);
});
console.log("After");

function getUser(id, callback) {
  setTimeout(() => {
    console.log("Reading from  db");
    callback({ id: id, name: "mosh" });  // 把return换成callback
  }, 2000);
}
```

再加入一个

```javascript
console.log("before");
getUser(1, (user) => {
  console.log("User", user);
  getRepositories(user.name, (repos) => {
    console.log(repos);
  });
});
console.log("After");

function getUser(id, callback) {
  setTimeout(() => {
    console.log("Reading from db ...");
    callback({ id: id, name: "mosh" });
  }, 2000);
}

function getRepositories(username, callback) {
  setTimeout(() => {
    console.log("Reading repo ...");
    console.log(username);
    callback(["repo1", "repo2", "repo3"]);
  }, 2000);
}
```

再加入一个

```javascript
console.log("before");
getUser(1, (user) => {
  console.log("User", user);
  getRepositories(user.name, (repos) => {
    console.log(repos);
    getCommits(repos[0], (comits) => {
      console.log(comits);
    });
  });
});
console.log("After");

function getUser(id, callback) {
  setTimeout(() => {
    console.log("Reading from db ...");
    callback({ id: id, name: "mosh" });
  }, 2000);
}

function getRepositories(username, callback) {
  setTimeout(() => {
    console.log("Reading repo ...");
    console.log(username);
    callback(["repo1", "repo2", "repo3"]);
  }, 2000);
}

function getCommits(repo, callback) {
  setTimeout(() => {
    console.log("Reading commits...");
    callback(["commit1", "commit2"]);
  }, 2000);
}
```



#### 解决回调嵌套：Named function

```javascript
console.log("before");
getUser(1, displayUser);
console.log("After");

function getUser(id, callback) {
  setTimeout(() => {
    console.log("Reading from db ...");
    callback({ id: id, name: "mosh" });
  }, 2000);
}

function getRepositories(username, callback) {
  setTimeout(() => {
    console.log("Reading repo ...");
    callback(["repo1", "repo2", "repo3"]);
  }, 2000);
}

function getCommits(repo, callback) {
  setTimeout(() => {
    console.log("Reading commits...");
    callback(["commit1", "commit2"]);
  }, 2000);
}

function displayCommits(commits) {
  console.log(commits);
}

function displayRepos(repos) {
  console.log(repos);
  getCommits(repos[0], displayCommits);
}

function displayUser(user) {
  console.log(user);
  getRepositories(user.name, displayRepos);
}
```

按签名把匿名函数换成具名函数

### Promises

Promise有三个状态：

1. Pending
2. Fulfilled
3. Rejected

Resolved:

```javascript
const p = new Promise((resolve, reject) => {
  // Async work
  setTimeout(() => {
    resolve(1);
  }, 2000);
  // reject(new Error('...'));
});

p.then((result) => {
  console.log(result);
});
```

Rejected

```javascript
const p = new Promise((resolve, reject) => {
  // Async work
  setTimeout(() => {
    // resolve(1);
    reject(new Error("..."));
  }, 2000);
});

p.then((result) => console.log(result)).catch((err) => console.log(err));
```

#### Replace Callback with Promise

```javascript
console.log("before");

// getUser(1, (user) => {
//   console.log("User", user);
//   getRepositories(user.name, (repos) => {
//     console.log(repos);
//     getCommits(repos[0], (comits) => {
//       console.log(comits);
//     });
//   });
// });

getUser(1)
  .then((user) => getRepositories(user.name))
  .then((repos) => getCommits(repos[0]))
  .then((commits) => console.log(commits));

console.log("After");

function getUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Reading from db ...");
      resolve({ id: id, name: "mosh" });
    }, 2000);
  });
}

function getRepositories(username) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Reading repo ...");
      console.log(username);
      resolve(["repo1", "repo2", "repo3"]);
    }, 2000);
  });
}

function getCommits(repo) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Reading commits...");
      resolve(["commit1", "commit2"]);
    }, 2000);
  });
}

```

#### Promise API

```javascript
const p = Promise.resolve({ id: 1 });
p.then((result) => console.log(result));
```

#### 并行Promise

只要有一个Promises出错就不会返回

```javascript
Promise.all([p1, p2])
  .then((result) => console.log(result));
```

```javascript
const p1 = new Promise((resolve) => {
  setTimeout(() => {
    console.log("Async Operation 1");
    resolve(1);
  }, 2000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => {
    console.log("Async Operation 2");
    resolve(2);
  }, 2000);
});

Promise.all([p1, p2]).then((result) => console.log(result));
```

### Async & Await

只要函数返回`Promise`，就可以await

只能在async函数中await

```javascript
console.log("before");

// getUser(1, (user) => {
//   console.log("User", user);
//   getRepositories(user.name, (repos) => {
//     console.log(repos);
//     getCommits(repos[0], (comits) => {
//       console.log(comits);
//     });
//   });
// });

// getUser(1)
//   .then((user) => getRepositories(user.name))
//   .then((repos) => getCommits(repos[0]))
//   .then((commits) => console.log(commits));

async function displayCommits() {
  try {
    const user = await getUser(1);
    const repos = await getRepositories(user.name);
    const commits = await getCommits(repos[0]);
    console.log(commits);
  } catch (err) {
    console.log(err);
  }
}

displayCommits();
console.log("After");

function getUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Reading from db ...");
      resolve({ id: id, name: "mosh" });
    }, 2000);
  });
}

function getRepositories(username) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Reading repo ...");
      console.log(username);
      resolve(["repo1", "repo2", "repo3"]);
    }, 2000);
  });
}

function getCommits(repo) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Reading commits...");
      resolve(["commit1", "commit2"]);
    }, 2000);
  });
}

```

## Mongoose

### 连接

```JavaScript 
mongoose
  .connect("mongodb://playground-owner:123456@120.55.83.216:27017/playground", {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  })
  .then(() => console.log("Connected"))
  .catch((err) => console.log(err));

```



### Schema & Modol

Schema ：Document的定义

```JavaScript
const courseSchema = new mongoose.Schema({
  name: String,
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean,
});
```

Model：由Schema编译成类

```javascript
const Course = mongoose.model("Course", courseSchema);
// Course是表名
```

实例

```javascript
const course = new Course({
  name: "Node.js Course",
  author: "mosh",
  tags: ["Node", "Backend"],
  isPublished: true,
});

const result = await course.save();
console.log(result);
```

### 查询

```javascript
async function getCourses() {
  const courses = await Course
  	.find({ author: "mosh", isPublished: true })
    .limit(10)
    .sort({ name: 1 })
    .select({ name: 1, tags: 1 });
  console.log(courses);
}
```

#### Comparison Operator

* eq
* ne
* gt
* gte
* lt
* lte
* in
* nin

```javascript
find({price: {$gt: 10}})
```

#### Logical Operator

`.find().or([{author: "mosh"}, {isPublished: true}])`

#### Regular Expression

* Starts with Mosh

  `/^Mosh/`

* Ends with Hamedani，不区分大小写

  `/Hamedani$/i`

* 任意字符



### Validator

只对mongoose有意义

`await course.save()`会有异常

#### built-in

* `require`

  ```
  {
  	price: {
  		type: Number,
  		required: function() {return this.isPublished;}
  	}
  }
  ```

* `minlength/maxlength`

* `enum`

* `match`

#### Custom

```json
tags: {
  type: Arrray,
  validate: {
    validator: function(v) {
      return v && v.length > 0;
    },
    message: 'A course should have at least 1 tag'
  }
}
```

#### Async Validator

```json
tags: {
  isAsync: true,
  type: Arrray,
  validate: {
    validator: function(v, callback) {
			// Do some async work
      setTimeout(() => {
        const result = v && v.length > 0;
        callback(result);
      }, 1000);
    },
    message: 'A course should have at least 1 tag'
  }
}
```

#### Validation Errors

```javascript
try {
  const result = await course.save();
} catch (ex) {
	for (field in ex.errors) {
    console.log(ex.errors[field].message)
  }
}
```

## 模型关系

1. 引用（范式） -> 一致性
2. 嵌入文档（非范式） ->效率

### reference

```javascript
{
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Author"
  }
}
```



```javascript
Course
  .find()
	.populate('author'， 'name-_id')  // 依据ref，填充author属性，只看name
```

