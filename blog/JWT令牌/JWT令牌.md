@[TOC]

# 一、JWT令牌的介绍

前面我们介绍了基于令牌技术来实现会话追踪。这里所提到的令牌就是用户身份的标识，其本质就是一个字符串。令牌的形式有很多，我们使用的是功能强大的 JWT令牌。

## 

JWT全称：JSON Web Token  （官网：https://jwt.io/）

- 定义了一种简洁的、自包含的格式，用于在通信双方以json数据格式安全的传输信息。由于数字签名的存在，这些信息是可靠的。

  > 简洁：是指jwt就是一个简单的字符串。可以在请求参数或者是请求头当中直接传递。
  >
  > 自包含：指的是jwt令牌，看似是一个随机的字符串，但是我们是可以根据自身的需求在jwt令牌中存储自定义的数据内容。如：可以直接在jwt令牌中存储用户的相关信息。
  >
  > 简单来讲，jwt就是将原始的json数据格式进行了安全的封装，这样就可以直接基于jwt在通信双方安全的进行信息传输了。



JWT的组成： （JWT令牌由三个部分组成，三个部分之间使用英文的点来分割）

- 第一部分：Header(头）， 记录令牌类型、签名算法等。 例如：{"alg":"HS256"（签名算法为HS256，将来就会根据这段指定的签名算法对JWT令牌进行数字前面）,"type":"JWT"（令牌的类型为JWT）}

- 第二部分：Payload(有效载荷，载荷就是用来装东西的），携带一些自定义信息、默认信息等。 例如：{"id":"1","username":"Tom"、令牌的签发日期，令牌的有效期等}，原始数据依然是JSON类型的数据，也是进行了base64编码

- 第三部分：Signature(签名），防止Token被篡改、确保安全性。将header、payload，并加入指定秘钥，通过指定签名算法计算而来。（并不是64编码）

  >签名的目的就是为了防jwt令牌被篡改，而正是因为jwt令牌最后一个部分数字签名的存在，所以整个jwt 令牌是非常安全可靠的。一旦jwt令牌当中任何一个部分、任何一个字符被篡改了，整个令牌在校验的时候都会失败，所以它是非常安全可靠的。

![image-20230106085442076](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281507711.png)

> JWT是如何将原始的JSON格式数据，转变为字符串的呢？
>
> 其实在生成JWT令牌时，会对JSON格式的数据进行一次编码：进行base64编码
>
> Base64：是一种基于64个可打印的字符来表示二进制数据的编码方式（注意不是加密方式）。既然能编码，那也就意味着也能解码。所使用的64个字符分别是A到Z、a到z、 0- 9，一个加号，一个斜杠，加起来就是64个字符。任何数据经过base64编码之后，最终就会通过这64个字符来表示。当然还有一个符号，那就是等号。等号它是一个补位的符号
>
> 需要注意的是Base64是编码方式，而不是加密方式。



![image-20230112114319773](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281507940.png) 

JWT令牌最典型的应用场景就是登录认证：

1. 在浏览器发起请求来执行登录操作，此时会访问登录的接口，如果登录成功之后，我们需要生成一个jwt令牌，将生成的 jwt令牌返回给前端。
2. 前端拿到jwt令牌之后，会将jwt令牌存储起来。在后续的每一次请求中都会将jwt令牌携带到服务端。
3. 服务端统一拦截请求之后，先来判断一下这次请求有没有把令牌带过来，如果没有带过来，直接拒绝访问，如果带过来了，还要校验一下令牌是否是有效。如果有效，就直接放行进行请求的处理。



在JWT登录认证的场景中我们发现，整个流程当中涉及到两步操作：

1. 在登录成功之后，要生成令牌。
2. 每一次请求当中，要接收令牌并对令牌进行校验。

---

# 二、JWT令牌的生成和校验

简单介绍了JWT令牌以及JWT令牌的组成之后，接下来我们就来学习基于Java代码如何生成和校验JWT令牌。

首先我们先来实现JWT令牌的生成。要想使用JWT令牌，需要先引入JWT的依赖：

~~~xml
<!-- JWT依赖-->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
~~~

> 在引入完JWT来赖后，就可以调用工具包中提供的API来完成JWT令牌的生成和校验
>
> 工具类：Jwts
>
> 官网的这个地方就指定了签名算法大概有哪些
>
> ![image-20231028145751915](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281600598.png)

---

## 生成JWT代码

~~~java
@Test
public void genJwt(){
    Map<String,Object> claims = new HashMap<>();
    claims.put("id",1);
    claims.put("username","Tom");
    
    String jwt = Jwts.builder()
        .setClaims(claims) //自定义内容(载荷)，封装到map集合中          
        .signWith(SignatureAlgorithm.HS256, "itheima") //签名算法，后面还需要指定数字签名时的秘钥        
        .setExpiration(new Date(System.currentTimeMillis() + 24*3600*1000)) //有效期，new了一个Date对象后代表当前时间，   
        .compact();//获取到字符串的返回值
    
    System.out.println(jwt);
}
~~~

>  使用Base64编码字符串长度至少为43位，位数报错的可以把itheima改成任意的大于等于43位的字符串

运行测试方法：

~~~
eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk
~~~

输出的结果就是生成的JWT令牌,，通过英文的点分割对三个部分进行分割，我们可以将生成的令牌复制一下，然后打开JWT的官网，将生成的令牌直接放在Encoded位置，此时就会自动的将令牌解析出来。

![image-20230106190950305](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281600156.png)

> 第一部分解析出来，看到JSON格式的原始数据，所使用的签名算法为HS256。
>
> 第二个部分是我们自定义的数据，之前我们自定义的数据就是id，还有一个exp代表的是我们所设置的过期时间。
>
> 由于前两个部分是base64编码，所以是可以直接解码出来。但最后一个部分并不是base64编码，是经过签名算法计算出来的，所以最后一个部分是不会解析的。
>
> 我们也可以直接去网上搜索base64解码工具





实现了JWT令牌的生成，下面我们接着使用Java代码来校验JWT令牌(解析生成的令牌)：

## 校验JWT令牌

---

~~~java
@Test
public void parseJwt(){
    Claims claims = Jwts.parser()
        .setSigningKey("itheima")//指定签名密钥（必须保证和生成令牌时使用相同的签名密钥）  
        .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjcyNzI5NzMwfQ.fHi0Ub8npbyt71UqLXDdLyipptLgxBUg_mSuGJtXtBk")//JWT令牌时一个字符串，需要用引号引起来
        .getBody();

    System.out.println(claims);
}
~~~

运行测试方法：

~~~
{id=1, exp=1672729730}
~~~

> 令牌解析后，我们可以看到id和过期时间，如果在解析的过程当中没有报错，就说明解析成功了。



下面我们做一个测试：把令牌header中的数字9变为8，运行测试方法后发现报错：

> 原header： eyJhbGciOiJIUzI1NiJ9
>
> 修改为： eyJhbGciOiJIUzI1NiJ8

![image-20230106205045658](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281601379.png)

结论：篡改令牌中的任何一个字符，在对令牌进行解析时都会报错，所以JWT令牌是非常安全可靠的。



我们继续测试：修改生成令牌的时指定的过期时间，修改为1分钟

~~~java
@Test
public void genJwt(){
    Map<String,Object> claims = new HashMap<>();
    claims.put(“id”,1);
    claims.put(“username”,“Tom”);
    String jwt = Jwts.builder()
        .setClaims(claims) //自定义内容(载荷)          
        .signWith(SignatureAlgorithm.HS256, "itheima") //签名算法        
        .setExpiration(new Date(System.currentTimeMillis() + 60*1000)) //有效期60秒   
        .compact();
    
    System.out.println(jwt);
    //输出结果：eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjczMDA5NzU0fQ.RcVIR65AkGiax-ID6FjW60eLFH3tPTKdoK7UtE4A1ro
}

@Test
public void parseJwt(){
    Claims claims = Jwts.parser()
        .setSigningKey("itheima")//指定签名密钥
.parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwiZXhwIjoxNjczMDA5NzU0fQ.RcVIR65AkGiax-ID6FjW60eLFH3tPTKdoK7UtE4A1ro")
        .getBody();

    System.out.println(claims);
}
~~~

等待1分钟之后运行测试方法发现也报错了，说明：JWT令牌过期后，令牌就失效了，解析的为非法令牌。

通过以上测试，我们在使用JWT令牌时需要注意：

- JWT校验时使用的签名秘钥，必须和生成JWT令牌时使用的秘钥是配套的。

- 如果JWT令牌解析校验时报错，则说明 JWT令牌被篡改 或 失效了，令牌非法。 

---

# 三、实现登录下发令牌

JWT令牌的生成和校验的基本操作我们已经学习完了，接下来我们就需要在案例当中通过JWT令牌技术来跟踪会话。具体的思路我们前面已经分析过了，主要就是两步操作：

1. 生成令牌
   - 在登录成功之后来生成一个JWT令牌，并且把这个令牌直接返回给前端
2. 校验令牌
   - 拦截前端请求，从请求中获取到令牌，对令牌进行解析校验



那我们首先来完成：登录成功之后生成JWT令牌，并且把令牌返回给前端。



JWT令牌怎么返回给前端呢？此时我们就需要再来看一下接口文档当中关于登录接口的描述（主要看响应数据）：

- 响应数据

  参数格式：application/json

  参数说明：

  | 名称 | 类型   | 是否必须 | 默认值 | 备注                     | 其他信息 |
  | ---- | ------ | -------- | ------ | ------------------------ | -------- |
  | code | number | 必须     |        | 响应码, 1 成功 ; 0  失败 |          |
  | msg  | string | 非必须   |        | 提示信息                 |          |
  | data | string | 必须     |        | 返回的数据 , jwt令牌     |          |

  响应数据样例：

  ~~~json
  {
    "code": 1,
    "msg": "success",
    "data": "eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoi6YeR5bq4IiwiaWQiOjEsInVzZXJuYW1lIjoiamlueW9uZyIsImV4cCI6MTY2MjIwNzA0OH0.KkUc_CXJZJ8Dd063eImx4H9Ojfrr6XMJ-yVzaWCVZCo"
  }
  ~~~

- 备注说明

  用户登录成功后，系统会自动下发JWT令牌，然后在后续的每次请求中，都需要在请求头header中携带到服务端，请求头的名称为 token ，值为 登录时下发的JWT令牌。

  如果检测到用户未登录，则会返回如下固定错误信息：

  ~~~json
  {
  	"code": 0,
  	"msg": "NOT_LOGIN",
  	"data": null
  }
  ~~~

解读完接口文档中的描述了，目前我们先来完成令牌的生成和令牌的下发，我们只需要生成一个令牌返回给前端就可以了。

---

## 实现步骤

1. 将JWT所需参数抽取到配置文件中，并提供其配置类

2. 引入JWT工具类

   在项目工程下创建utils包，并把提供JWT工具类复制到该包下

3. 登录完成后，调用工具类生成JWT令牌并返回

**application.yml**

~~~yml
jwt:
    # 设置jwt签名加密时使用的秘钥
    secret-key: itcast
    # 设置jwt过期时间
    ttl: 7200000
    # 设置前端传递过来的令牌名称
    token-name: token
~~~

**properties/JwtProperties.java**

~~~java
@Component
@ConfigurationProperties(prefix = "jwt")
@Data
public class JwtProperties {
    /**
     * 生成jwt令牌相关配置
     */
    private String secretKey;
    private long ttl;
    private String tokenName;

}
~~~

**JWT工具类**

~~~java
public class JwtUtil {
    /**
     * 生成jwt
     * 使用Hs256算法, 私匙使用固定秘钥
     *
     * @param secretKey jwt秘钥
     * @param ttlMillis jwt过期时间(毫秒)
     * @param claims    设置的信息
     * @return
     */
    public static String createJWT(String secretKey, long ttlMillis, Map<String, Object> claims) {
        // 指定签名的时候使用的签名算法，也就是header那部分
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;

        // 生成JWT的时间
        long expMillis = System.currentTimeMillis() + ttlMillis;
        Date exp = new Date(expMillis);

        // 设置jwt的body
        JwtBuilder builder = Jwts.builder()
                // 如果有私有声明，一定要先设置这个自己创建的私有的声明，这个是给builder的claim赋值，一旦写在标准的声明赋值之后，就是覆盖了那些标准的声明的
                .setClaims(claims)
                // 设置签名使用的签名算法和签名使用的秘钥
                .signWith(signatureAlgorithm, secretKey.getBytes(StandardCharsets.UTF_8))
                // 设置过期时间
                .setExpiration(exp);

        return builder.compact();
    }

    /**
     * Token解密
     *
     * @param secretKey jwt秘钥 此秘钥一定要保留好在服务端, 不能暴露出去, 否则sign就可以被伪造, 如果对接多个客户端建议改造成多个
     * @param token     加密后的token
     * @return
     */
    public static Claims parseJWT(String secretKey, String token) {
        // 得到DefaultJwtParser
        Claims claims = Jwts.parser()
                // 设置签名的秘钥
                .setSigningKey(secretKey.getBytes(StandardCharsets.UTF_8))
                // 设置需要解析的jwt
                .parseClaimsJws(token).getBody();
        return claims;
    }

}
~~~



**登录成功，生成JWT令牌并返回**

~~~java
@RestController
@Slf4j
public class LoginController {
    // 注入配置类
    @Autowired
    private JwtProperties jwtProperties;
    
    //依赖业务层对象
    @Autowired
    private EmpService empService;

    @PostMapping("/login")
    public Result login(@RequestBody Emp emp) {
        //调用业务层：登录功能
        Emp loginEmp = empService.login(emp);

        //判断：登录用户是否存在
        if(loginEmp != null ){
            //自定义信息
            Map<String , Object> claims = new HashMap<>();
            claims.put("id", loginEmp.getId());
            claims.put("username",loginEmp.getUsername());
            claims.put("name",loginEmp.getName());

            //使用JWT工具类，生成身份令牌
            String token = JwtUtils.createJWT(jwtProperties.getSecretKey(), jwtProperties.getTtl(), claims);
            return Result.success(token);
        }
        return Result.error("用户名或密码错误");
    }
}
~~~



重启服务，打开postman测试登录接口：

![image-20230106212805480](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281507470.png)

打开浏览器完成前后端联调操作：利用开发者工具，抓取一下网络请求

![image-20230106213419461](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281543302.png)

> 登录请求完成后，可以看到JWT令牌已经响应给了前端，此时前端就会将JWT令牌存储在浏览器本地。



服务器响应的JWT令牌存储在本地浏览器哪里了呢？

- 在当前案例中，JWT令牌存储在浏览器的本地存储空间local storage中了。 local storage是浏览器的本地存储，在移动端也是支持的。

![image-20230106213910049](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281507492.png)



我们在发起一个查询部门数据的请求，此时我们可以看到在请求头中包含一个token(JWT令牌)，后续的每一次请求当中，都会将这个令牌携带到服务端。

![image-20230106214331443](https://cdn.jsdelivr.net/gh/Epiphany6666/Picture/blog/JWT%E4%BB%A4%E7%89%8C/assets/202402281507495.png)

# 