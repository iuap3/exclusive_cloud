- ITokenProcessor token处理类接口，其实现类处理token的生成，cookies信息的生成等

<table style="border-collapse:collapse">
	<thead>
		<tr>
			<th>方法名</th>
			<th>参数</th>
			<th>返回值</th>
			<th>说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>getCookieFromTokenParameter(tp)</td>
			<td>TokenParameter token参数类，包含用户id、登录时间ts等</td>
			<td>Cookies[] token相关的Cookies数组</td>
			<td>根据当前的Token参数获取需要存储的cookies，以记录登录成功的token、登录时间等信息</td>
		</tr>
	</tbody>
</table>


- SessionManager session信息管理类，操作redis缓存管理session对应的信息，注册删除在线用户等功能

<table style="border-collapse:collapse">
	<tr>
		<th>方法名</th>
		<th>参数</th>
		<th>返回值</th>
		<th>功能说明</th>
	</tr>

	<tr>
		<td>getAllSessionAttrCache</td>
		<td>String sid（模拟Http的Session的id，Session信息的唯一标识，可以用token代替）</td>
		<td>Map<String, Object>（session对应的map集合）</td>
		<td>从redis中获取指定session下的所有值</td>
	</tr>
	<tr>
		<td>removeSessionCache</td>
		<td>String sid（模拟Http的Session的id，Session信息的唯一标识，可以用token代替）</td>
		<td>void</td>
		<td>从redis中清除session信息</td>
	</tr>
	<tr>
		<td>putSessionCacheAttribute</td>
		<td>String sid（session id）, String key（属性名）, T value（属性值）</td>
		<td><T extends Serializable> 对应value类型的返回对象</td>
		<td>向redis中放置session中的某一项属性信息</td>
	</tr>
	<tr>
		<td>putSessionCacheAttribute</td>
		<td>String sid（session id）, String key（属性名）, T value（属性值）,int timeout(此session信息的reids缓存过期时间)</td>
		<td><T extends Serializable> 对应value类型的返回对象</td>
		<td>从redis中获取session中的某一项属性信息同时设置session的有效期</td>
	</tr>
	<tr>
		<td>updateSessionCacheAttribute</td>
		<td>String sid（session id）, String key（属性名）, T value（属性值）,int timeout(此session信息的reids缓存过期时间)</td>
		<td><T extends Serializable> 对应value类型的返回对象</td>
		<td>更新session中的某一项属性值，并设置session过期时间</td>
	</tr>
	<tr>
		<td>getSessionCacheAttribute</td>
		<td>String sid（session id）, String key（属性名）</td>
		<td>Object 对应value类型的返回对象</td>
		<td>获取session中对应属性名为key的属性值</td>
	</tr>
	<tr>
		<td>removeSessionCacheAttribute</td>
		<td>String sid（session id）, String key（属性名）</td>
		<td>void</td>
		<td>清除session中对应属性名为key的信息</td>
	</tr>
	<tr>
		<td>registOnlineSession</td>
		<td>final String userid（用户标识）, final String token（token登录时候生成的值）, final ITokenProcessor processor（token处理类，shiro的配置文件中注入）</td>
		<td>void</td>
		<td>注册登录信息，登录处理类获取cookies时候会调用</td>
	</tr>
	<tr>
		<td>validateOnlineSession</td>
		<td>final String userid（用户标识）, final String token（token登录时候生成的值）</td>
		<td>boolean 是否有效标识</td>
		<td>判断传入的token是否有效</td>
	</tr>
	<tr>
		<td>delOnlineSession</td>
		<td>final String userid（用户标识）, final String token（token登录时候生成的值）</td>
		<td>void</td>
		<td>退出登录时候，删除在线的session信息</td>
	</tr>
	<tr>
		<td>deleteUserSession</td>
		<td>final String userid（用户标识）</td>
		<td>void</td>
		<td>禁用或者删除用户时，删除用户相关的信息</td>
	</tr>
</table>