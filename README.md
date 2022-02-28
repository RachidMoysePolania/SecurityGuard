# SecurityGuard
Historico para las vulnerabilidades

## Integracion con ELK
Para la integracion de esta webapp podemos utilizar logstash con la siguiente configuracion:
```conf
input {
  jdbc {
  clean_run => true
    jdbc_driver_library => "/root/mysql-connector-java-8.0.28.jar" #Este conector debe ir de acuerdo con la version del mysql server
    jdbc_driver_class => "com.mysql.cj.jdbc.Driver"
    jdbc_connection_string => "jdbc:mysql://localhost:3306/vulnmanager?useSSL=false&allowPublicKeyRetrieval=true" #La configuracion del connection string siempre debe ir con ?useSSL=false&allowPublicKeyRetrieval=true
    jdbc_user => "username"
    jdbc_password => "password"
    schedule => "* * * * *"
    statement => "SELECT * from vulnerabilitymanager_task"
    use_column_value => true
    tracking_column => "taskid"
  }
}

output {
    elasticsearch{
        hosts=>["localhost:9200"]
        index=>"task"
    }
    stdout { codec => rubydebug }
}
```
Resaltar que podemos tener multiples statements para hacer multiples queries