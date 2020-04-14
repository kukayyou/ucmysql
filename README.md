# ucmysql
mysql操作库使用说明

1. 在配置文件中添加如下配置
  #mysql相关
  mysql_datasrc = root:quanshi@tcp(10.255.0.198:3306)/conf_props?charset=utf8mb4（根据自己的数据库配置填写）
  mysql_cons = 10 （最大连接数）
  mysql_debug = on （是否开启debug）
 
2. 程序中初始化
  cons, err := beego.AppConfig.Int("mysql_cons")
	if err != nil {
		//uclog.Error("load mysql connections config error")
		return fmt.Errorf("load mysql connections config error")
	}
	datasrc := beego.AppConfig.String("mysql_datasrc")
	if !ucmysql.InitConnectionPool(cons, datasrc) {
		//uclog.Critical("initialize connection pool fail")
		time.Sleep(time.Millisecond * 100)
		return fmt.Errorf("initialize connection pool fail")
	}
	sqlDebug := beego.AppConfig.DefaultBool("mysql_debug", true)
	if sqlDebug {
		ucmysql.OpenDebug()
	} else {
		ucmysql.CloseDebug()
	}
