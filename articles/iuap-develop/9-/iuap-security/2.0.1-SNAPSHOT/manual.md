# iuap-security组件简介 #

iuap-security组件主要提供安全访问时候请求的签名、验证，传输简单数据加密解密，编码解码等功能，包含对Hmac、RSA等算法的封装，方便业务开发使用。组件通过可配置的方式提供给开发人员使用，在调用端和服务端分别做信息的摘要和验证，确保数据的完整性，防止恶意篡改。

组件引入OWASP组织的开源项目ESAPI，利用ESAPI，平台实现了对跨站脚本攻击（XSS），sql脚本注入等安全问题的预防。ESAPI逐步成为各大公司对Web应用程序安全保障的手段。

ESAPI具有一个安全接口集，其对每一种安全控制有一种参考实现并支持自定义实现。组件在ESAPI的基础上进行扩展。加入了对HTML、CSS、JavaScript、XML、URL等编码与解码，信息的加密解密，信息的摘要签名，防sql注入等功能的API，方便了业务开发人员使用。

# 使用说明 #

## 组件配置 ##

	<dependency>
	  <groupId>com.yonyou.iuap</groupId>
	  <artifactId>iuap-security</artifactId>
	  <version>${iuap.modules.version}</version>
	</dependency>

${iuap.modules.version} 为平台在maven私服上发布的组件的version。

## 属性配置文件说明 ##

- application.properties
		
		#RSA数字签名算法，目前JDK支持MD2withRSA,MD5withRSA,SHA1withRSA,都是1024bits
		UAP.DigitalSignatureAlgorithm=SHA1withRSA
		
		#RSA秘钥长度1024/2048
		UAP.DigitalSignatureKeyLength=1024
		
		#随机算法
		UAP.RandomAlgorithm=SHA1PRNG
		
		#HMAC摘要算法，目前jdk支持：HmacSHA1 (160 bits), HmacSHA256 (256 bits), HmacSHA384 (384 bits),HmacSHA512 (512 bits).
		UAP.KDF.PRF=HmacSHA1
		
		#签名或者摘要算法目前支持HMAC、RSA.
		UAP.AUTH.ALG=HMAC
		
		#客户端身份文件路径
		bpm.client.credential.path=/etc/authfile_bpm.txt
		
		#如果客户端需要用多个服务提供的证书，可以通过appid来区分不同的证书目录
		#appid.client.credential.path=/etc/authfile_appid.txt

示例工程中的antisamy-esapi.xml、ESAPI.properties、validation.properties在jar包中已经包含，如果没有需要覆盖的属性，则不需要添加。ESAPI.properties中如果需要覆盖Encryptor.MasterKey和Encryptor.MasterSalt的值，请参考示例工程中的setMasterKey.bat，生成新值，并覆盖到classpath中的ESAPI.properties中。

## RestAPI签名和验签 ##

**1.1:在请求端，根据URL参数（含ts值）、请求端IP地址、表单参数以及Content-Length的大小进行生成签名sign**

	SignProp prop = SignPropGenerator.genSignProp(带ts的完整URL);
	signProp.setIp(请求端IP地址);
	/************post请求时需要**************/
	signProp.setPostParamsStr(PostParamsHelper.genParamsStrByMap(POST表单参数Map));
	signProp.setContentLength(Content-Length的值);
	/***************************************/
	String sign = ClientSignFactory.getSigner().sign(prop);

注意：如果配置多份客户端证书，需要在getSinger的时候指定前缀：

	ClientSignFactory.getSigner("bpm")	


**1.2:在服务端，首先比较当前时间ts’与ts的大小，如果其差值不大于DEFAULT_EXPIRED，则从HTTP请求中获得时间戳ts、URL参数、请求的来源IP地址、表单参数以及ContentLength的大小生成sign’。此时如果sign与sign’相同，则签名验证通过。**

	SignProp prop = SignPropGenerator.genSignProp(带ts的完整URL);
	signProp.setIp(请求端IP地址);
	/************post请求时需要**************/
	signProp.setPostParamsStr(PostParamsHelper.genParamsStrByMap(HttpServletRequest对象));
	signProp.setContentLength(HttpServletRequest中取得的Content-Length的值);
	/***************************************/
	DemoServerVirifyFactory factory = new DemoServerVirifyFactory();
	Boolean result = factory.getVerifier(appid).verify(sign, prop);


**1.3:请求端的签名凭证由服务器端生成：对不同的请求端，都由服务端为其生成唯一的签名凭证，服务端维护一份，客户端维护一份。在请求端，签名凭证的存储位置需要在application.properties中进行配置**

	#客户端身份文件路径
	client.credential.path=/authfile.txt

注意：如果同一个客户端需要对发向不同服务端的不同类型的请求进行签名，需要为每种类型的服务配置不同的证书，可以用前缀来区分，配置示例如下：

	bpm.client.credential.path=/etc/authfile_bpm.txt
    osp.client.credential.path=/etc/authfile_osp.txt


**1.4:由于服务端不止维护一个请求端的签名凭证，所以当签名的请求到来时，需要根据appId得到与之对应的请求端签名凭证。即：在验签时，使用者需继承ServerVerifyFactory，重写获取某一请求端签名凭证的逻辑genCredential()**

	/**
	 * 服务端必须实现ServerVerifyFactory，用来获取某个请求端的签名凭证
	 */
	class DemoServerVirifyFactory extends ServerVerifyFactory {
		@Override
		protected Credential genCredential(String appId) {
			try {
				// 应该根据appid查询并构造
				return ClientCredentialGenerator.loadCredential(PropertyUtil.getPropertyByKey(AuthConstants.CLIENT_CREDENTIAL_PATH));
			} catch (UAPSecurityException e) {
				logger.error(e);
			}
			return null;
		}
	}

## 编码和解码、加密和解密 ##

**2.1:数据编码分为多种形式，如html编码、javascript编码、css编码、sql编码等，它们主要是用来解决各类注入问题：xss注入、sql注入。除此之外，还有Base64编码，常用来将非ASCII字符的数据转换成ASCII字符**

	//html编码
	String safe =IUAPESAPI.encoder().htmlEncode( “input”);
	//属性编码
	String safe =IUAPESAPI.encoder().htmlAttributeEncode ( “input”);
	//js编码
	String safe =IUAPESAPI.encoder().javaScriptEncode ( “input”);
	//url编码
	String safe =IUAPESAPI.encoder().urlEncode ( “input”);
    //css编码
	String safe =IUAPESAPI.encoder().cssEncode ( “input”);
	//sql注入示例
	String queryCondition = IUAPESAPI.encoder().sqlPreparedString(conditionStr,paras, DatabaseCodec.ORACLE);
	//base64
	String base64Text = IUAPESAPI.encoder().encodeForBase64(bytes);

**2.2:对称加密算法可以用作存储加密，即将数据持久化前对数据进行加密**

	String plainText = "我是被加密的信息!";
	System.out.println("明文：" + plainText);

	String info = IUAPESAPI.encryptor().encrypt(plainText);
	System.err.println("密文：" + info);

	String decryptorInfo = IUAPESAPI.encryptor().decrypt(info);
	System.err.println("解密后得到明文：" + decryptorInfo);


**2.3:非对称加密算法可以用作传输加密，即数据从请求端发送到服务端时，对敏感数据用公钥进行加密，在服务端用私钥进行解密，这里我们提供了RSAUtils工具类进行RSA加密**

	String password = "123456";
	KeyPair gk = RSAUtils.generateKeyPair();
	System.out.println(gk.toString());
		
	RSAPublicKey publicKey = RSAUtils.getDefaultPublicKey();
	String hexM = Hex.encodeHexString(publicKey.getModulus().toByteArray());
	String hexE = Hex.encodeHexString(publicKey.getPublicExponent().toByteArray());
		
	System.out.println("hexM:" + hexM);
	System.out.println("hexE:" + hexE);
		
		
	byte[] encryptBytes = RSAUtils.encrypt(publicKey, password.getBytes());
	String encryptStr = new String(Hex.encodeHex(encryptBytes));
	System.out.println("加密结果1：" + encryptStr);
		
	RSAPublicKey publicKey2 = RSAUtils.getRSAPublidKey(hexM, hexE);
	byte[] encryptBytes2 = RSAUtils.encrypt(publicKey2, password.getBytes());
	String encryptStr2 = new String(Hex.encodeHex(encryptBytes2));
	System.out.println("加密结果2：" + encryptStr2);

更加详细的配置和示例请参考示例工程(DevTool/examples/example\_iuap_security)