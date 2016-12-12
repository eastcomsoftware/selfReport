# selfReport
# 部署方式一
* 不引入Spring，直接将项目war包放入tomcat中，启动。
* 主要配置文件：selfReport\src\cfg_jdbc_info.properties，包含bean-开头需要注入的类，以及sml-init值决定是否初始datasource-开头配置的数据源。
* 直接使用需修改：cfg_jdbc_info.properties配置文件中datasource-开头的数据源配置，改为自己使用的数据源。

# 部署方式二
* 引入Spring，用Spring配置文件注入类。
* 需修改：将cfg_jdbc_info.properties文件中bean-开头的类在Spring配置文件中注入。
* 此外还需要在web.xml中配置


  <!-- sml-manager -->
			<servlet>
				<servlet-name>sml-servlet</servlet-name>
				<servlet-class>com.eastcom_sw.sml.manager.SmlServlet</servlet-class>
			</servlet>
			<servlet-mapping>
				<servlet-name>sml-servlet</servlet-name>
				<url-pattern>/sml/*</url-pattern>
			</servlet-mapping>

  
*	以及在Spring配置文件application*.xml中配置：

  <!-- sml-manager -->
      <bean id="jdbcFTemplate" class="com.eastcom_sw.inas.core.service.jdbc.JdbcFTemplate" init-method="init">
				<property name="jsonMapper">
					<bean class="com.eastcom_sw.inas.base.service.RcptFastJsonMapper"></bean>
				</property>
				<property name="dss">
					<map>
						<entry key="defJt" value-ref="ipmsdmDataSource"></entry>
						<entry key="srpt" value-ref="ipmsdmDataSource"></entry>
					</map>
				</property>
			</bean>		
			<bean id="smlManageService" class="com.eastcom_sw.sml.manager.service.SmlManageService">
				<property name="jdbcFTemplate" ref="jdbcFTemplate"/>
			</bean>


# 注意事项
* 为减小项目大小，已将项目依赖的导出相关jar移除，所以解压使用时还需自己引入，poi-3.8.jar相关以及opencsv.jar
