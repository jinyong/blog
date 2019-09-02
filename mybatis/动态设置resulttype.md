# 1. 概述
## 动机
1. mybatis到目前为止没有提供动态的resulttype。
2. 项目中有很多个表，所以存在很多对应的POJO。
3. 不同POJO查询之间的区别为
    + 字段不同
    + POJO的类型不同
4. 不同POJO查询之间的相似点为
    + sql语句的结构一致
5. 既然mybatis提供动态sql，为何不写一个通用的sql
### 于是产生了以下的sql
```
<select id="queryDynamic" parameterType="baseDynamicQuery" resultType="HashMap">
    select
    <foreach collection="columnList" item="column" separator=",">
        ${column}
    </foreach>
    from
        ${table}
    <if test="leftJoinList != null">
        <foreach collection="leftJoinList" item="leftJoin" separator=" ">
            ${leftJoin}
        </foreach>
    </if>
    <if test="whereList != null">
    where
        <foreach collection="whereList" item="where" separator=" ">
            ${where}
        </foreach>
    </if>
    <if test="groupByList != null">
    group by
        <foreach collection="groupByList" item="groupBy" separator=",">
            ${groupBy}
        </foreach>
    </if>
    <if test="orderByList != null">
    order by
        <foreach collection="orderByList" item="orderBy" separator=",">
            ${orderBy}
        </foreach>
    </if>
</select>
```
执行条件：
+ 需要通过baseDynamicQuery指定各种查询条件。
    + columnList：字段列表
    + table： 要查询的表
    + leftJoinList： 做外链接列表
    + whereList：条件列表
    + groupByList：分组列表
    + orderByList： 排序列表

特点：
+ 可以执行任意sql

缺点：
+ 关于关联查询：只适用于一对一（尚未考虑一对多的关联查询）
+ 要在Java端动态设置sql，加大代码量
+ 返回类型是固定的，所以要下点功夫解决返回值问题（见2.1）
# 2 问题解决
## 2.1 动态返回类型
mybatis的xml配置暂不支持动态设置resulttype。
### 2.1.1 返回类型设置为HashMap<String, String>
注：
+ 设置字段列表时，需要在sql里把不是string类型的转为string
+ 需要进一步将Map转为目标Class
#### Map转目标Class
写在工具类里，以便所有类似需求的地方进行统一调用。

Map转POJO步骤：
1. 将Map转为JSON。
2. 将JSON转为POJO。

或

1. 将Map直接转为POJO。

利弊：
+ Map直接转为POJO
    + 通过反射可以做到，但是需要拼接方法名之类的步骤
+ Map转JSON转POJO
    + JSON lib 提供了Map转JSON和JSON转POJO的API
    + 代码简介
##### 工具类：ConvertUtil.java
```
public class ConvertUtil {
    /**
     * convert Map<String, String> to JSONObject
     * @param Map<String, String>
     * @return JSONObject
     */
    public static JSONObject mapToJson(Map<String, String> map) {
    	return JSONObject.fromObject(map);
    }
    /**
     * convert JSONObject to any kind of Class
     * @param JSONObject
     * @param T.class
     * @return T
     */
    public static <T> T jsonToClass(JSONObject obj, Class<T> clazz) {
    	
    	@SuppressWarnings("unchecked")
		T t = (T) JSONObject.toBean(obj, clazz);
    	
    	return t;
    }
}
```
附：
+ 个人感觉也可以把返回类型设置为Map<String, Object>
    + 没有时间进行验证，不知道可不可行
