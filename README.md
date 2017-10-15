function
---

- 拦截ajax,fetch请求,模拟返回后端数据
- 支持debug
- 根据参数返回不同的数据
- 支持Mock数据
- 方便移动端调式
- 支持延时返回数据
- 支持设置cookie

Install
---
```$js
npm install analog-data
```
Example
---
1.install
```js
npm install analog-data --save
```
2.引入analog-ajax脚本

```$ja
<script src="node_modules/analog-data/dist/analog-ajax.js"></script>
```
注意必须放在其他脚本的前面,不然无法拦截ajax请求

3.编写配置文件,如:

api.js
```js
module.exports = {
    config: {
      // 全局cookie,当item设置了cookie,则覆盖此cookie
      'cookie': '',
      // 控制总开关, 默认为true
      'open': true,
    },
    request: {
      // 需要匹配的路径
      // 可以返回一个函数,字符串,数字,数组, 如果返回的是"数据对象",那么里面不能出现ok,active字段,否则会被当做是一个配置项从而报错
      '/api/queryUserInfo': (request, data, Mock) => {
            let result = null;

            switch (data.id) {
                case 0:
                    result = {
                   		status: 'ok',
                      	data: []
                    };
                    break;
                case 1:
                    result = result = {
                   		status: 'error',
                      	data: []
                    };;
                    break;
            }

            return result;
       },
      '/test': {
          // 是否拦截本条请求,默认为true
          open: false,
          cookie: '',
          // 使用哪个项作为返回的结果, 默认返回ok项
          active: 'test',
          // 延时返回
          delay: 1000,
          test: (request, data, Mock) => {
              return Mock.mock({
                  'data|0-10': [{
                      name: '@name',
                      email: '@email',
                      'age|1-10': ['112312312']
                  }]
              });
          },
          ok: () => {
              return {
                  status: 'ok',
                  data: [{a:11}]
              };
          },
          // 使用json文件作为返回结果
          file: '/Users/xx/code/xx/project.config.json'
      },
      '/user': (request, data, Mock) => {
          return Mock.mock({
             'data|1-5': [{
                 'id|+1': 1
             }]
          });
      },
      '/id': 1
    }
};
```
request: 发送请求的头信息

data: 发送请求的数据

Mock: 可以方便模拟假数据,教程http://mockjs.com/examples.html



4.启动拦截服务 
node-dev analog-data api.js || node node_modules/analog-data api.js

// api.js: 相对路径

5.debug

http://localhost:9870