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
      cookie: '',
      // 是否将请求转发到本地, 默认为true, 如果打开了此选项但由于本地没有匹配到数据,则还是会去请求服务器上的数据
      open: true,
      // 延时返回,不设置则使用默认
	  delay: () => {
		return Math.random() * 2 * 10000
	  },
	  // 需要转发到的服务器地址和端口
	  testServer: 'http://www.xx.com:808',
    },
    request: {
      // 可以返回一个函数,字符串,数字,数组, 如果返回的是"数据对象",那么里面不能出现ok,active字段,否则会被当做是一个配置项从而报错
      // 路径使用正则来匹配的
      '/api/queryUserInfo': function(Mock, postData, formData) {
            let result = null;

            switch (postData.id) {
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
          // 使用哪个项作为返回的结果, 默认返回ok项
          active: 'test',
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
