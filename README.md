## 二兔兔公司后台管理与SSL虚拟专用网项目开发

1. 完成了前台展示页面
2. 后台以及VPN已经着手设计方案了

## 需求分析

1. 在用户登录之后，需要客户端配合登录到公司内网办公
2. 需要自动配置统一上网出口（网关）来访问内网应用，并对内网流量做特殊标记
3. 后台管理系统要针对前端展示做更改操作

## backup

创建VPN服务端

```go
// @router /vpn [get]
func (c *WsController) Proxy() {
	server := shadowproxy.NewServer("D77Admin!@#")
	s := websocket.Server{
		Handler: websocket.Handler(server.Handler),
		Handshake: func(*websocket.Config, *http.Request) (err error){
			return nil
		},
	}
	s.ServeHTTP(c.Ctx.ResponseWriter, c.Ctx.Request)
	c.Data["json"] = "exit"
	c.ServeJSON()
}
```