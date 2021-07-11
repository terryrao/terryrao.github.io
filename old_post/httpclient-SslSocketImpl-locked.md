# httpclient SslSocketImpl locked
;date: 2019-04-17 10:54:40
;tags:

最近项目的 spring quartz 的时候碰到几个定时任务经常不能执行，使用 jps + jstack 将数据 dump 下来，发现以下数据有问题:

```java
"scheduler_Worker-20" #42 prio=5 os_prio=0 tid=0x00007f4a95510800 nid=0x7da2 runnable [0x00007f4b078f6000]
   java.lang.Thread.State: RUNNABLE
	at java.net.SocketInputStream.socketRead0(Native Method)
	at java.net.SocketInputStream.socketRead(SocketInputStream.java:116)
	at java.net.SocketInputStream.read(SocketInputStream.java:170)
	at java.net.SocketInputStream.read(SocketInputStream.java:141)
	at sun.security.ssl.InputRecord.readFully(InputRecord.java:465)
	at sun.security.ssl.InputRecord.readV3Record(InputRecord.java:593)
	at sun.security.ssl.InputRecord.read(InputRecord.java:532)
	at sun.security.ssl.SSLSocketImpl.readRecord(SSLSocketImpl.java:973)
	- locked <0x00000005d16161d8> (a java.lang.Object)
	at sun.security.ssl.SSLSocketImpl.performInitialHandshake(SSLSocketImpl.java:1375)
	- locked <0x00000005d1616298> (a java.lang.Object)
	at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1403)
	at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1387)
	at org.apache.http.conn.ssl.SSLConnectionSocketFactory.createLayeredSocket(SSLConnectionSocketFactory.java:275)
	at org.apache.http.conn.ssl.SSLConnectionSocketFactory.connectSocket(SSLConnectionSocketFactory.java:254)
	at org.apache.http.impl.conn.HttpClientConnectionOperator.connect(HttpClientConnectionOperator.java:123)
	at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.connect(PoolingHttpClientConnectionManager.java:318)
	at org.apache.http.impl.execchain.MainClientExec.establishRoute(MainClientExec.java:363)
	at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:219)
	at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:195)
	at org.apache.http.impl.execchain.RetryExec.execute(RetryExec.java:86)
	at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:108)
	at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
	at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
	at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:106)
  ...
	at org.quartz.core.JobRunShell.run(JobRunShell.java:202)
	at org.quartz.simpl.SimpleThreadPool$WorkerThread.run(SimpleThreadPool.java:573)
	- locked <0x00000005c3586260> (a java.lang.Object)

```

虽然线程状态是 `BUNNABLE` 的，但是在 `SllSocketImpl` 中却显示 `locked` ：

```java
at sun.security.ssl.SSLSocketImpl.readRecord(SSLSocketImpl.java:973)
	- locked <0x00000005d16161d8> (a java.lang.Object)
	at sun.security.ssl.SSLSocketImpl.performInitialHandshake(SSLSocketImpl.java:1375)
	- locked <0x00000005d1616298> (a java.lang.Object)
```

网上查询说是没有设置超时时间的原因，我查了本地代码实际上是有设置超时间时间的，但是还是出现了上面的 无限挂起的情况：

```java
RequestConfig defaultRequestConfig = RequestConfig.custom().setSocketTimeout(DepositeUtil.getConnectTimeout()).setConnectTimeout(DepositeUtil.getConnectTimeout())
                    .setConnectionRequestTimeout(DepositeUtil.getReadTimeout())
                    .build();
```



继续找发现 github 上已经有人碰到这种问题了，在 4.3.6 中修复了该 bug ，原因是 `Socket` 没有设置超时，导致 `socketRead0` 无限等待。而上述代码里并没有设置 sockectTimeOut 的时间，而在 4.3.6 版本中添加了以下代码，解决这个问题：

```java
 if (connectTimeout > 0 && sock.getSoTimeout() == 0) {
                sock.setSoTimeout(connectTimeout);
            }
```

解决办法是设置超时时间，并将库升级到 4.3.6 以后的版本。



参考：<https://github.com/apache/httpcomponents-client/commit/d954cd287dfcdad8f153e61181e20d253175ca8c#diff-4f1f0cfa92ca97f7ee68436780ce874c>

