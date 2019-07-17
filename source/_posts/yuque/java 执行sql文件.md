
---

title: java 执行sql文件

date: 2019-04-17 19:57:14 +0800

tags: []

---
<a name="42550742"></a>
# # 背景

用例执行完毕，期望回滚数据，因此希望执行sql来回滚数据

<a name="65638618"></a>
# # 步骤

直接show代码，借助的是mybatis的ScriptRunner


```java
/**
     * 执行xx库下的表备份脚本
     *
     * @param tableName
     */
    public static void runSqlInStat(String tableName) {

        String className = Configurations.INSTANCE.get("jdbc.xx.driver");
        String dbUrl = Configurations.INSTANCE.get("jdbc.xx.url");
        String dbUsername = Configurations.INSTANCE.get("jdbc.xx.username");
        String dbPassword = Configurations.INSTANCE.get("jdbc.xx.password");

        try {
            Class.forName(className);
            Connection conn = DriverManager.getConnection(dbUrl, dbUsername, dbPassword);
            ScriptRunner runner = new ScriptRunner(conn);
            runner.setAutoCommit(true);

            String fileName = String.format("src/main/resources/db/%s.sql", tableName);
            File file = new File(fileName);

            try {
                if (file.getName().endsWith(".sql")) {
                    runner.setFullLineDelimiter(false);
                    runner.setDelimiter(";");//语句结束符号设置
                    runner.setLogWriter(null);//日志数据输出，这样就不会输出过程
                    runner.setSendFullScript(false);
                    runner.setAutoCommit(true);
                    runner.setStopOnError(true);
                    runner.runScript(new InputStreamReader(new FileInputStream(fileName), "utf8"));
                    logger.info(String.format("【%s】回滚成功", tableName));
                }
            } catch (Exception e) {
                e.printStackTrace();
            }

            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {

            e.printStackTrace();
        }

    }
```


