# 外部应用对网格服务访问

本页说明如何从一个外部应用访问集群内部的服务网格。

前置条件：

- 服务 `bookinfo.com` 运行于网格 `global-service` 的 `default` 命名空间下

- 网格提供 `egressgatway` 网关实例

配置目标：

1. 实现内部服务 `bookinfo.com` 对外暴露。

2. 通过 URI 匹配方式，实现外部应用对服务 `bookinfo.com` 的指定页面访问路由。

    ![img](../../images/out-to-in01.png)

3. 点击`流量治理`->`网关规则`->`创建`为 istio 网关创建网关规则，对外暴露服务及端口。

    ![img](../../images/out-to-in02.png)
    
    完成配置后的 YAML 示例如下：

    ```yaml
    apiVersion: networking.istio.io/v1beta1
    kind: Gateway
    metadata:
    name: bookinfo-gateway
    spec:
    selector:
        istio: ingressgateway # 使用默认控制器
    servers:
    - port:
        number: 80
        name: http
        protocol: HTTP
        hosts:
        bookinfo.com
    ```

4. 点击`确定`回到网关规则列表，可见创建成功。

    ![img](../../images/out-to-in03.png)

5. 点击`流量治理`->`虚拟服务`->`创建`来创建路由规则，基于请求中的 URI 路由到指定页面。

    ![img](../../images/out-to-in04.png)

    完成配置后的 YAML 示例如下：

    ```yaml
    apiVersion: networking.istio.io/v1beta1
    kind: VirtualService
    metadata:
    name: bookinfo
    spec:
    hosts:
    - bookinfo.com
    gateways:
    - bookinfo-gateway
    http:
    - match:
        - uri:
            exact: /productpage
        - uri:
            exact: /login
        - uri:
            exact: /logout
        - uri:
            prefix: /api/v1/products
        route:
        - destination:
            host: productpage
            port:
            number: 9080
    ```

6. 点击`确定`回到虚拟服务列表，可以看到创建成功的提示。

    ![img](../../images/out-to-in05.png)
