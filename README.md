# caddy_with_dnspod

这是一个基于Caddy构建的Docker镜像，集成了DNSPod DNS插件。

## 项目说明

本项目基于Caddy 2.9.1版本构建，并集成了DNSPod DNS插件（[caddy-dns/dnspod](https://github.com/caddy-dns/dnspod)），使得Caddy可以自动自动申请托管在dnspod上域名的泛域名证书。

## 使用方法

### 1. 拉取镜像

```bash
docker pull jorbenzhu/caddy_with_dnspod:2.9.1
```

### 2. 配置Caddyfile

创建一个`Caddyfile`，示例如下：

```caddyfile
example.com {
    tls {
        dns dnspod {env.DNSPOD_API_TOKEN}
    }
    reverse_proxy localhost:8080
}
```

### 3. 运行容器

```bash
docker run -d \
    --name caddy \
    -p 80:80 \
    -p 443:443 \
    -v /path/to/Caddyfile:/etc/caddy/Caddyfile \
    -e DNSPOD_API_TOKEN=your_api_token \
    your-registry/caddy_with_dnspod:latest
```

## 环境变量

- `DNSPOD_API_TOKEN`: DNSPod API Token，用于DNS验证
- 您可以直接在配置中用实际的鉴权令牌替换{env.DNSPOD_API_TOKEN}，其格式为"APP_ID,APP_TOKEN"，而不必使用环境变量

## 构建镜像

如果您想自己构建镜像，可以使用以下命令：

```bash
docker build -t your-registry/caddy_with_dnspod:latest .
```

## 注意事项

1. 请确保您有有效的DNSPod API Token
2. 建议在生产环境中使用环境变量或Docker secrets来管理API Token
3. 确保您的域名已经在DNSPod上正确配置

## 许可证

本项目采用Apache 2.0许可证。详见[LICENSE](LICENSE)文件。
