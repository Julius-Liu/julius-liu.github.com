---
layout: post
title: mybatis-spring启动过程和调用过程
categories: blog
tags: [mybatis]
description: mybatis-spring启动过程和调用过程
---

## mybatis-spring 可以为我们做什么 ##

mybatis框架已经很不错了，它把配置和执行sql的通用过程抽象出来。只要你符合mybatis框架的要求，首先有正确的配置，然后有model，interface层，sql语句，还有bean定义让interface和sql关联起来，那么当你执行interface中的方法的时候，mybatis框架就会为你找到对应的sql的可执行的statement，然后执行并返回结果。

可是这样还不够，最大的问题在于许多bean定义，必须一个一个手写，并且要保证interface和sql的名称和位置要填写正确。mybatis-spring最大的贡献在于它的bean扫描机制，只要注解使用正确，那么它可以为你自动扫描所有interface和sql语句，并且建立bean定义让它们关联起来。Spring还可以为动态的Bean定义创建缓存，这非常酷。

另外，既然融入了Spring框架，那么mybatis配置信息和SqlSessionFactory之类的信息，也可以用 Spring Bean 来管理。Spring 可以在它的层面上为它们做一些缓存。

## mybatis执行sql的完整流程 ##

我们回顾一下在单一的mybatis的机制中，配置的加载和sql的执行的完整流程。

```
// 解析配置文件，生成配置
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);

// 根据配置，构建一个SqlSessionFactory
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);  

// 得到一个真正可用的SqlSession
SqlSession sqlSession = sqlSessionFactory.openSession();

// 从SqlSession获取interface的代理
ArticleMapper articleMapperProxy = sqlSession.getMapper(ArticleMapper.class);

// 执行代理类中的方法
Article article = articleMapperProxy.selectByPrimaryKey("123");

// 以下省略对 article 的操作

```

## mybatis-spring 启动过程 ##

我们先来说说mybatis-spring框架的启动过程。mybatis-spring 决定接管 sqlSessionFactory 和 sqlSession，并且为 sqlSession 创建代理类 sqlSessionProxy。除此之外，mybatis-spring 决定扫描所有interface层的mapper，然后接管所有 mapper 的代理类。2条线我们分开来看。

### 线1：sqlSessionFactory 和 sqlSession 的初始化 ###

1. 创建 sqlSessionFactoryBean，这是一个被Spring管理的工厂bean
2. 创建 sqlSessionFactory，这是一个 Spring 管理的 Bean，属于 mybatis 范畴
3. 创建 sqlSessionTemplate，其中使用到了 sqlSessionFactory，属于 mybatis-spring 范畴
4. 创建 sqlSessionProxy

SqlSessionFactory是一个十分重要的工厂类，让我们来回顾一下SqlSessionFactory中有哪些信息：

```
# sqlSessionFactory 中的重要信息

sqlSessionFactory
    configuration
        environment        # 里面有 dataSource 信息
        mapperRegistry
            config         # 里面有配置信息
            knownMappers   # 里面有所有的 mapper
        mappedStatements   # 里面有所有 mapper 的所有方法
        resultMaps         # 里面有所有 xml 中的所有 resultMap
        sqlFragments       # 里面有所有的 sql 片段
```

这些信息非常重要，在不久的将来创建 sqlSessionProxy 和 将来创建 mapperProxy 的时候，都需要使用里面的信息。

在 mybatis-spring 框架中，sqlSessionFactory由Spring管理，让我们来看一下 sqlSessionFactory 是如何创建出来的。

```
@Bean(name = "sqlSessionFactory")
public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
    SqlSessionFactoryBean factory = new SqlSessionFactoryBean();
    factory.setDataSource(dataSource);
    if (StringUtils.hasText(this.properties.getConfig())) {
        factory.setConfigLocation(this.resourceLoader.getResource(this.properties.getConfig()));
    } else {
        if (this.interceptors != null && this.interceptors.length > 0) {
            factory.setPlugins(this.interceptors);
        }
        factory.setTypeAliasesPackage(this.properties.getTypeAliasesPackage());
        factory.setTypeHandlersPackage(this.properties.getTypeHandlersPackage());
        factory.setMapperLocations(this.properties.getMapperLocations());
    }
    return factory.getObject();
}
```

注意：`SqlSessionFactoryBean` 是一个工厂bean，它的作用就是解析mybatis 配置（数据源、别名等），然后通过 getObject方法返回一个 SqlSessionFactory 实例。我们先看下SqlSessionFactoryBean是在初始化的时候作了哪些工作。

让我们来看一下 SqlSessionFactoryBean 的源码：

```
public class SqlSessionFactoryBean 
    implements FactoryBean<SqlSessionFactory>, InitializingBean, 
        ApplicationListener<ApplicationEvent> {
    private static final Log LOGGER = LogFactory.getLog(SqlSessionFactoryBean.class);
    private Resource configLocation;
    private Configuration configuration;
    private Resource[] mapperLocations;
    private DataSource dataSource;
    private TransactionFactory transactionFactory;
    private Properties configurationProperties;
    private SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
    private SqlSessionFactory sqlSessionFactory;
    private String environment = SqlSessionFactoryBean.class.getSimpleName();
    private boolean failFast;
    private Interceptor[] plugins;
    private TypeHandler<?>[] typeHandlers;
    private String typeHandlersPackage;
    private Class<?>[] typeAliases;
    private String typeAliasesPackage;
    private Class<?> typeAliasesSuperType;
    private DatabaseIdProvider databaseIdProvider;
    private Class<? extends VFS> vfs;
    private Cache cache;
    private ObjectFactory objectFactory;
    private ObjectWrapperFactory objectWrapperFactory;

    public SqlSessionFactoryBean() {
    }
    ...
}
```

我们可以看到，这个类实现了FactoryBean、InitializingBean和ApplicationListener接口，对应的接口在bean初始化的时候又执行了一些特定的方法，此处不再展开。现在来看看都有哪些重要的方法会被执行，这些方法又做了哪些工作。

```
// FactoryBean中的方法
public SqlSessionFactory getObject() throws Exception {
    if (this.sqlSessionFactory == null) {
        this.afterPropertiesSet();
    }

    return this.sqlSessionFactory;
}
```

通过观察代码，getObject方法最终返回 sqlSessionFactory，如果 sqlSessionFactory 为空就会执行 afterPropertiesSet 中的 buildSqlSessionFactory 构建sqlSessionFactory，在构建sqlSessionFactory时mybatis会去解析配置文件，构建configuation。后面的onApplicationEvent主要是监听应用事件时做的一些事情（不展开，有兴趣的同学可以自己去了解下）。

我们来看看 afterPropertiesSet 方法是怎么将属性设置进去的：

```
// InitializingBean中的方法
public void afterPropertiesSet() throws Exception {
    Assert.notNull(this.dataSource, "Property 'dataSource' is required");
    Assert.notNull(this.sqlSessionFactoryBuilder, "Property 'sqlSessionFactoryBuilder' is required");
    Assert.state(this.configuration == null && this.configLocation == null || this.configuration == null || this.configLocation == null, "Property 'configuration' and 'configLocation' can not specified with together");
    
    // 看到了我们熟悉的build方法
    this.sqlSessionFactory = this.buildSqlSessionFactory();
}
```

buildSqlSessionFactory 的主要方法是：this.sqlSessionFactoryBuilder.build(configuration); 此处不再展开。到这里为止，sqlSessionFactory 已经创建完成，下面我们来简单看看 sqlSessionTemplate 的创建过程：

```
@Bean
@ConditionalOnMissingBean
public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory) {
    return new SqlSessionTemplate(sqlSessionFactory, this.properties.getExecutorType());
}
```

如果跟踪进去，就会发现 new SqlSessionTemplate 的同时，会创建 sqlSessionProxy，此处不再展开。

### 线2：mapper扫描和代理类的创建 ###

1. 使用 MapperScannerConfigurer
2. scan() 扫描mapper
3. setBeanClass
4. 得到 MapperFactoryBean
5. MapperFactoryBean.getObject() 方法
6. configuration.getMapper
7. getMapper
8. MapperRegistry
9. knownMappers
10. 最终得到一组 MapperProxy，他们是原始 mapper 的代理。MapperProxy 实现了 InvocationHandler 接口，其中有 invoke 方法，在实际调用的时候执行。

说明：对于第4点，`MapperFactoryBean` 是一个工厂bean，在spring容器里，工厂bean是有特殊用途的，当spring将工厂bean注入到其他bean里时，它不是注入工厂bean本身，而是调用bean的getObject方法。

## mybatis-spring 调用过程 ##

# mybatis-spring 调用过程

1. 调用 mapper 中的方法

2. MapperProxy     invoke
- 2.1 if 判断
- 2.2 else if 判断
- 2.3 cachedMapperMethod   重点方法
- 2.4  mapperMethod.execute   重点方法，实际执行的方法

3. 根据sql语句的类型，分情况处理
- case INSERT
- case UPDATE
- case DELETE
- case SELECT
- case FLUSH

4. 以 SELECT 情况举例，将会执行 sqlSession.<E> selectList

5. sqlSessionTemplate.sqlSessionProxy.<E> selectList

6. SqlSessionInterceptor       invoke
- 6.1 getSqlSession，用到了 sqlSessionFactory，使用了sqlSessionHolder 技术，有就拿一个，没有就新建一个。无论如何，都会有一个 sqlSession
- 6.2 defaultSqlSession.selectList
重点，之后是 query -> queryFromDatabase -> doQuery -> 1. prepareStatement   2. execute
- 6.3 closeSqlSession

我们可以发现，在 mybatis-spring 框架中，真正 sqlSession 的创建，是在调用interface中的方法的时候才进行的。更细节的过程可以参考上文。

## 参考资料 ##

- https://segmentfault.com/a/1190000015165470

<br/>

<div align="right">06/08/2019 21:00 </div>
