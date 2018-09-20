### 常见开发问题

#### Mapper 类需要放到下面包中，不然mapper无法加载
```
@MapperScan("com.konka.bds.electroniccard.dao")

# 常见的错误提示：
Field XXX  that could not be found.
Consider defining a bean of type 'XX ‘ in your configuration.
```
