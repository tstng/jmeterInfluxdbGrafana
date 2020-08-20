# Grafana con InfluxDB para JMeter
Usar JMeter con lenguaje en Inglés, preferiblemente. Al parecer hay bugs cuando los test plans son creados en JMeter en español.
<br>Luego de desplegar InfluxDB en nuestro cluster de K8s, se deben ejecutar los siguientes comandos:

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

[Aprovisionamiento de Grafana mediante configFiles](https://grafana.com/docs/grafana/latest/administration/provisioning/)

## Dashboards JMeter con InfluxDB
[Apache JMeter Dashboard using Core InfluxdbBackendListenerClient](https://grafana.com/grafana/dashboards/5496)
[JMeter Load Test](https://grafana.com/grafana/dashboards/1152)
[Mas dashboards de JMeter para Grafana](https://grafana.com/grafana/dashboards?search=jmeter)

# Consideraciones
- Los datos se grafican en tiempo real, mientras la prueba se ejecuta.
- El dashboard depende del BackendListener configurado en el TestPlan.
- Algunos dashboards dependen de plugins `.jar` que se deben de instalar en la ruta `/lib/ext/`.
- la carpeta `dashboards_JMeter` contiene el backup de un dashboard para JMeter, pero este ya se encuentra volcado dentro del manifiesto `1grafana_configmap.yml`
