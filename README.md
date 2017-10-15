function
---

- 拦截ajax,fetch请求,模拟返回后端数据
- 支持可视化查看请求信息
- 支持根据参数返回不同的数据
- 支持Mock数据
- 方便移动端调式
- 支持延时返回数据
- 支持设置cookie

支持webpack编译,如果没有设置webpack配置文件路径则不会进行webpack编译

例子
---

1.安装
```js
npm install analog-data-webpack --save
```

2.编写配置文件,如:

api.js
```js
module.exports = {
    config: {
      // cookie
      'cookie': '',
      // 控制总开关, 默认为true
      'open': true,
    },
    request: {
      // 可以返回一个函数,字符串,数字,数组, 如果返回的是"数据对象",那么里面不能出现ok,active字段,否则会被当做是一个配置项从而报错
      // 路径使用正则来匹配的
      '/api/queryUserInfo': function(Mock, postData, formData) {
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
          // 使用哪个项作为返回的结果, 默认返回ok项
          active: 'test',
          // 延时返回
          delay: 1000,
          test: function(Mock, postData, formData) {
              return Mock.mock({
                  'data|0-10': [{
                      name: '@name',
                      email: '@email',
                      'age|1-10': ['112312312']
                  }]
              });
          },
          ok: function() {
              return {
                  status: 'ok',
                  data: [{a:11}]
              };
          },
          // 使用json文件作为返回结果
          file: '/Users/xx/code/xx/project.config.json'
      },
      '/user': function(Mock, postData, formData) {
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

上下文
---

this.requestInfo: url,method信息

this.query: url query

this.postData: post请求的参数

this.formData: post formData参数

this.resultData: 请求返回的数据


3.启动服务 
node-dev analog-data-webpack [port, apiPath, webapckConfigPath]

默认:

port: 9870

- node-dev analog-data-webpack 8080

apiPath: api.js

- node-dev analog-data-webpack api.js

webpackConfigPath: webpack.dev.config.js

- node-dev analog-data-webpack api.js webpack.dev1.config.js


4.查看请求信息

http://localhost:9870/debug


其他
---
Mock: 可以方便模拟假数据,教程http://mockjs.com/examples.html
