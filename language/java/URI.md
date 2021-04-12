# URI

> URI：通用资源标志符 -- Uniform Resource Identifier。
> 

### <font color="green">URI结构</font>

[schema:]schema-specific-part[#fragment]

### <font color="green">样例</font>

`http://www.java2s.com:8080/yourpath/fileName.htm?stove=10&path=32&id=4#harvic`

- scheme:匹对上面的两个Uri标准形式，很容易看出在：前的部分是scheme，所以这个Uri字符串的sheme是：http
- scheme-specific-part:很容易看出scheme-specific-part是包含在scheme和fragment之间的部分，也就是包括第二部分的[//authority][path][?query]这几个小部分，所在这个Uri字符串的scheme-specific-part是：//www.java2s.com:8080/yourpath/fileName.htm?stove=10&path=32&id=4 ，注意要带上//，因为除了[scheme:]和[#fragment]部分全部都是scheme-specific-part，当然包括最前面的//；
- fragment:这个是更容易看出的，因为在最后用#分隔的部分就是fragment，所以这个Uri的fragment是：harvic
下面就是对scheme-specific-part进行拆分了；
在scheme-specific-part中，最前端的部分就是authority，？后面的部分是query，中间的部分就是path
- authority：很容易看出scheme-specific-part最新端的部分是：www.java2s.com:8080
- query:在scheme-specific-part中，？后的部分为：stove=10&path=32&id=4
- path:在**query:**在scheme-specific-part中，除了authority和query其余都是path的部分:/yourpath/fileName.htm
又由于authority又一步可以划分为host:port形式，其中host:port用冒号分隔，冒号前的是host，冒号后的是port，所以：
- host:www.java2s.com
- port:8080