# MyBatis接口式编程
## 一、原始方法:
```
public class MyBatisTest{
    public void MyTest(){
        String resource = "mybatis-config.xml";
        IntputStream intputStream = Resources.getResourceAsStream();
        SqlSessionFactory factory = newSqlSessionFactoryBuilder().bulid(intputStream);
        SqlSession sqlSession = factory.openSession();
        /** selectOne(String statement, Object parameter):
                statement :即sql映射文件中的namespace和id构成:namespace.id
                parameter :传递给sql映射文件的值
         */
        Product p = sqlSession.selectOne("org.test.example.ProductMapper.selectProduct", 1L);
    }
}

```
```
<!-- sql映射文件 -->
<mapper namespace="org.test.example.ProductMapper">
	<select id="selectProduct" resultType="mytest.dao.Product">
		SELECT * FROM product p WHERE p.id = #{id};
	</select>
</mapper>
```

## 二、接口式编程
### 1.创建Mapper接口
```
public interface ProductMapper {
	public Product getProductById(Long id);
}
```
### 2.sql映射文件中的namespace属性改为Mapper接口的全限定名,id改为Mapper接口中对应的方法
```
<!-- sql映射文件 -->
<mapper namespace="mytest.dao.ProductMapper">
	<select id="getProductById" resultType="mytest.dao.Product">
		SELECT * FROM product p WHERE p.id = #{id};
	</select>
</mapper>
```
### 3.调用sqlSession对象中的getMapper(Class<T> type)方法来绑定Mapper接口
```
public class MyBatisTest{
    public void MyTest(){
        String resource = "mybatis-config.xml";
        IntputStream intputStream = Resources.getResourceAsStream();
        SqlSessionFactory factory = newSqlSessionFactoryBuilder().bulid(intputStream);
        SqlSession sqlSession = factory.openSession();
        /** getMapper(Class<T> type)
            type: Mapper interface class.传入Mapper接口class绑定接口
         */
        ProductMapper mapper = sqlSession.getMapper(ProductMapper.class);
        Product p = mapper.getProductById(1L);
    }
}
```
