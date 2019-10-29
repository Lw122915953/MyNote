# 一、[springmvc Controller接收前端参数的几种方式总结](https://www.cnblogs.com/mjs154/p/11667796.html)

 

## (1) 普通方式-请求参数名和Controller方法的参数一致

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 @Controller
 2 @RequestMapping("/param")
 3 public class TestParamController {
 4     private static final Logger logger = LoggerFactory.getLogger(TestParamController.class);
 5     /**
 6      * 请求参数名和Controller方法的参数一致
 7      * produces 设置返回参数的编码格式可以设置返回数据的类型以及编码，可以是json或者xml
 8      * {
 9      *     @RequestMapping(value="/xxx",produces = {"application/json;charset=UTF-8"})
10      *      或
11      *     @RequestMapping(value="/xxx",produces = {"application/xml;charset=UTF-8"})
12      *      或
13      *     @RequestMapping(value="/xxx",produces = "{text/html;charset=utf-8}")
14      * }
15      * @param name 用户名
16      * @param pwd 密码
17      * @return
18      *
19      */
20     @RequestMapping(value = "/add", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
21     @ResponseBody
22     public String addUser(String name, String pwd){
23         logger.debug("name:" + name + ",pwd:" + pwd);
24         return "name:" + name + ",pwd:" + pwd;
25     }
26 
27 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

如下图所示：

通过访问：http://localhost:8080/sty/param/add.action?name=张三&pwd=123456

  ![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013182756186-1547350380.png)        

 

## (2) 对象方式-请求参数名和Controller方法中的对象的参数一致

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
@Controller
@RequestMapping("/param")
public class TestParamController {
    private static final Logger logger = LoggerFactory.getLogger(TestParamController.class);
    /**
     * 请求参数名和Controller方法的参数一致
     * produces 设置返回参数的编码格式可以设置返回数据的类型以及编码，可以是json或者xml
     * }
     * @param user 用户信息
     * @return
     *
     */
    @RequestMapping(value = "/addByObject", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUserByObject(User user){
        logger.debug("name:" + user.getName() + ",pwd:" + user.getPwd());
        return "name:" + user.getName() + ",pwd:" + user.getPwd();
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

如下图所示：

通过访问：http://localhost:8080/sty/param/addByObject.action?name=张三&pwd=123456 。

 ![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183008750-2044912343.png)

 

## (3) 自定义方法参数名-当请求参数名与方法参数名不一致时

 

　　注意可以在参数中增加@RequestParam注解。如果在方法中的参数增加了该注解，说明请求的url必须带该带有该参数，否则不能执行该方法。如果在方法中的参数没有增加该注解，说明请求的url无需带有该参数，也能继续执行该方法。

　　@RequestParam(defaultValue="0")可设置默认值(仅当传入参数为空时)。

　　@RequestParam(value="id")可接受传入参数为id的值，覆盖该注解注释的字段。

　　@RequestParam(name="name",defaultValue = "李四") String u_name   如果传入字段”name”为空，默认u_name的值为”李四”。若传入”name”不为空，默认u_name值为传入值。

以下只该出方法：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**
 * 自定义方法参数名-当请求参数名与方法参数名不一致时
 * @param u_name 用户名
 * @param u_pwd 密码
 * @return
 */
@RequestMapping(value = "/addByDifName", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
@ResponseBody
public String addUserByDifName(@RequestParam("name") String u_name, @RequestParam("pwd")String u_pwd){
    logger.debug("name:" + u_name + ",pwd:" + u_pwd);
    return "name:" + u_name + ",pwd:" + u_pwd;
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

如下图所示：

通过访问：http://localhost:8080/sty/param/addUserByDifName.action?name=张三&pwd=123456 。

 ![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183130814-1350322278.png)

 

## (4) HttpServletRequest方式

 

　　以下只给出该方法：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**

 * 通过HttpServletRequest接收

 * @param request

 * @return

 */

@RequestMapping(value = "/addByHttpServletRequest", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})

@ResponseBody

public String addUserByHttpServletRequest(HttpServletRequest request){

    String name = request.getParameter("name");

    String pwd = request.getParameter("pwd");

    logger.debug("name:" + name + ",pwd:" + pwd);

    return "name:" + name + ",pwd:" + pwd;

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

如下图所示：

通过访问：http://localhost:8080/sty/param/addByHttpServletRequest.action?name=张三&pwd=123456

 ![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183303668-1903838336.png)



##  (5) @PathVariable获取路径中的参数接收

 

​         参考：https://www.iteye.com/blog/zhlj11-1885005 。

　　　注：url含有中文名称时，因为编码问题，无法进行映射，需要修改tomcat下的conf文件夹下的server.xml中的URIEncoding=”UTF-8”，对URL编码设置就可以解决中文问题。

对于经常遇到路径在有符号”.”问题，因为springmvc默认是把点后面的信息作为文件后缀，需要修改默认值：

```
<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"> 
              <property name="interceptors" ref="localeChangeInterceptor"/>
              <property name="useDefaultSuffixPattern" value="false" /> 
       </bean> 
```

　　另外，这时候如果只设置这个，请求可以传递到对于的controller，但传过去的数据会有问题，只会传最后一个点前面的数据，除非你在最后加上“/”，比如/news/测试.点/  这样就会把“测试.点”当作整体，不然只会得到“测试”。这时候我们可以这样设置@RequestMapping("/news/{title:.*}") 

以下只给出该方法(本次不进行中文及特殊符号测试)：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**

 * 通过@PathVariable获取路径中的参数 

 * @param name 用户名

 * @param pwd 密码

 * @return

 */

@RequestMapping(value = "/add/{name}/{pwd}", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})

@ResponseBody

public String addUserByPathVariable(@PathVariable String name, @PathVariable String pwd){

    logger.debug("name:" + name + ",pwd:" + pwd);

    return "name:" + name + ",pwd:" + pwd;

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

如下图所示：

通过访问：http://localhost:8080/sty/param/add/zhangsan/123456.action

 ![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183507968-1158068097.png)

 

## (6) @RequestBody-JSON方式接收 

　　以上方式(1)/(2)/)(3)/(4)/(5)都是非JSON方式，也就是说如果使用JSON方式提交，会报错(在第二种对象方式中，将get请求方式修改为POST，并将上送数据修改为JSON串方式)：

​       此时未引入jackson-databind.jar依赖。并在springmvc.xml文件未进行开启json格式的支持，也就是说未加入以下代码：

```
<!-- 同时开启json格式的支持-->
<mvc:annotation-driven></mvc:annotation-driven>
```

　　提交请求打印未有报错，但是返回的数据为null，如图所示：

 　　![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183622571-370150079.png)

​         若开启json格式的支持，测试也如上图所示，也并能正常返回。

​         原因：因为为在字段名称之前未使用@RequestBody注解。

**eg1(****测试普通对象)**

​         代码如下所示，

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**

 * RequestBody-JSON 对象方式

 * @param user

 * @return

 */

@RequestMapping(value = "/addByObjectJSON", produces = {"application/json;charset=UTF-8"})

@ResponseBody

public String addUserByObjectJSON(@RequestBody User user){

    logger.debug("name:" + user.getName() + ",pwd:" + user.getPwd());

    return "name:" + user.getName() + ",pwd:" + user.getPwd();

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
测试结果如图所示(访问 http://localhost:8080/sty/param/addByObjectJSON.action ):
```

 ![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183734999-378814857.png)

**eg2(****测试List****对象)**

代码如下所示，

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**
 * RequestBody-JSON List对象方式
 * @param users
 * @return
 */
@RequestMapping(value = "/addByListJSON", produces = {"application/json;charset=UTF-8"})
@ResponseBody
public String addUsersByListJSON(@RequestBody List<User> users){
    StringBuilder sb = new StringBuilder("{");
    if(null != users){
        for(User user : users){
            sb.append("{" + "name:" + user.getName() + ",pwd:" + user.getPwd() + "}");
        }
    }
    sb.append("}");
    logger.debug(sb.toString());
    return sb.toString();
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　 测试结果

```
　 测试结果如图所示(访问 http://localhost:8080/sty/param/addByListJSON.action  ):
```

 　　![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013183859617-719405434.png)

**eg3(测试Map对象)**

代码如下图所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
/**

 * RequestBody-JSON Map对象方式

 * @param users

 * @return

 */

@RequestMapping(value = "/addByMapJSON", produces = {"application/json;charset=UTF-8"})

@ResponseBody

public String addUsersByMapJSON(@RequestBody Map<String, User> users){

    StringBuilder sb = new StringBuilder("{");

    if(null != users){

        Iterator it = users.keySet().iterator();

        while(it.hasNext()){

            User user = users.get(it.next());

            sb.append("{" + "name:" + user.getName() + ",pwd:" + user.getPwd() + "}");

        }

    }

    sb.append("}");

    logger.debug(sb.toString());

    return sb.toString();

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

   测试结果

```
测试结果如图所示(访问 http://localhost:8080/sty/param/addByMapJSON.action ):
```

![img](https://img2018.cnblogs.com/blog/643014/201910/643014-20191013184024930-348985723.png)

```
另外附部分源码：
```

 　User.java

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.mjs.study.action.dto;

/**
 * @Description 
 * @ClassName User
 * @Author Administrator
 * @Data 2019/10/13 2:43
 * @Version 1.0
 */
public class User {
    private String name;
    private String pwd;
    private String sex;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 TestParamController.java　
```

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.mjs.study.action;

import com.github.pagehelper.PageInfo;
import com.mjs.study.action.dto.User;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import javax.servlet.http.HttpServletRequest;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

/**
 * @Description 测试springmvc传入参数
 * @ClassName TestParamController
 * @Author Administrator
 * @Data 2019/10/13 1:33
 * @Version 1.0
 */
@Controller
@RequestMapping("/param")
public class TestParamController {
    private static final Logger logger = LoggerFactory.getLogger(TestParamController.class);
    /**
     * 请求参数名和Controller方法的参数一致
     * produces 设置返回参数的编码格式 可以设置返回数据的类型以及编码，可以是json或者xml
     * {
     *     @RequestMapping(value="/xxx",produces = {"application/json;charset=UTF-8"})
     *      或
     *     @RequestMapping(value="/xxx",produces = {"application/xml;charset=UTF-8"})
     *      或
     *     @RequestMapping(value="/xxx",produces = "{text/html;charset=utf-8}")
     * }
     * @param name 用户名
     * @param pwd 密码
     * @return
     *
     */
    @RequestMapping(value = "/add", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUser(String name, String pwd){
        logger.debug("name:" + name + ",pwd:" + pwd);
        return "name:" + name + ",pwd:" + pwd;
    }

    /**
     * 自定义方法参数名-当请求参数名与方法参数名不一致时
     * @param u_name 用户名
     * @param u_pwd 密码
     * @return
     */
    @RequestMapping(value = "/addByDifName", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUserByDifName(@RequestParam(name="name",defaultValue = "李四") String u_name, @RequestParam("pwd")String u_pwd){
        logger.debug("name:" + u_name + ",pwd:" + u_pwd);
        return "name:" + u_name + ",pwd:" + u_pwd;
    }

    /**
     * 请求参数名和Controller方法的参数一致
     * produces 设置返回参数的编码格式 可以设置返回数据的类型以及编码，可以是json或者xml
     * @param user 用户信息
     * @return
     *
     */
    @RequestMapping(value = "/addByObject", produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUserByObject(User user){
        logger.debug("name:" + user.getName() + ",pwd:" + user.getPwd());
        return "name:" + user.getName() + ",pwd:" + user.getPwd();
    }

    /**
     * RequestBody-JSON 对象方式
     * @param user
     * @return
     */
    @RequestMapping(value = "/addByObjectJSON", produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUserByObjectJSON(@RequestBody User user){
        logger.debug("name:" + user.getName() + ",pwd:" + user.getPwd());
        return "name:" + user.getName() + ",pwd:" + user.getPwd();
    }

    /**
     * RequestBody-JSON List对象方式
     * @param users
     * @return
     */
    @RequestMapping(value = "/addByListJSON", produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUsersByListJSON(@RequestBody List<User> users){
        StringBuilder sb = new StringBuilder("{");
        if(null != users){
            for(User user : users){
                sb.append("{" + "name:" + user.getName() + ",pwd:" + user.getPwd() + "}");
            }
        }
        sb.append("}");
        logger.debug(sb.toString());
        return sb.toString();
    }
    /**
     * RequestBody-JSON Map对象方式
     * @param users
     * @return
     */
    @RequestMapping(value = "/addByMapJSON", produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUsersByMapJSON(@RequestBody Map<String, User> users){
        StringBuilder sb = new StringBuilder("{");
        if(null != users){
            Iterator it = users.keySet().iterator();
            while(it.hasNext()){
                User user = users.get(it.next());
                sb.append("{" + "name:" + user.getName() + ",pwd:" + user.getPwd() + "}");
            }
        }
        sb.append("}");
        logger.debug(sb.toString());
        return sb.toString();
    }
    /**
     * 通过HttpServletRequest接收
     * @param request
     * @return
     */
    @RequestMapping(value = "/addByHttpServletRequest", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUserByHttpServletRequest(HttpServletRequest request){
        String name = request.getParameter("name");
        String pwd = request.getParameter("pwd");
        logger.debug("name:" + name + ",pwd:" + pwd);
        return "name:" + name + ",pwd:" + pwd;
    }

    /**
     * 通过@PathVariable获取路径中的参数
     * @param name 用户名
     * @param pwd 密码
     * @return
     */
    @RequestMapping(value = "/add/{name}/{pwd}", method = RequestMethod.GET, produces = {"application/json;charset=UTF-8"})
    @ResponseBody
    public String addUserByPathVariable(@PathVariable String name, @PathVariable String pwd){
        logger.debug("name:" + name + ",pwd:" + pwd);
        return "name:" + name + ",pwd:" + pwd;
    }
}
```

# 二、springmvc跳转至前端的几种方式

## 一、 根据 String 字符串跳转

      1、返回字符串 --- 返回jsp页面

/**
* description: 返回字符串 --- 返回jsp页面
* @author cao
* @date 2019年4月10日 下午10:17
*/
@RequestMapping(value={"/forwardJsp"})
public String forwardJsp(Model model){
    model.addAttribute("name", "返回jsp页面");
    return "modules/sys/sysLogin";
}
       2、返回字符串 --- 服务端转发

/**
* description: 返回字符串 --- 服务端转发
* @author cao
* @date 2019年4月10日 下午10:20
*/
@RequestMapping(value={"/forward"})
public String forward(Model model){
    model.addAttribute("name", "服务端转发");
    return "forward:forwardJsp";
}
       3、返回字符串 --- 客户端重定向    

/**
* description: 返回字符串 --- 客户端重定向
* @author cao
* @date 2019年4月10日 下午10:27
*/
@RequestMapping(value="/redirect")
public String redirect(){
    return "redirect:"+"/forward";
}

 

## 二、根据 request 或 response 进行跳转

      1、返回 void --- 请求转发（request转发）      

/**
* description: 返回 void --- 请求转发（request转发）
* @author cao
* @date 2019年4月10日 下午10:26
*/
@RequestMapping(value="/requestForward")
public void requestForward(HttpServletRequest request,HttpServletResponse response) throws ServletException, IOException{
    request.setAttribute("name", "请求转发（服务端转发）");
    request.getRequestDispatcher("/forward").forward(request, response);
}
     2、返回 void --- 重定向 （response）

/**
* description: 返回 void --- 重定向 （response）
* @author cao
* @date 2019年4月10日 下午10:29
*/
@RequestMapping(value="/response")
public void response(HttpServletResponse response) throws IOException{
    response.sendRedirect("/forwardJsp");
}
     3、返回 void --- Json字符串

/**
* description: 返回 void --- Json字符串
* @author cao
* @date 2019年4月10日 下午10:30
*/
@RequestMapping(value="/responseJson")
public void responseJson(HttpServletResponse response) throws IOException{
    response.setCharacterEncoding("utf-8");
    response.setContentType("application/json;charset=utf-8");
    response.getWriter().write("json串");
}

##  三、根据 ModelAndView 对象进行跳转

    1、返回对象 ModelAndView --- 返回 jsp 页面  

/**
* description: 返回对象 ModelAndView --- 返回 jsp 页面
* @author cao
* @date 2019年4月10日 下午10:32
*/
@RequestMapping(value="/modelAndViewJsp")
public ModelAndView modelAndViewJsp(){
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.setViewName("modules/sys/sysLogin");
    return modelAndView;
}


    2、返回对象 ModelAndView --- 服务端转发

/**
* description: 返回对象 ModelAndView --- 服务端转发
* @author cao
* @date 2019年4月10日 下午10:37
*/
@RequestMapping(value="/modelAndViewForward")
public ModelAndView modelAndViewForward(){
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.setViewName("forward:/forwardJsp");
    return modelAndView;
}
   3、返回对象 ModelAndView --- 重定向  

/**
* description: 返回对象 ModelAndView --- 重定向
* @author cao
* @date 2019年4月10日 下午10:40
*/
@RequestMapping(value="/modelAndViewRedirect")
public ModelAndView modelAndViewRedirect(){
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.setViewName("redirect:/forwardJsp");
    return modelAndView;
}
* 
