# springboot整合pagehelper实现分页

第一步

```xml
<dependency>
<groupId>com.github.pagehelper</groupId>
<artifactId>pagehelper</artifactId>
<version>5.1.2</version> 
</dependency>
```

导入依赖

第二步

```java
public PageInfo findPage(int page,int pageSize){
  PageHelper.startPage(page,pageSize);
  List<Company> List=companyDao.selectAll();
  PageInfo pageInfo = new PageInfo(list);
  return pageInfo;
 }
```

在查询方法里写入PageHelper.startPage(page.getPage(), page.getRows());

参数：当前页码，每页查询条数

返回的信息就是pageInfo对象，该类是插件里的类，这个类里面的属性还是值得看一看

```java
public class PageInfo<T> implements Serializable {
private static final long serialVersionUID = 1L;
//当前页
private int pageNum;
//每页的数量
private int pageSize;
//当前页的数量
private int size;
//由于startRow 和endRow 不常用，这里说个具体的用法
//可以在页面中"显示startRow 到endRow 共size 条数据"
//当前页面第一个元素在数据库中的行号
private int startRow;
//当前页面最后一个元素在数据库中的行号
private int endRow;
//总记录数
private long total;
//总页数
private int pages;
//结果集
private List<T> list;
//前一页
private int prePage;
//下一页
private int nextPage;
//是否为第一页
private boolean isFirstPage = false;
//是否为最后一页
private boolean isLastPage = false;
//是否有前一页
private boolean hasPreviousPage = false;
//是否有下一页
private boolean hasNextPage = false;
//导航页码数
private int navigatePages;
//所有导航页号
private int[] navigatepageNums;
//导航条上的第一页
private int navigateFirstPage;
//导航条上的最后一页
private int navigateLastPage;
}
```

项目中实战

后端

```java
@RequestMapping("works")
    public String works(@RequestParam(required = false, defaultValue = "1", value = "pagenum") int pagenum, Model model) {
        
        List<Admin> works = service.findAllWorks();
        //分页查询
        PageHelper.startPage(pagenum, 10);  //pagenum：页数，pagesize:每页的信息数
        PageInfo<Admin> pageInfo = new PageInfo<>(works); //得到分页结果对象      
        model.addAttribute("pageInfo", pageInfo);  //携带分页结果信息
        return "admin/works";
    }
```

前端

```html

<li><a th:href="@{/admin/bulletinList}">首页</a></li>
<li><a th:href="@{/admin/bulletinList(pagenum=${pageInfo.hasPreviousPage}?${pageInfo.prePage}:1)}">上一页</a></li>
<li><a th:href="@{/admin/bulletinList(pagenum=${pageInfo.hasNextPage}?${pageInfo.nextPage}:${pageInfo.pages})}">下一页</a></li>
<li><a th:href="@{/admin/bulletinList(pagenum=${pageInfo.pages})}">尾页</a></li>                           
                                
<p >当前第<span th:text="${pageInfo.pageNum}"></span>页，总<span th:text="${pageInfo.pages}"></span>页，共<span th:text="${pageInfo.total}"></span>条记录</p>
                             
```

