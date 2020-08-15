# Grafana con InfluxDB para JMeter
Usar JMeter con lenguaje en Inglés, preferiblemente. Al parecer hay bugs cuando los test plans son creados en JMeter en español.
Luego de desplegar InfluxDB en nuestro cluster de K8s, se deben ejecutar los siguientes comandos:

1. Nos conectamos a la consola de influx dentro del POD de InfluxDB:
```
kubectl exec -it [influxdb-xxxxxxx] influx
```
2. Creamos la base de datos:
```
CREATE DATABASE jmeter
```

3. Validamos que haya sido creada:
```
show databases
```

4. Validamos los usuarios creados:
```
show users
```

5. Si no está el usuario que definimos en el secret, lo creamos:
```
CREATE USER [NombreUsuario] WITH PASSWORD '[Password]'
```

6. Damos permisos a la base de datos:
```
GRANT ALL ON jmeter TO [NombreUsuario]
```

Si queremos darle permisos de admin sobre el servidor:
```
GRANT ALL PRIVILEGES TO [NombreUsuario]
```

# Ejecutando pruebas desde JMeter CLI
Emplearemos el comando:
```
 ./apache-jmeter-5.3/bin/jmeter -n -t LoadTest.jmx -f -o Results.xml -Jhostname=[webSite]
```
----------
# Documentos y Recursos
##  Instalación y configuración InfluxDB y Grafana:
[Administración InfluxDB](https://influxdbcom.readthedocs.io/en/latest/content/docs/v0.9/administration/administration/)

[Deploy InfluxDB and Grafana on Kubernetes to collect Twitter stats](https://opensource.com/article/19/2/deploy-influxdb-grafana-kubernetes)

## Conectar JMeter con InfluxDB
[JMeter Load Test](https://grafana.com/grafana/dashboards/1152)

## Tableros de JMeter para Grafana
[Dashboards JMeter](https://grafana.com/grafana/dashboards?search=jmeter)

# Conclusiones
- Los datos se grafican en tiempo real, mientras la prueba se ejecuta.
- El dashboard depende del BackendListener configurado en el TestPlan.
- 
