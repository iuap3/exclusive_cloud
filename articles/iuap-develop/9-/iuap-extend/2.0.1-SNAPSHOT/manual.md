#JPA持久化扩展组件#

##简介##
本组件主要是对Spring data JPA和iuap-persistence组件中的JPA部分做扩展，扩展了查询条件、数据库方言识别、和事务的添加范围等功能。如果使用默认的Spring data JPA设置没有扩展的需求，可以不依赖此组件。

##配置和使用方式##
**1:如果需要扩展JPA对事务的控制，在工程上依赖组件**

    根据项目的需求，排除冲突的版本，需要调整exclusions.
    <dependency>
        <groupId>com.yonyou.iuap</groupId>
        <artifactId>iuap-extend</artifactId>
        <version>${iuap.modules.version}</version>
        <exclusions>
            <exclusion>
                <artifactId>guava</artifactId>
                <groupId>com.google.guava</groupId>
            </exclusion>
            <exclusion>
                <artifactId>cglib</artifactId>
                <groupId>cglib</groupId>
            </exclusion>
        </exclusions>
    </dependency>


**2:在对应的spring配置文件中，调整如下配置**

    <!-- Spring Data Jpa配置 -->
    <jpa:repositories base-package="com.yonyou.iuap.repository"
                      transaction-manager-ref="transactionManager"
                      factory-class="org.springframework.data.jpa.repository.support.CustomJpaRepositoryFactoryBean"
                      entity-manager-factory-ref="entityManagerFactory"/>

	
**3:在服务调用需要控制事务的方法上，使用Spring的事务控制**

注意：Spring的持久化配置文件建议只有一份，统一规范数据源和事务的控制，尽量不要声明多个事务控制器。

**4:特殊数据库和查询的扩展**

此组件中，对springside的查询和spring data jpa对数据库的兼容类进行扩展，如果需要spring data jpa扩展查询条件和特殊数据库兼容，请参考组件内部的方式

- 扩展对特殊数据库的支持
- 
	public class Hibernates {
		
		... ...
	
		/**
		 * 从DataSoure中取出connection, 根据connection的metadata中的jdbcUrl判断Dialect类型.
		 * 仅支持Oracle, H2, MySql, PostgreSql, SQLServer，如需更多数据库类型，请仿照此类自行编写。
		 */
		public static String getDialect(DataSource dataSource) {
			String jdbcUrl = getJdbcUrlFromDataSource(dataSource);
	
			// 根据jdbc url判断dialect
			if (StringUtils.contains(jdbcUrl, ":h2:")) {
				return H2Dialect.class.getName();
			} else if (StringUtils.contains(jdbcUrl, ":mysql:")) {
				return MySQL5InnoDBDialect.class.getName();
			} else if (StringUtils.contains(jdbcUrl, ":oracle:")) {
				return Oracle10gDialect.class.getName();
			} else if (StringUtils.contains(jdbcUrl, ":postgresql:")) {
				return PostgreSQL82Dialect.class.getName();
			} else if (StringUtils.contains(jdbcUrl, ":sqlserver:")) {
				return SQLServer2008Dialect.class.getName();
			} else if(StringUtils.contains(jdbcUrl, ":hsqldb:")) {
				return HSQLDialect.class.getName();
			} else {
				throw new IllegalArgumentException("Unknown Database of " + jdbcUrl);
			}
		}
	
		... ...
	}

- 扩展对特殊条件查询的支持

请参考修改DynamicSpecifications和SearchFilter类

**5:其他详细配置和参数，请参考示例工程(DevTool/examples/example\_iuap\_persistence)和JPA相关文档**