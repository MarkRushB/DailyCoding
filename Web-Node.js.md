# Node.js

- 为什么要学Node.js
    - 企业需求：  
        1. 具有服务端开发经验更好
        2. front-end
        3. back-end
        4. 全栈开发工程师（全干）
        5. 基本的网站开发能力
            - 服务端
            - 前端
            - 运维部署 
- Node.js是什么
    - Node.js不是一门语言
    - Node.js不是库、不是框架
    - Node.js是一个JavaScript运行时的环境
    - 简单来讲就是Node.js能解析和执行JS代码，以前只有浏览器可以解析执行JS代码，但是现在JS完全可以脱离浏览器运行，一切都归功于Node.js
    - Node.js中的JS
        - **没有BOM、DDOM**
        - EcmaScript
        - 在Node这个JS执行环境中为JS提供了一些服务器级别的操作API
            - 例如文件读写
            - 网络服务的构建
            - 网络通信
- Node.js能做什么
    - Web服务器后台
    - 命令行工具
        - npm(node)
        - git(c)
        - hexo(node)
        - ....
    - 对于前端开发工程师来讲，接触node最多的就是它的命令行工具
        - 自己写的很少，主要是使用别人第三方的
        - webpack
        - gulp
        - npm


- Hello World
    1. 创建编写JS脚本文件
    2. 打开终端，定位到脚本文件所属目录
    3. 输入`node 文件名`执行对应文件  
    **注意：文件名不能使用node.js命名**

    - 解析执行JavaScript
    - 读写文件
        - Node中的JS具有文件操作的能力
        - 在Node中如果想进行文件操作，必须引入fs(file-system)这个核心模块，在这个核心模块中，就提供了所有文件操作相关的API
        - 读文件
            ```javascript
            // 1. 使用require这个方法家在fs核心模块
            var fs = require('fs')
            
            // 2.  读取文件
            //      第一个参数就是要读取的文件路径
            //      第二个参数就是一个回调函数
            //          成功
            //              data 数据
            //              error null
            //          失败
            //              data null
            //              error 错误对象
            fs.readFile('./data/hello.txt', function(error, data){

            })
            ```
        - 写文件
            ```javascript
            var fs = require('fs')

            // 第一个参数：文件路径
            // 第二个参数： 文件内容
            // 第三个参数： 回调函数
            // error

            // 成功：
            //      文件写入成功
            //      error是null
            // 失败：
            //      文件写入失败
            //      error就是错误对象

            fs.writeFile('./data/你好.md', '大家好，给大家介绍一下，我是Node.js', function(error){

            })
            ```
    - http
        - 使用Node非常轻松的构建一个Web服务器
        - 在Node中专门提供了一个核心模块：http
        - 这个模块的职责就是帮你创建编写服务器的
        1. 加载http核心模块
        ```javascript
        var http = require('http')
        ```
        2. 使用http.createServer()方法创建一个Web服务器
        返回一个Server实例
        ```javascript
        var server = http.createServer()
        ```
        3. 服务器要干嘛？  
        提供服务：对数据的服务  
        发请求  
        接受请求  
        处理请求  
        给个反馈（发送响应）  
        注册request请求事件  
        当客户端请求过来，就会自动触发服务器的request请求事件，然后执行第二个参数：回调处理
        ```javascript
        server.on('request', funciton(){
            console.log('收到客户端的请求了')
        })
        ```
        4. 绑定端口号，启动服务器
        ```javascript
        server.listen(3000, function(){
            console.log('服务器启动成功了，可以通过http://127.0.0.1:3000/ 来访问')
        })
        ```

        ```javascript
        var http = require('http')

        var server = http.createServer()
        // request请求事件处理函数，需要接受两个参数：
        //  Request 请求对象
        //      请求对象可以获取客户端的一些请求信心，例如请求路径
        //  Response 相应对象
        //      相应对象可以用来给客户端发送响应消息
        server.on('request', funciton(request, response){
            console.log('收到客户端的请求了，请求路径是：' + request.url)
        // response对象有一个方法：write可以用来给客户端发送相应数据 
        // write可以使用多次，但是最后一定要使用end来结束相应，否则客户端会一直等待
            response.write('hello')
            response.write('hello')
        // 告诉客户端，我的话说完了，你可以呈递给用户了
            response.end()
        // 或者发送响应的同时直接end
            response.end('hello world')
        })
       
        server.listen(3000, function(){
            console.log('服务器启动成功了，可以通过http://127.0.0.1:3000/ 来访问')
        })
        ```
    - 模块化编程  
        require是一个方法，作用就是用来加载模块的。    
        用户自己编写的其实也是模块

        
