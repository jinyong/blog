# 替换符
<|<=|>|>=|&|'|"
-|-|-|-|-|-|-
```&lt;```|```&lt;=```|```&gt;```|```&gt;=```|```&amp;```|```&apos;```|```&quot;```

例子：
```
create_date_time &gt;= #{startTime} and  create_date_time &lt;= #{endTime}
```

# ```<![CDATA[]]>```
例子：
```
create_date_time <![CDATA[ >= ]]> #{startTime} and  create_date_time <![CDATA[ <= ]]> #{endTime}
```
