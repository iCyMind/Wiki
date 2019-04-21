# Serverless Framework

## pre
- `npm i serverless -g`
- configure aws credentials
    - aws console => IAM
    - new user => serverless-cli
    - access type => programatic access
    - attach existing policies directly => administratoraccess (创建完成后, serverless 会自动去掉一些权限)
    - download credentials.csv (you will never see it on aws again)
    - `sls config credentials --provider aws --key oxoxoxox --secret oxoxox`

## create project
- `sls create --help`
- `sls create --template aws-nodejs`
- change serverless.yml
    - service => awesome-api
    - runtime => nodejs8.10
    - add provider.memorySize => 128
    - stage => dev/prod
    - ap-northeast-1 (这是东京)

## deploy

- `sls deploy`
- check on cloudfomation
- check it on lambda

## execute
- execute on cloud: `sls invoke --function hello`
- execute local `sls invoke local --function hello`

## integrating with API gateway
- add functions.hello.events => http
- 在 http 下添加 path 和 method 属性, 记得缩进, 否则框架会报错
- 部署后, 在 chrome 里访问对应的 endpoints, 可以看到对应的输出

## 处理 input
- GET method
    - 从 endpoint 传进来的参数可以通过 event.queryStringParameters 访问
- POST method
    - 通过 postman 发送数据
    - 设置 body 为 raw - json
    - 可以在 event.body 中看到响应的数据
    - 通过 json.parse(event.body) 即可

## 同时部署其他 aws 资源
- 部署 lambda 的同时, 可以部署 s3 或者 dynamoDB 等资源
- 资源的名称可以利用 yml 中定义的变量, 如 `${service}-${provider.stage}-upload`
- 资源的写法可以看 aws 文档 - cloudfomation - template reference - AWS resource type

## 环境变量
- 把 API key, 数据库用户密码等变量存储在 .yml 文件, 可以避免修改 lambda 函数
- 两种设置环境变量的方法:
    - 项目级设置: provider.environments
    - lambda 级设置: functions.hello.environments
- 在 lambda 里可以通过 process.env.GOOGLE_MAPS_API 访问对应的变量
