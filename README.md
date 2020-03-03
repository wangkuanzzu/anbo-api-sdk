# anbo-api-sdk 使用说明
## 运行环境
1. JDK版本1.8及以上
2. 编译工具maven

## 获取SDK方式
### 一、源码编译
    1.git clone https://github.com/xyz2020/anbo-api-sdk.git
    2.maven下载依赖
    3.maven clean 
    4.maven compile
    5.maven package生成target目录以及其中文件sign-*.jar
    6.将jar包引入项目

### 二、直接下载编译后的jar包文件
下载地址：
> https://github.com/xyz2020/anbo-api-sdk/tree/master/download

## 私钥文件说明
> 将私钥文件放置到项目的资源文件夹（resources）下.

    注意：
    1. 文件名称要求必须是private_key.json。
    2. 该私钥文件文件必须由开发者跟项目方申请，才能做api联调，自己创建的公私钥无法联调。项目方会进行公私钥的备案。

私钥文件内容示例:
```$xslt
{
	"pub_key":"MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE1SmTD9+hRzZB4NXZnfPpRkRayW8RUQ0JeLAJEqap07C8MLe/jC4nP4b7SsNBfEzneZAdmn6gfXY7DwAXInBR6w==",
	"priv_key":"MEECAQAwEwYHKoZIzj0CAQYIKoZIzj0DAQcEJzAlAgEBBCCGk6jWcuKK/9N0M/CKEEGh84VGRlGV+sXct+i0IY09ng=="
}
```
## api-sdk方法说明及示例
### 核心方法
    1.AnboECCKey => String generateKeyPair()
    创建密钥对
    2.AnboECCSign => String sign(Map<String,String> data)
    执行签名：返回字符串。
    data：需要签名的数据，指定传入map格式。
    3.AnboECCVerify => boolean verify(String publKeyStr, Map<String,String> data, String sign)
    验证签名：返回true，验签成功；返回false，验签失败。
    publKeyStr：公钥
    data：需要签名的数据，指定传入map格式。
    sign：签名后的字符串
> 代码示例
```$java
        //签名
        AnboECCSign anboECCSign = new AnboECCSign();
        Map<String,String> map = new HashMap<>();
        map.put("accountType","1");
        map.put("address","北京市朝阳区中电发展大厦");
        map.put("name","张三");
        map.put("bankName","招商银行");
        map.put("accountNo","6225888888888888");
        map.put("accountName","张三");
        System.out.println("map:"+map);

        String sign = anboECCSign.sign(map);
        System.out.println("sign:"+sign);

```

```
        //验签
        AnboECCVerify anboECCVerify = new AnboECCVerify();
        Map<String,String> map = new HashMap<>();
        map.put("accountType","1");
        map.put("address","北京市朝阳区中电发展大厦");
        map.put("name","张三");
        map.put("bankName","招商银行");
        map.put("accountNo","6225888888888888");
        map.put("accountName","张三");
        String sign = "MEQCIGyJWpeBk+i64XTF9c4TE96LP+W4O4BYHSBFLEEYIC6VAiB8XYSItDaiaDgzvpsehs1RQ54l30h2YPYwF87Z5kC/nQ==";
        String pubKey = "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAE1SmTD9+hRzZB4NXZnfPpRkRayW8RUQ0JeLAJEqap07C8MLe/jC4nP4b7SsNBfEzneZAdmn6gfXY7DwAXInBR6w==";
        Boolean check = anboECCVerify.verify(pubKey,map,sign);
        System.out.println("check:"+check);
```