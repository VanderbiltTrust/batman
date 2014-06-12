batman
======

A black-box testing tool for identifying access control flaws within web applications

BATMAN System Setup:

1. Check out BATMAN, WebProxy (based on webscarab), Crawljax, GenCrawler code;
2. Trace collection: 
1) Portal: receive indexing requests from WebProxy and MySQLProxy, generate index log (.log)
Configure dirs in Portal.java, mode set to MODE_LOGGING, run Portal.java;
2) WebProxy: intercept http requests/responses; send indexes to Portal
Configure dirs, set web_service in WebProxy.java to point to Portal; Run org.owasp.webscarab.Main.java; 
Configure the running WebScarab proxy to listen on 8000 port and forward requests to the actual web app server on 80 portal.
3) MySQLProxy: intercept sql queries/responses; send indexes to Portal
Install MySQL proxy and lua script according to MYSQL_Proxy_INSTALL note;
Configure the server address in /usr/local/lua-script/mysql-proxy.lua to point to Portal;
4) Crawler: configure the web server address to the WebProxy address.
2. Inference & Testing:
After trace collection, the traces should include: suppose the app name is "app"
app.log  -- indexing log, generated by Portal;
app_sql_log -- SQL queries/resposnes collected by MySQLProxy on server;
log_1/1-requestFile -- Http requests and response indexes, collected by WebProxy;
log_1/html/*.html -- Http resposnes;

Run ConstraintAnalyzer.java in BATMAN, several files should be generated: 
app_RolePolicy -- the inferred access control policy file;
app_TestProfile -- all of the generated testing vectors;

Prepare an app_LoginProfile to include the user login/role information for Testing. 
Run MySQLProxy to send SQL queries/responses to Portal at runtime;
Run Portal in the mode of MODE_TESTING, an file: app_TestLog is generated. 
Search "Summary" in app_TestLog and see the testing results. 