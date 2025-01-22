
# Instrucciones para probar la conexión a MySQL usando MySQL Router

## Construye la imagen 
```
docker build -t ubuntu-node20-mysql-router .
```

## Corre una instancia del contenedor pero debes tener levandod el compose del cluster de mysql 

```
docker run -it ubuntu-node20-mysql-router --network mi-red-cluster-mysql
```

## Verificar la instalación de MySQL Router
Dentro del contenedor interactivo, verifica que MySQL Router está instalado correctamente:
```
mysqlrouter --version
```
- Este comando muestra la versión instalada de MySQL Router. Asegúrate de que devuelve algo como `MySQL Router 8.x`.

---

## Configurar MySQL Router
Para conectar MySQL Router al clúster MySQL, sigue estos pasos:

### Paso 1: Autenticación inicial con un nodo del clúster
Ejecuta el siguiente comando para bootstrap de MySQL Router:
```bash
mysqlrouter --bootstrap username:password@<host>:<port>
```
- **username**: Usuario de MySQL con permisos (por ejemplo, `root`).
- **password**: Contraseña del usuario.
- **<host>:<port>**: Dirección IP y puerto de un nodo del clúster MySQL.

Ejemplo real:
```bash
mysqlrouter --bootstrap root:password@192.168.1.100:3306
```
- Esto configurará MySQL Router para conectarse al clúster MySQL y generará los archivos de configuración necesarios.

---

### Paso 2: Confirmar la configuración
Después del bootstrap, MySQL Router indicará los puertos locales configurados para las conexiones. Por ejemplo:
```
MySQL Router configured to listen on:
  Classic MySQL protocol (read/write): 127.0.0.1:6446
  Classic MySQL protocol (read-only): 127.0.0.1:6447
```
- **6446**: Puerto para conexiones de lectura y escritura.
- **6447**: Puerto para conexiones de solo lectura.

---

## Probar la conexión desde MySQL Client
Después de configurar MySQL Router, verifica la conexión al clúster usando `mysql`:

### Comando de conexión:
```bash
mysql -h 127.0.0.1 -P 6446 -u root -p
```
- **-h**: Dirección del Router (usualmente localhost o `127.0.0.1`).
- **-P**: Puerto configurado por MySQL Router para conexiones.
- **-u**: Usuario de MySQL.
- **-p**: Solicita la contraseña del usuario.

- Si la conexión es exitosa, tendrás acceso al clúster MySQL a través del Router.

---

## Acceder al contenedor para pruebas
Si deseas realizar pruebas desde dentro del contenedor:
1. Accede al contenedor:
   ```bash
   docker exec -it <container_name> /bin/bash
   ```
   - Reemplaza `<container_name>` con el nombre o ID del contenedor.
   - Esto te llevará al entorno del contenedor donde puedes ejecutar los comandos.

2. Desde el contenedor, prueba la conexión usando:
   ```bash
   mysql -h 127.0.0.1 -P 6446 -u root -p
   ```

---

## Solución de problemas
### 1. Error de conexión
- Verifica que el puerto del clúster sea accesible desde el contenedor.
- Usa `telnet` o `ping` para confirmar conectividad:
  ```bash
  telnet 192.168.1.100 3306
  ```

### 2. Firewall
- Asegúrate de que los puertos del clúster y del Router estén abiertos en el firewall.

### 3. Revisión de logs
- Inspecciona los logs de MySQL Router para más detalles:
  ```bash
  cat /var/log/mysqlrouter/mysqlrouter.log
  ```

---