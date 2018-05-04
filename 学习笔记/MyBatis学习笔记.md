#MyBatis学习笔记

##sqlSession
	SqlSessionFactory
	SqlSessionFactory是MyBatis的关键对象，它是单个数据库映射关系经过编译后的内存镜像。SqlSessionFactory对象的实例可以通过SqlSessionFactoryBuilder对象来获得，而SqlSessionFactoryBuildr则可以从XML配置文件或一个预先定制的Configuration的实例构建出Sq1SessionFactory的实例。
	每一个MyBatis的应用程序都以一个SqlSessionFactory对象的实例为核心。SqlSessionFactory是线程安全的，它一旦被创建，应该在应用执行期间都存在。在应用运行期间不要重复创建多次，建议使用单例模式。SqlSessionFactory 是创建SqlSession的工厂。
	
	SqlSession
	SqlSession是MyBatis的关键对象，是执行持久化操作的对象，类似于JDBC中的Connection。它是应用程序与持久存储层之间执行交互操作的一个单线程对象，也是MyBatis执行持久化操作的关键对象。SqlSession对象完全包含以数据库为背景的所有执行SQL操作的方法，它的底层封装了JDBC连接，可以用SqlSession实例来直接执行已映射的SQL语句。每个线程都应该有它自己的SqlSession实例。SqlSession的实例不能被共享，也是线程不安全的，绝对不能将SqlSession 实例的引用放在一个类的静态字段甚至是实例字段中。也绝不能将SqlSession实例的引用放在任何类型的管理范围中，比如Serlvet当中的HttpSession 对象中。使用完SqlSession之后关闭Session很重要，应该确保使用finally块来关闭它。
	
	
##输出映射

输出映射的两种方式
```
* resultType
* resultMap
```	

###resultType
```
* 使用resultType进行输出映射，只有查询出来的列名和pojo中的属性名一致，该列才可以映射成功。
* 如果查询出来的列名和pojo中的属性名全部不一致，没有创建pojo对象。
* 只要查询出来的列名和pojo中的属性有一个一致，就会创建pojo对象。
```

mapper.xml 
```
 <!-- 用户信息综合查询总数
        parameterType：指定输入类型和findUserList一样
        resultType：输出结果类型
    -->
    <select id="findUserCount" parameterType="com.iot.mybatis.po.UserQueryVo" resultType="int">
        SELECT count(*) FROM user WHERE user.sex=#{userCustom.sex} AND user.username LIKE '%${userCustom.username}%'
    </select>
```

输出pojo对象和pojo列表
不管是输出的pojo单个对象还是一个列表（list中包括pojo），在mapper.xml中resultType指定的类型是一样的。在mapper.java指定的方法返回值类型不一样。
生成的动态代理对象中是根据mappper方法的返回值类型确定是调用selectOne(返回单个对象调用)还是selectList(返回集合对象调用);

###resultMap
mybatis中使用resultMap完成高级输出结果映射。（一对多，多对多）
使用方法
  如果查询出来的列名和pojo的属性名不一致，通过定义一个resultMap对列名和pojo属性名之间作一个映射关系。
	1.定义resultMap
	2.使用resultMap作为statement的输出映射类型

定义resultMap

```
<!-- 定义resultMap
    将SELECT id id_,username username_ FROM USER 和User类中的属性作一个映射关系

    type：resultMap最终映射的java对象类型,可以使用别名
    id：对resultMap的唯一标识
     -->
     <resultMap type="user" id="userResultMap">
        <!-- id表示查询结果集中唯一标识 
        column：查询出来的列名
        property：type指定的pojo类型中的属性名
        最终resultMap对column和property作一个映射关系 （对应关系）
        -->
        <id column="id_" property="id"/>
        <!-- 
        result：对普通名映射定义
        column：查询出来的列名
        property：type指定的pojo类型中的属性名
        最终resultMap对column和property作一个映射关系 （对应关系）
         -->
        <result column="username_" property="username"/>

     </resultMap>
```

使用resultMap作为statement的输出映射类型

```
<!-- 使用resultMap进行输出映射
        resultMap：指定定义的resultMap的id，如果这个resultMap在其它的mapper文件，前边需要加namespace
        -->
    <select id="findUserByIdResultMap" parameterType="int" resultMap="userResultMap">
        SELECT id id_,username username_ FROM USER WHERE id=#{value}
    </select>
```
总结
 使用resultType进行输出映射，只有查询出来的列名和pojo中的属性名一致，该列才可以映射成功。
 如果查询出来的列名和pojo的属性名不一致，通过定义一个resultMap对列名和pojo属性名之间作一个映射关系。
 
##动态sql
mybatis核心，对sql语句进行灵活操作，通过表达式进行判断，对sql进行灵活拼接，组装。
###if判断

```
<!-- 用户信息综合查询
    #{userCustom.sex}:取出pojo包装对象中性别值
    ${userCustom.username}：取出pojo包装对象中用户名称
 -->
<select id="findUserList" parameterType="com.iot.mybatis.po.UserQueryVo"
        resultType="com.iot.mybatis.po.UserCustom">
    SELECT * FROM user
    <!--  where 可以自动去掉条件中的第一个and -->
    <where>
        <if test="userCustom!=null">
            <if test="userCustom.sex!=null and userCustom.sex != '' ">
               AND user.sex=#{userCustom.sex}
            </if>
            <if test="userCustom.username!=null and userCustom.username != '' ">
               AND user.username LIKE '%${userCustom.username}%'
            </if>
        </if>
    </where>


</select>

<!-- 用户信息综合查询总数
    parameterType：指定输入类型和findUserList一样
    resultType：输出结果类型
-->
<select id="findUserCount" parameterType="com.iot.mybatis.po.UserQueryVo" resultType="int">
    SELECT count(*) FROM user
    <where>
        <if test="userCustom!=null">
            <if test="userCustom.sex!=null and userCustom.sex != '' ">
                AND user.sex=#{userCustom.sex}
            </if>
            <if test="userCustom.username!=null and userCustom.username != '' ">
                AND user.username LIKE '%${userCustom.username}%'
            </if>
        </if>
    </where>
</select>
``` 

sql片段(重点)
将上边实现的动态sql判断代码块抽取出来，组成一个sql片段。其他的statement中可以引用sql片段。
定义sql片段

```
<!-- 定义sql片段
id：sql片段的唯 一标识

经验：是基于单表来定义sql片段，这样话这个sql片段可重用性才高
在sql片段中不要包括 where
 -->
<sql id="query_user_where">
    <if test="userCustom!=null">
        <if test="userCustom.sex!=null and userCustom.sex!=''">
            AND user.sex = #{userCustom.sex}
        </if>
        <if test="userCustom.username!=null and userCustom.username!=''">
            AND user.username LIKE '%${userCustom.username}%'
        </if>
    </if>
</sql>
```

引用sql片段 

```
<include refid="query_user_where"></include>
```

```
<!-- 用户信息综合查询
    #{userCustom.sex}:取出pojo包装对象中性别值
    ${userCustom.username}：取出pojo包装对象中用户名称
 -->
<select id="findUserList" parameterType="com.iot.mybatis.po.UserQueryVo"
        resultType="com.iot.mybatis.po.UserCustom">
    SELECT * FROM user
    <!--  where 可以自动去掉条件中的第一个and -->
    <where>
        <!-- 引用sql片段 的id，如果refid指定的id不在本mapper文件中，需要前边加namespace -->
        <include refid="query_user_where"></include>
        <!-- 在这里还要引用其它的sql片段  -->
    </where>
</select>
```

###foreach标签
向sql传递数组或者list，mybatis使用foreach解析
在用户查询列表和查询总数的statement中增加多个id输入查询。两种方法的sql语句如下:

```
* 
```

##一对一查询
###resultType实现
自定义pojo类OrdersCustom的代码

```
package com.iot.mybatis.po;

/**
 * 
 * <p>Title: OrdersCustom</p>
 * <p>Description: 订单的扩展类</p>
 */
//通过此类映射订单和用户查询的结果，让此类继承包括 字段较多的pojo类
public class OrdersCustom extends Orders{

    //添加用户属性
    /*USER.username,
      USER.sex,
      USER.address */

    private String username;
    private String sex;
    private String address;


    public String getUsername() {
        return username;
    }
    public void setUsername(String username) {
        this.username = username;
    }
	  ...
}
```

mapper.xml

```
 <!-- 查询订单关联查询用户信息 -->
<select id="findOrdersUser"  resultType="com.iot.mybatis.po.OrdersCustom">
  SELECT
      orders.*,
      user.username,
      user.sex,
      user.address
    FROM
      orders,
      user
    WHERE orders.user_id = user.id
</select>
```

mapper.java

```
//查询订单关联查询用户信息
public List<OrdersCustom> findOrdersUser()throws Exception;
}
```

###resultMap实现
使用resultMap将查询结果中的订单信息映射到Orders对象中，在orders类中添加User属性，将关联查询出来的用户信息映射到orders对象中的user属性中。

定义resultMap

```
<!-- 订单查询关联用户的resultMap
将整个查询的结果映射到com.iot.mybatis.po.Orders中
 -->
<resultMap type="com.iot.mybatis.po.Orders" id="OrdersUserResultMap">
    <!-- 配置映射的订单信息 -->
    <!-- id：指定查询列中的唯一标识，订单信息的中的唯 一标识，如果有多个列组成唯一标识，配置多个id
        column：订单信息的唯一标识列
        property：订单信息的唯一标识列所映射到Orders中哪个属性
      -->
    <id column="id" property="id"/>
    <result column="user_id" property="userId"/>
    <result column="number" property="number"/>
    <result column="createtime" property="createtime"/>
    <result column="note" property="note"/>

    <!-- 配置映射的关联的用户信息 -->
    <!-- association：用于映射关联查询单个对象的信息
    property：要将关联查询的用户信息映射到Orders中哪个属性
     -->
    <association property="user"  javaType="com.iot.mybatis.po.User">
        <!-- id：关联查询用户的唯 一标识
        column：指定唯 一标识用户信息的列
        javaType：映射到user的哪个属性
         -->
        <id column="user_id" property="id"/>
        <result column="username" property="username"/>
        <result column="sex" property="sex"/>
        <result column="address" property="address"/>
    </association>
</resultMap>
```

statement定义

```
<!-- 查询订单关联查询用户信息 -->
<select id="findOrdersUserResultMap" resultMap="OrdersUserResultMap">
    SELECT
    orders.*,
    user.username,
    user.sex,
    user.address
    FROM
    orders,
    user
    WHERE orders.user_id = user.id
</select>
```

mapper.java

```
//查询订单关联查询用户使用resultMap
public List<Orders> findOrdersUserResultMap()throws Exception;
```

resultType和resultMap实现一对一查询小结
实现一对一查询:

```
* resultType：使用resultType实现较为简单，如果pojo中没有包括查询出来的列名，需要增加列名对应的属性，即可完成映射。如果没有查询结果的特殊要求建议使用resultType。
* resultMap：需要单独定义resultMap，实现有点麻烦，如果对查询结果有特殊的要求，使用resultMap可以完成将关联查询映射pojo的属性中。
* resultMap可以实现延迟加载，resultType无法实现延迟加载。
```

##一对多查询
resultMap定义

```
<!-- 订单及订单明细的resultMap
使用extends继承，不用在中配置订单信息和用户信息的映射
 -->
<resultMap type="com.iot.mybatis.po.Orders" id="OrdersAndOrderDetailResultMap" extends="OrdersUserResultMap">
    <!-- 订单信息 -->
    <!-- 用户信息 -->
    <!-- 使用extends继承，不用在中配置订单信息和用户信息的映射 -->


    <!-- 订单明细信息
    一个订单关联查询出了多条明细，要使用collection进行映射
    collection：对关联查询到多条记录映射到集合对象中
    property：将关联查询到多条记录映射到com.iot.mybatis.po.Orders哪个属性
    ofType：指定映射到list集合属性中pojo的类型
     -->
    <collection property="orderdetails" ofType="com.iot.mybatis.po.Orderdetail">
        <!-- id：订单明细唯 一标识
        property:要将订单明细的唯 一标识 映射到com.iot.mybatis.po.Orderdetail的哪个属性
          -->
        <id column="orderdetail_id" property="id"/>
        <result column="items_id" property="itemsId"/>
        <result column="items_num" property="itemsNum"/>
        <result column="orders_id" property="ordersId"/>
    </collection>

</resultMap>
```

mapper.java

```
//查询订单(关联用户)及订单明细
public List<Orders>  findOrdersAndOrderDetailResultMap()throws Exception;
```

小结
mybatis使用resultMap的collection对关联查询的多条记录映射到一个list集合属性中。
使用resultType实现：将订单明细映射到orders中的orderdetails中，需要自己处理，使用双重循环遍历，去掉重复记录，将订单明细放在orderdetails中。

resultMap中不能出现相同column的值，具体解决方案:
查询语句中，为字段建立别名，通过别名返回结果集（为重复的字段建立别名）。只需要修改CustomerMapper.xml文件即可。

```
<mapper namespace="cn.itcast.test1.domain">
	<resultMap type="cn.itcast.test1.domain.Customer" id="customerTest">
		<id property="id" column="id" />
		<result property="name" column="name" />
		<collection property="orderList" ofType="cn.itcast.test1.domain.Order">
			<id property="id" column="o_id" />
			<result property="name" column="o_name" />
		</collection>
	</resultMap>
	<select id="getinfo" parameterType="int" resultMap="customerTest">
		select c.*,o.id as o_id,o.name as o_name from customer c,`order` o where c.id = o.customer_id and c.id=#{id}
	</select>
</mapper>
```

##多对多查询
使用association和collection完成一对一和一对多高级映射（对结果有特殊的映射要求）。
association:

```
* 作用：将关联查询信息映射到一个pojo对象中。
* 场合：为了方便查询关联信息可以使用association将关联订单信息映射为用户对象的pojo属性中，比如：查询订单及关联用户信息。
```

使用resultType无法将查询结果映射到pojo对象的pojo属性中，根据对结果集查询遍历的需要选择使用resultType还是resultMap。
collection:

```
* 作用：将关联查询信息映射到一个list集合中。
* 场合：为了方便查询遍历关联信息可以使用collection将关联信息映射到list集合中，比如：查询用户权限范围模块及模块下的菜单，可使用collection将模块映射到模块list中，将菜单列表映射到模块对象的菜单list属性中，这样的作的目的也是方便对查询结果集进行遍历查询。如果使用resultType无法将查询结果映射到list集合中。
```

##延迟加载
resultMap可以实现高级映射（使用association、collection实现一对一及一对多映射），association、collection具备延迟加载功能。
延迟加载：先从单表查询、需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。
需求：
如果查询订单并且关联查询用户信息。如果先查询订单信息即可满足要求，当我们需要查询用户信息时再查询用户信息。把对用户信息的按需去查询就是延迟加载。

###使用association实现延迟加载
定义两个mapper的方法相应的statement
1.只查询订单信息， 在查询订单的statement中使用association去延迟加载下边的statement(关联查询用户信息)。

```
<!-- 查询订单关联查询用户，用户信息需要延迟加载 -->
<select id="findOrdersUserLazyLoading" resultMap="OrdersUserLazyLoadingResultMap">
    SELECT * FROM orders
</select>
```
2.关联查询用户信息，通过上边查询到的订单信息中user_id去关联查询用户信息，使用UserMapper.xml中的findUserById

```
<select id="findUserById" parameterType="int" resultType="com.iot.mybatis.po.User">
    SELECT * FROM  user  WHERE id=#{value}
</select>
```

上边先去执行findOrdersUserLazyLoading，当需要去查询用户的时候再去执行findUserById，通过resultMap的定义将延迟加载执行配置起来。
延迟加载resultMap

```
<!-- 延迟加载的resultMap -->
<resultMap type="com.iot.mybatis.po.Orders" id="OrdersUserLazyLoadingResultMap">
    <!--对订单信息进行映射配置  -->
    <id column="id" property="id"/>
    <result column="user_id" property="userId"/>
    <result column="number" property="number"/>
    <result column="createtime" property="createtime"/>
    <result column="note" property="note"/>
    <!-- 实现对用户信息进行延迟加载
    select：指定延迟加载需要执行的statement的id（是根据user_id查询用户信息的statement）
    要使用userMapper.xml中findUserById完成根据用户id(user_id)用户信息的查询，如果findUserById不在本mapper中需要前边加namespace
    column：订单信息中关联用户信息查询的列，是user_id
    关联查询的sql理解为：
    SELECT orders.*,
    (SELECT username FROM USER WHERE orders.user_id = user.id)username,
    (SELECT sex FROM USER WHERE orders.user_id = user.id)sex
     FROM orders
     -->
    <association property="user"  javaType="com.iot.mybatis.po.User"
                 select="com.iot.mybatis.mapper.UserMapper.findUserById"
                 column="user_id">
     <!-- 实现对用户信息进行延迟加载 -->

    </association>

</resultMap>
```
与非延迟加载的主要区别就在association标签属性多了select和column。

##缓存
不使用分布缓存，缓存的数据在各各服务单独存储，不方便系统开发。所以要使用分布式缓存对缓存数据进行集中管理。
mybatis无法实现分布式缓存，需要和其它分布式缓存框架进行整合。
对缓存数据进行集中管理(redis集群),使用分布式缓存框架, redis, memcached,ehcache

##spring和mybatis整合
通过MapperScannerConfigure进行mapper扫描

```
<!-- mapper批量扫描，从mapper包中扫描出mapper接口，自动创建代理对象并且在spring容器中注册
    遵循规范：将mapper.java和mapper.xml映射文件名称保持一致，且在一个目录 中
    自动扫描出来的mapper的bean的id为mapper类名（首字母小写）
    -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!-- 指定扫描的包名
    如果扫描多个包，每个包中间使用半角逗号分隔
    -->
    <property name="basePackage" value="com.iot.ssm.mapper"/>
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>

</bean>
```




