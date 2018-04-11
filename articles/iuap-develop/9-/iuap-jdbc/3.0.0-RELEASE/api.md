**iuap-jdbc组件api接口介绍**

组件dao类BaseDAO


<table style="border-collapse:collapse">
	<tr>
		<th>方法名</th>
		<th>参数</th>
		<th>返回值</th>
		<th>功能说明</th>
	</tr>

	<tr>
		<td>queryByPK</td>
		<td>Class&lt;T&gt; className（查询实体类类型）, ID pk（需要查询的实体类的主键键值）</td>
		<td> T（查询到的实体类）</td>
		<td>通过主键查询实体</td>
	</tr>

	<tr>
		<td>queryByPK</td>
		<td>Class&lt;T&gt; className（查询实体类类型）, ID pk（需要查询的实体类的主键键值），String[] selectedFields（指定实体的返回列）</td>
		<td> T（查询到的实体类）</td>
		<td>通过主键查询实体（指定实体的返回列）</td>
	</tr>

	<tr>
		<td>queryByPK</td>
		<td>Class&lt;T&gt; className（查询实体类类型）, ID pk（需要查询的实体类的主键键值），String[] selectedFields（指定实体的返回列）</td>
		<td> T（查询到的实体类）</td>
		<td>通过主键查询实体（指定实体的返回列）</td>
	</tr>

	<tr>
		<td>queryByClause</td>
		<td>Class&lt;T&gt; className（查询实体类类型）,  String sql（查询的sql语句）</td>
		<td>List&lt;T&gt;（查询到的实体列表）</td>
		<td>通过sql语句查询实体</td>
	</tr>

	<tr>
		<td>queryByClause</td>
		<td>Class&lt;T&gt; className（查询实体类类型）,  String sql（查询的sql语句），SQLParameter parameter（sql语句拼接的参数）</td>
		<td>List&lt;T&gt;（查询到的实体列表）</td>
		<td>通过sql语句查询实体</td>
	</tr>

	<tr>
		<td>queryAll</td>
		<td>Class&lt;T&gt; className（查询实体类类型）</td>
		<td>List&lt;T&gt;（查询到的实体列表）</td>
		<td>查询该实体的所有记录</td>
	</tr>

	<tr>
		<td>queryForList</td>
		<td>String sql(查询sql语句), SQLParameter parameter（sql参数）, ResultSetProcessor processor(查询结果解析器)</td>
		<td>List&lt;T&gt;（返回结果列表）</td>
		<td>根据查询结果解析器返回相应的查询结果列表</td>
	</tr>

	<tr>
		<td>queryForList</td>
		<td>String sql(查询sql语句),ResultSetProcessor processor(查询结果解析器)</td>
		<td>List&lt;T&gt;（返回结果列表）</td>
		<td>根据查询结果解析器返回相应的查询结果列表</td>
	</tr>

	<tr>
		<td>queryForObject</td>
		<td>String sql(查询sql语句), SQLParameter parameter（sql参数）,ResultSetProcessor processor(查询结果解析器)</td>
		<td>T（返回结果）</td>
		<td>根据查询结果解析器返回相应的查询结果</td>
	</tr>
	
	<tr>
		<td>queryForObject</td>
		<td>String sql(查询sql语句),ResultSetProcessor processor(查询结果解析器)</td>
		<td>T（返回结果）</td>
		<td>根据查询结果解析器返回相应的查询结果</td>
	</tr>

	<tr>
		<td>queryPage</td>
		<td>String sql(查询sql语句), SQLParameter parameter(查询sql参数), PageRequest pageRequest(分页请求参数), Class&lt;T&gt; type（查询实体类类型）</td>
		<td>Page&lt;T&gt;（返回结果分页）</td>
		<td>分页查询</td>
	</tr>

	<tr>
		<td>insert</td>
		<td>T t(待插入的实体)</td>
		<td>ID(插入实体类的主键键值)</td>
		<td>插入新实体</td>
	</tr>

	<tr>
		<td>insert</td>
		<td>List&lt;T&gt; vos(待插入的实体列表)</td>
		<td>ID[](插入实体类的主键键值列表)</td>
		<td>插入多个新实体</td>
	</tr>

	<tr>
		<td>insert</td>
		<td>List&lt;T&gt; vos(待插入的实体列表)</td>
		<td>ID[](插入实体类的主键键值列表)</td>
		<td>插入多个新实体</td>
	</tr>

	<tr>
		<td>insertWithPK</td>
		<td>T vo(待插入的实体)</td>
		<td>ID(插入实体类的主键键值)</td>
		<td>插入新实体（自带主键键值）</td>
	</tr>
	
	<tr>
		<td>insertWithPK</td>
		<td>List&lt;T&gt; vos(待插入的实体列表)</td>
		<td>ID[](插入实体类的主键键值列表)</td>
		<td>插入多个新实体（自带主键键值）</td>
	</tr>

	<tr>
		<td>insertOptional</td>
		<td>BaseEntity vo（实体）</td>
		<td>ID（实体主键）</td>
		<td>存储实体数据（选择性保存字段）</td>
	</tr>

	<tr>
		<td>insertOptional</td>
		<td>List<? extends BaseEntity> list（实体列表）</td>
		<td>ID[]（实体主键列表）</td>
		<td>存储实体数据（选择性保存字段）</td>
	</tr>

	<tr>
		<td>insertOptionalWithPK</td>
		<td>BaseEntity vo（实体）</td>
		<td>ID（实体主键）</td>
		<td>存储实体数据自带主键键值（选择性保存字段）</td>
	</tr>

	<tr>
		<td>insertOptionalWithPK</td>
		<td>List<? extends BaseEntity> list（实体列表）</td>
		<td>ID[]（实体主键列表）</td>
		<td>存储实体数据自带主键键值（选择性保存字段）</td>
	</tr>

	<tr>
		<td>update</td>
		<td>T vo(更新实体)</td>
		<td>int (更新的行数)</td>
		<td>更新数据</td>
	</tr>

	<tr>
		<td>update</td>
		<td>List&lt;T&gt; vos(更新实体列表)</td>
		<td>int (更新的行数)</td>
		<td>更新数据</td>
	</tr>

	<tr>
		<td>update</td>
		<td>List &lt;T&gt; vos(更新实体列表)</td>
		<td>int (更新的行数)</td>
		<td>更新数据</td>
	</tr>

	<tr>
		<td>update</td>
		<td>T vo(更新实体)，String... fieldNames（需要更新的实体字段）</td>
		<td>int (更新的行数)</td>
		<td>更新数据的特定字段</td>
	</tr>

	<tr>
		<td>update</td>
		<td>List&lt;T&gt; vos(更新实体列表)，String... fieldNames（需要更新的实体字段）</td>
		<td>int (更新的行数)</td>
		<td>更新多个数据的特定字段</td>
	</tr>

	<tr>
		<td>update</td>
		<td>String sql（sql语句）</td>
		<td>int (更新的行数)</td>
		<td>通过sql进行更新</td>
	</tr>

	<tr>
		<td>update</td>
		<td>String sql（sql语句）, SQLParameter parameter（sql函数）</td>
		<td>int (更新的行数)</td>
		<td>通过sql进行更新</td>
	</tr>

	<tr>
		<td>updateOptional</td>
		<td>BaseEntity vo(待更新实体类)</td>
		<td>int (更新的行数)</td>
		<td>更新实体数据（选择性保存字段）</td>
	</tr>
	
	<tr>
		<td>updateOptional</td>
		<td>List<? extends BaseEntity> list（实体列表）</td>
		<td>int (更新的行数)</td>
		<td>更新多个实体数据（选择性保存字段）</td>
	</tr>

	<tr>
		<td>remove</td>
		<td>T vo(待删除的实体)</td>
		<td>void</td>
		<td>删除数据</td>
	</tr>
	
	<tr>
		<td>remove</td>
		<td>List&lt;T&gt; vos(待删除的实体列表)</td>
		<td>void</td>
		<td>批量删除数据</td>
	</tr>

	<tr>
		<td>save</td>
		<td>BaseEntity parent(主实体), BaseEntity... children（子实体）</td>
		<td>void</td>
		<td>存储主子实体</td>
	</tr>

</table>