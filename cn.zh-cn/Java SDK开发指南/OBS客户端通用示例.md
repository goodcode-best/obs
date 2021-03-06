# OBS客户端通用示例<a name="ZH-CN_TOPIC_0142815557"></a>

使用OBS客户端进行接口调用操作完成后，没有异常抛出，则表明返回值有效，返回[SDK公共响应头](SDK公共响应头.md)实例或其子类实例；若抛出异常，则说明操作失败，此时应从[SDK自定义异常](SDK自定义异常.md)实例中获取错误信息。

以下代码展示了使用OBS客户端的通用方式：

```
// 您的工程中可以只保留一个全局的ObsClient实例
// ObsClient是线程安全的，可在并发场景下使用
ObsClient obsClient = null; 
try
{
    String endPoint = "https://your-endpoint";
    String ak = "*** Provide your Access Key ***";
    String sk = "*** Provide your Secret Key ***";
    // 创建ObsClient实例
    obsClient = new ObsClient(ak, sk, endPoint);
    // 调用接口进行操作，例如上传对象
    HeaderResponse response = obsClient.putObject("bucketname", "objectname", new File("localfile"));  // localfile为待上传的本地文件路径，需要指定到具体的文件名
    System.out.println(response);
}
catch (ObsException e)
{
    System.out.println("HTTP Code: " + e.getResponseCode());
    System.out.println("Error Code:" + e.getErrorCode());
    System.out.println("Error Message: " + e.getErrorMessage());
    
    System.out.println("Request ID:" + e.getErrorRequestId());
    System.out.println("Host ID:" + e.getErrorHostId());
}finally{
    // 关闭ObsClient实例，如果是全局ObsClient实例，可以不在每个方法调用完成后关闭
    // ObsClient在调用ObsClient.close方法关闭后不能再次使用
    if(obsClient != null){
        try
        {
            // obsClient.close();
        }
        catch (IOException e)
        {
        }
    }
}
```

