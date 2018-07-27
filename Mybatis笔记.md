    Hello World   配置文件式:
	1.  导入jar包
	2. 配置环境：log4j.properties，mybatis-config.xml(配置数据库环境，datasource)
	3. 读取配置文件创建SqlSessionFactory对象
	4. 建立Mapper映射文件，xxxMapper.xml：包括编写sql语句，mapper名称空间
	5. 通过SqlSessionFactory建立SqlSession对象
	6. 通过SqlSession对象调用selectOne或selectList方法调用xxxMapper.xml映射文件中的Sql语句并执行返回结果

    接口方式（主要）：
	1. 导入jar包
	2. 配置环境：log4j.properties，mybatis-config.xml(配置数据库环境，datasource)
	3. 读取配置文件创建SqlSessionFactory对象
	4. 建立Mapper映射文件，xxxMapper.xml：包括编写sql语句，mapper名称空间
	5. 创建xxxMapper同名接口，编写接口方法
	6. 将mapper映射文件的命名空间绑定为接口类全名，将Sql语句id绑定为接口方法名
	7. 通过SqlSessionFactory建立SqlSession对象
	8. 通过SqlSession对象获取接口实现对象mapper
	9. 通过mapper对象调用接口方法


	* SqlSession代表和数据库的一次回话，用完必须关闭。
	* SqlSession和connection都是非线程安全的。每次使用都应该去获取新的对象。
	*  mapper接口没有实现类，但是mybatis会为接口生成一个代理对象（通过将接口与xml绑定起来实现）
	* 两个重要的配置文件：
	* mybatis-config.xml全局配置文件，抱哈数据库连接池信息，事务管理器信息等，系统运行环境信息
	* sql映射文件，保存了每一个sql语句的映射信息，将sql抽取出来。

    全局配置文件
	* properties:用来引入外部properties配置文件的内容(与Spring整合后使用不多）

		* 参数：resource:引入类路径下的资源
		* url：引入网络的资源
	* setting:
	* typeAliases：别名处理器，可以为Java类型起别名（了解）

		* 参数：type:指定要起别名的类型全类名；默认别名为类名小写
		*              alias:指定新的别名
		* 还可以为包批量别名
		* 在类名上面加上注解@Alias("alias")也可以某个类型指定新的别名

@Alias("author")public class Author {...}
	* 还有框架已经起好的别名


	* typeHandlers：类型处理器，对Java与数据库的数据类型进行映射   
	* plugins（插件）
	* 允许你在已映射语句执行过程中的某一点进行拦截调用。 默认情况下，MyBatis 允许使用插件来拦截的方法调用包括：

		* Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
		* ParameterHandler (getParameterObject, setParameters)
		* ResultSetHandler (handleResultSets, handleOutputParameters)
		* StatementHandler (prepare, parameterize, batch, update, query)
	* environments（环境配置）

		* 每一个environment可以配置一个具体的环境信息，两个必要参数：id代表当前环境的唯一标识

			* transactionManager：事务管理器（与Spring整合后由Spring来实现事务管理和数据源管理）
			* dataSource：数据源
	* mappers（映射器）：将写好的sql映射注册到全局配置中

		* mapper：每一个mapper标签注册一个sql映射

			* 参数：resource：引用类路径下的sql映射文件

				* url：引用网络路径或者磁盘路径下的sql映射文件
				* （注册接口）class：引用接口

					* ·有sql映射文件，映射文件名必须与接口同名，且两者放在同一目录下
					* 没有sql映射文件，所有的sql都是用注解写在接口上（重要的接口尽量使用映射文件而普通的可以用注解方式）
		* package：批量注册接口

			* 参数：name = "接口全路径"

映射文件
	*    Sqlsession的增删改方法是需要手动commit的。创建SqlSession方法opensession(true)中的参数true表示自动提交
	* mybatis允许增删改直接定义一下类型返回值：Integer,Long,Boolean，void
	* 获取自增主键的值：mybatis利用statement.getGenreatedKeys（），在映射文件sql语句中加入如下语句useGeneratedKeys="true"，keyProperty属性指定获取到的主键值封装给javabean的某个属性
	* Mybatis参数处理：

		* 单个参数情况下不做任何处理
		* 多个参数： 多个参数会被封装成一个map（key：param1...paramn或者参数索引；values：传入的参数值），#{}就是从map中获取指定的key的值

			* 建议封装参数时明确指定key的值，通过@Parma("key")参数来实现

public AddrInfo getAddrInfo(@Param("corpId")int corpId, @Param("addrId")int addrId);
	* POJO：如果多个参数正好是业务逻辑的数据模型，这样可以直接穿入pojo       #{属性名}取出属性值
	* MAP：如果多个参数不是业务逻辑的数据模型，为了方便也可以传入map       #{key}取出map中的值
	* TO：如果多个参数已然不是业务逻辑的数据模型，但是又经常使用，可以编写一个TO（Transfer Object）数据传输对象

Page{    int index;    int size;}        


			* 特别注意，如果是Collection(List、Set)类型或是数组，也会特殊处理，也是把传入的list或者数组封装在map中

				* key：Collection（collection），如果是List，key就是list
		* 参数值的获取：

			* 除了#{ }还有${ }，区别：# { }是以预编译的方式，将参数设置到sql语句中，PreparedStatement

				* ${ }是直接将参数值拼装到sql语句中，会有安全问题（如sql语句注入），大多数情况应该使用#{ }，原生jdbc不支持占位符的地方可以使用${ }
			* #{ }更丰富的用法：

				* 规定参数的一些规则：
	* select标签：

		* resultType，如果返回的是一个集合，要写集合中的元素的类型

			* 返回一条记录的map对象，key就是列名。values就是记录值
			* 返回多条记录封装一个map，Map<Inetege，User>要求键是记录的主键，值是封装的javabean。可以在接口方法上加上注释@MapKey（"id"）即可。
		* resultMap:自定义结果集映射规则

			* resultMap标签：自定义某个javabean的封装规则

				* 属性：type：javabean类全路径

					* id：唯一标识
				* id标签：指定主键列的封装规则；column属性指定哪一列；peoperty指定对应的javabean属性
				* result标签：定义普通列的封装规则；column属性指定哪一列；property指定对应的javabean的属性
			* 关联查询：利用resultMap标签对关联查询结果集的属性进行封装

				* association标签：

					* association标签（中同样有id，result，标签）可以指定联合的javabean对象；属性property 指定哪个属性是联合的对象，属性javaType 指定这个属性对象的类型（不可省略）
					* association还能用于分布查询：select属性：表明当前属性是调用select指定的方法（方法的全路径）查出的结果，column属性指定将哪一列的值传给该方法，查出的对象封装给property属性
					* 延迟加载：在分段查询的基础上加上两个配置，在全局配置文件settings标签中加入两个setting标签，第一个属性name 为lazyLoadingEnabled，value为true，第二个属性name为aggressiveLazyLoading，value为false
				* collection标签：

					* collection：定义关联集合类型的属性的封装规则，属性peoperty指定哪个属性是联合的对象，oftrpe指定javabean类型
					* collection也可分布查询，延迟加载，类比association
					* 分布查询传递多个值是，可以先将要传递的值封装成map再传递给column属性，如column="{key1=column1,key2=column2}"
					* Tips：属性fetchType=“lazy”表示使用延迟加载，=“eager”表示立即加载
				* discriminator标签：

					* 鉴别器：mybatis可以使用discriminator判断某列的值，然后根据某列的值改变封装行为,属性：column指定要判定的列名，javaType列值对应的java类型

						* case标签：属性value判断值，javaType指定封装的结果类型

							* 插入association标签，指定分布查询

动态sql
	* if

		* test属性为判断表达式（OGNL：格式参考官方文档），假设test="id!=null"，mybatis会从参数中取出id值进行判断，特殊符号需要用转义字符来写，参照ISO-8859-1
		* where 标签：使用where标签来讲所有的查询条件包括在内，mybatis就会将where中拼装的sql中第一个多出来的and或者or自动去掉
		* trim标签：自定义字符串的截取规则，属性：

			* prefix= "" 前缀，trim标签体重视整个字符串拼串后的结果。prefix给拼串后的整个字符串加一个前缀
			* prefixOverrides=“”:前缀覆盖，去掉整个字符串前面多余的字符
			* sufffix=""：后缀
			* suffixOverrides=“”：后缀覆盖
	* choose (when, otherwise)

		* 类似switch() case break，只按顺序选取一个
	* trim (where, set)：

		* <set>标签可以自动去除插入语句中多余的字符
	* foreach

		* 属性：

			* collection：指定要遍历的集合，list类型的参数会特殊处理封装在map中，map的key就叫list
			* item：将当前遍历出的元素赋值给指定的变量
			* separator：每个元素之间的分隔符
			* open：遍历出所有结果拼接一个开始的字符
			* close：遍历出所有结果拼接一个结束的字符
			* index：索引，遍历list的时候是 索引，遍历map的时候是key，item就是map的值
			* #{变量名}取出当前遍历的元素
	* 批量插入：利用foreach来实现
	* 两个重要的内置参数：不只是方法传递过来的参数可以被用来判断取值，mybatis默认还有两个内置参数

		* _parameter：代表整个参数

			* 单个参数：_parameter就是这个参数
			* 多个参数：参数会被封装为一个map，_parameter就代表这个map
		* _databaseId：如果配置了databaseIdProvide标签，_databaseId就是代表当前数据库（mysql/oracle）的别名
	* bind标签：可以将OGNL表达式的值绑定到一个变量中，方便后来引用这个变量的值

		* 属性：

			* name：变量的名字
			* value：变量的值，如value = "'%'+'nickname'+'%'"
	* sql标签：抽取可重用的sql片段，方便后面引用

		* 抽取：将要经常查询的列名或者插入用的列名抽取出来方便引用
		* include来引用已经抽取的sql片段
	* include标签：引用外部定义的sql片段

		* include还可以自定义一些peoperty，sql片段中就能使用${ }（#{ }无法使用）获取这些property

缓存机制
	* 一级缓存:(本地缓存)：也称为sqlSession级别的缓存，一级缓存是默认一直开启的

		* 与数据库一次回话期间查询到的数据会放在本地缓存中，以后如果需要获取相同的数据，直接从缓存中拿，没必要去查询数据库
		* 四种失效情况：

			* 1.使用不同的sqlSession
			* 2.查询条件不同
			* 3.两次查询中有增删改操作
			* 4.sqlSession相同，但是手动清空的缓存。
	* 二级缓存（全局缓存）：基于namespace级别的缓存，一个namespace对应一个二级缓存。

		* 原理：sqlSession关闭时数据会被存入二级缓存。
		* 使用步骤：

			* 全局配置文件中配置，<setting name="cacheEnabled" value="true"></setting>
			* 去mapper.xml中配置使用二级缓存，<cache></cache>标签，属性：

				* evction：缓存回收策略
				* flushInterval：缓存刷新间隔：默认不清空，可手动设置，毫秒为单位
				* readOnly：是否只读：

					* true：只读，mybatis认为所有从缓存中获取数据的操作都是只读操作，不会修改数据，mybatis为了加快获取速度，直接就会将数据在缓存中的引用交给用户，不安全，速度快
					* false（默认）：非只读，mybatis局的获取的数据可能会被修改，mybatis会利用序列化&反序列化技术克隆一份新的数据给你。安全，速度慢
				* size：缓存存放多少元素
				* type：指定自定义实现的全类名，实现cache接口即可。
			* 我们的pojo需要实现序列化接口（impletments Serializable）。
			* 和缓存有关的设置/属性：

				*  cacheEnabled=true、false，关闭缓存（二级缓存关闭，一级缓存一直可用）
				* 每个select标签都有useCache="true"、"false"（不使用缓存，一级缓存依然使用，但二级缓存不使用）
				* 每个增删改标签的flushCache="true"，增删改执行完成后就会清空缓存，一级二级都会清空。
				* Sqlsession.clearCache()；只会清除当前session的一级缓存
				* localCacheScope，本地缓存作用域（一级缓存SESSION），当前会话的所有数据保存在会话缓存中

					* STATEMENT可以禁用一级缓存。
	* 第三方缓存ehCache

逆向工程
	* 逆向工程可以针对单表自动生成mybatis执行需要的代码（mapper.java,mapper.xml,pojo）
	* 建立代码自动生成配置文件并修改相关内容
	* 执行java程序


