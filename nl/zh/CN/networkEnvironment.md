---

copyright:
  years: 2017, 2018
lastupdated: "2018-04-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# 网络环境
{: #networkEnvironment}

供应 {{site.data.keyword.appserver_full}} 服务实例之后，您可以采取多种方式访问 VM。您可以通过安全 VPN 进行连接，以获取 SSH、传统 WebSphere 管理控制台以及应用程序对 VM 的访问权。您还可以使用公共 IP 地址将 VM 连接到互联网。

下图显示这些网络路径：

图 1. 使用公共 IP 的多租户联网的客户机视图

![图 1. 使用公共 IP 的多租户联网的客户机视图](images/wasaas_multi_tenantPublicIP.gif)

## VPN 访问
{: #vpnAccess}

从 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板供应 WebSphere Application Server in {{site.data.keyword.Bluemix_notm}} 服务实例之后，可以通过展开相应下拉菜单并使用**下载 VPN 配置**按钮来下载 VPN 配置，从而建立 OpenVPN 连接。VPN 配置包含用于向 OpenVPN 服务器进行认证的 **.ovpn** 文件和证书。建立 OpenVPN 连接后，即可通过 SSH 来访问 VM。您还可以访问 Liberty 管理中心、传统 WebSphere 管理控制台和应用程序。

VPN 配置的作用域限定为您的组织和区域。有效期为一年，自创建时间开始计算。可以使用相同的 VPN 配置同时建立多个 OpenVPN 客户机连接。

**注：**仅当组织包含**活动**预订时，VPN 配置才有效。删除组织的最后一个预订时，将暂挂组织的所有 VPN 配置。当新预订变为活动状态时，会自动重新激活未到期的 VPN 配置。

## 高级 VPN 配置管理
{: #advancedVPN}

在大多数情况下，只需要单个 VPN 配置，此配置可以使用**下载 VPN 配置**按钮进行下载。但是，通过“高级 VPN 管理”页面（使用 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板中的**高级 VPN 管理**按钮来访问），可以创建和管理多个 VPN 配置。具有多个配置可能有助于在旧配置即将到期时，顺利过渡到新的 VPN 配置。您还可以请求多个 VPN 配置，以管理组织中不同个人或团队对 VM 的访问权。  

**注：**在任何时间，组织**最多**只能有 10 个活动 VPN 配置。

如果 VPN 配置遭到破坏或到期，那么可以使用“高级 VPN 管理”页面来撤销 VPN 配置。此外，在审计透视图中，可以查看所有 VPN 管理活动的历史记录，并下载先前在“高级 VPN 管理”页面中创建的活动 VPN 配置。

在 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板中提供的所有功能还可以使用 REST API 进行脚本编制。有关更多信息，请参阅 {{site.data.keyword.Bluemix_notm}}[REST API 文档](https://wasaas-broker.ng.bluemix.net/wasaas-broker/api#/){: new_window}中的 WebSphere Application Server。


## 公共互联网访问
{: #publicInternetAccess}

（可选）可以在 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板中管理公用因特网访问。您可以向池**请求**公共 IP 地址，然后**打开**从因特网到 WebSphere Application Server in {{site.data.keyword.Bluemix_notm}} 服务实例的连接。反之，您也可以**关闭**服务实例对因特网的访问，并将公共 IP 地址**还给**池。

要**请求**公共 IP 地址并**打开**连接，请遵循以下指示信息进行操作：

1. 在 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板上，单击**管理公共 IP 访问**。
2. 这将显示主机的 IP 地址，但不会显示公共 IP 地址。单击**请求公共 IP 地址**。

    **注：**您将返回到服务仪表板，其中包含分配的公共 IP。但是，还将显示以下消息：

    > **访问当前已关闭。单击“管理公共 IP”可打开访问。**
3. 在 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板上，单击**管理公共 IP 访问**。
4. 这将显示主机的 IP 地址和新的公共 IP，但访问已关闭。单击**打开访问**。

    **注：**您将返回到服务仪表板，其中显示以下消息：

    > **访问当前已打开。单击“管理公共 IP”可关闭访问。**

要**关闭**连接并**返回**公共 IP 地址，请遵循以下指示信息进行操作：

1. 在 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板上，单击**管理公共 IP 访问**。
2. 单击**关闭访问**。

    **注：**您将返回到服务仪表板，其中显示以下消息：

    > **访问当前已关闭。单击“管理公共 IP”可打开访问。**
3. 在 {{site.data.keyword.Bluemix_notm}} UI 的服务仪表板上，单击**管理公共 IP 访问**。
4. 单击**返回公共 IP 地址**。

    **注：**您将返回到服务仪表板，其中显示主机的 IP 地址以及以下消息：

    > **返回了公共 IP。**

## 公共 IP 端口
{: #publicIPports}

公开公共 IP 的访问权时，IP 地址将与您的 VM 关联，网关的 80 和 443 端口将打开。但是，缺省情况下，Liberty Core 和传统 WebSphere 基础服务器不打开端口 80 和 443。而 IBM HTTP Server 在缺省情况下打开端口 80 和 443。因此，在使用公共 IP 时，您可能需要配置 Liberty Core 和传统 WebSphere 基础服务器来侦听端口 80/443 上的应用程序流量。
* 要配置 Liberty Core 服务器，请参阅[为公共访问权配置 Liberty Core 服务器](networkEnvironment.html#configureLibertyForPublicAccess)。
* 要配置传统 WebSphere 基础服务器，请按[配置传输链](http://www.ibm.com/support/knowledgecenter/SSEQTP_8.5.5//com.ibm.websphere.nd.doc/ae/trun_chain_transport.html){: new_window}中所述，添加 Web 容器传输链来侦听端口 80/443。

**应避免的问题：**对于特权用户（例如 **root** 用户），Linux 会保留小于 1024 的端口。但是，以**非 root** 用户身份运行传统 WebSphere Base 服务器是一种常见做法。因此，添加 Web 容器传输链时，请以 **root** 用户身份更改 **iptables** 配置。具体来说，是将受限端口 80 和 443 重定向到大于 1024 的其他端口，例如 9080 和 9443。以下命令提供了此过程的示例：

```
-bash-4.1# sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 9080
-bash-4.1# sudo iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
-bash-4.1# sudo iptables -I INPUT -p tcp -m tcp --dport 9080 -j ACCEPT

-bash-4.1# sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 9443
-bash-4.1# sudo iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT
-bash-4.1# sudo iptables -I INPUT -p tcp -m tcp --dport 9443 -j ACCEPT
```

**注：**对 **iptables** 的更改是临时的。例如，如果重新引导了访客，或者如果重新启动了 **iptables** 服务，那么会自动清空并重置规则。要保存规则，以便在启动 iptables 服务或重新引导访客时持久保存这些规则，请以 **root** 用户身份使用以下命令：

```
-bash-4.1# service iptables save
iptables: Saving firewall rules to /etc/sysconfig/iptables:[  OK  ]

bash-4.1# cat /etc/sysconfig/iptables
# Generated by iptables-save v1.4.7 on Wed May 31 19:44:11 2017
*nat
:PREROUTING ACCEPT [0:0]
:POSTROUTING ACCEPT [23:1706]
:OUTPUT ACCEPT [23:1706]
-A PREROUTING -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 9080
-A PREROUTING -p tcp -m tcp --dport 443 -j REDIRECT --to-ports 9443
COMMIT
# Completed on Wed May 31 19:44:11 2017
# Generated by iptables-save v1.4.7 on Wed May 31 19:44:11 2017
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT DROP [0:0]
-A INPUT -p tcp -m tcp --dport 9443 -j ACCEPT
-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
-A INPUT -p tcp -m tcp --dport 9080 -j ACCEPT
-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT

```


**注：****iptables** 会在通过访客外部接口传递的请求上进行调用。通过本地回送 (127.0.0.1) 传递的请求不会由 **iptables** 进行处理，因此上述重定向端口的操作不会通过回送进行调用。

## VPN 专用 IP 端口
{: #privateIPports}

您可以通过 VPN 连接到 VM 专用 IP 地址。Liberty 管理中心（9080、9443）、传统 WebSphere 管理控制台（9060、9043）、SSH (22) 以及 80/443 之外的其他端口均仅可以通过图 1 描述的 VPN 连接进行访问。有关将 Liberty 管理中心从应用程序端口分开的详细信息，请参阅样本 Liberty Core **server.xml** 和 **ibm-web-bnd.xml**。

**应避免的问题：**对于 Liberty Core 和传统 WebSphere 基础服务器，供应 VM 时可以预配置防火墙端口。但是，对于 Deployment Manager 或 Collective 控制器与 IBM HTTP Server 并置的 Network Deployment 配置，您可能需要在防火墙上打开端口。请参阅[防火墙端口](systemAccess.html#firewall_ports)以获取详细信息。

## 针对公共 IP 访问配置 Liberty Core 服务器
{: #configureLibertyForPublicAccess}

在使用公共 IP 时，您需要配置 Liberty Core 来侦听端口 80/443 上的应用程序流量。

缺省情况下，使用 Liberty 管理中心和 **default_host** 虚拟主机上可用的应用程序配置 Liberty，该虚拟主机与端口 9080 和 9443 上的 **defaultHttpEndpoint** 相关联。重新配置服务器，以将 Liberty 管理中心与应用程序虚拟主机和端点区分开来，使得它们可用于不同的端口上。

以下片段是调整 server.xml 配置的示例：

```    
    <!-- open port 9080/9443 for incoming http connections -->
    <httpEndpoint id="defaultHttpEndpoint"
        host="*"
        httpPort="9080"
        httpsPort="9443">
        <tcpOptions soReuseAddr="true"/>
    </httpEndpoint>

    <!-- define a new endpoint for public app traffic -->
    <httpEndpoint id="publicHttpEndpoint"
        host="*"
        httpPort="80"
        httpsPort="443">
        <tcpOptions soReuseAddr="true"/>
    </httpEndpoint>

    <!–- restrict default_host to vpn so the Liberty Admin Center is not public -->
    <virtualHost id="default_host" allowFromEndpointRef="defaultHttpEndpoint">
      <hostAlias>*:9080</hostAlias>
      <hostAlias>*:9443</hostAlias>
    </virtualHost>

    <virtualHost id="external_host">
      <hostAlias>*:80</hostAlias>
      <hostAlias>*:443</hostAlias>
    </virtualHost>
```
{: codeblock}

现在通过将以下片段包含在应用程序的 **WEB-INF/ibm-web-bnd.xml** 文件中，将应用程序与 **external_host** 虚拟主机相关联：

```
    <?xml version="1.0" encoding="UTF-8"?>
    <web-bnd
        xmlns="http://websphere.ibm.com/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://websphere.ibm.com/xml/ns/javaee   
        http://websphere.ibm.com/xml/ns/javaee/ibm-web-bnd_1_0.xsd"
        version="1.0">

        <virtual-host name="external_host" />
    </web-bnd>
```
{: codeblock}

## 配置 DNS
{: #dnsConfig}

请务必注意，WebSphere Application Server in {{site.data.keyword.Bluemix_notm}} VM 配置有两个 DNS 解析器。解析器是在 VM 上的 **/etc/resolv.conf** 文件中进行配置的。主 DNS 服务器是 WebSphere Application Server in {{site.data.keyword.Bluemix_notm}} 服务提供的非权威服务器。辅助 DNS 服务器是 {{site.data.keyword.Bluemix_notm}} 提供的非权威服务器。

我们建议您查看 DNS 配置以了解它是否满足您的需求。如果 IBM 提供的 DNS 服务器不满足您的需求，您可以更新 **/etc/resolv.conf** 文件来引用所选的 DNS 服务器。

**注：**较旧的 WebSphere Application Server in {{site.data.keyword.Bluemix_notm}} VM 可能仅在 **/etc/resolv.conf** 文件中配置了主 DNS 服务器。如果需要 HA 解决方案，那么可以释放 VM 并供应新的 VM，或者更新 **/etc/resolv.conf** 文件以添加辅助 DNS 服务器。这可以是您的首选 DNS 服务器，也可以是 {{site.data.keyword.Bluemix_notm}} (10.0.80.11) 提供的 DNS 服务器。


## 为定制节点上的新服务器打开端口
{: #serversOnCustom}

在定制节点上创建服务器时，必须先在 Deployment Manager 和定制节点 VM 上打开服务器所需的更多端口，然后再启动服务器。创建服务器后，但在启动服务器之前，先通过运行 `openWASPorts.sh` 脚本来打开端口：

 1. 以 root 用户身份登录到每个 Deployment Manager 和定制 VM。
 1. 运行 `/opt/IBM/WebSphere/AppServer/virtual/bin/openWASPorts.sh` 脚本以打开端口。
 
运行脚本后，可以从 Deployment Manager 管理控制台启动服务器。