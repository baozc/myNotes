## spring.profiles.active
1. 可以指定多个配置文件，方便开发测试时自动切换
    1. 指定`application.yaml`、`application-local.yaml`、`application-test.yaml`
    2. 在`application.yaml`中配置`spring.profiles.active`来指定运行时使用哪个配置文件，可选值`local`/`test`
2. 关于`spring.profiles.active`加载顺序
  1. 优先查找`spring.profiles.active`指定的配置文件
  2. 如果没有找到再查找`application.yaml`
  3. 及`active`指定 > `application.yaml`
3. 如果在命令行使用`--spring.profiles.active`选项时，命令行指定的优先
  - `命令行`优先级大于`配置文件`

## 命令行指定参数
1. maven运行时，命令行指定参数
  - 使用`mvn spring-boot:run -Drun.arguments="--spring.profiles.active=test"`
2. 执行jar包时，命令行指定参数
  - 使用`java -jar target/xxx.jar --spring.profiles.active=test`

> 在`application.yaml`中使用的参数，都可以用`--参数`的形式来指定
