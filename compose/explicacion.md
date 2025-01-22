
# Pasos para arrancar el clúster MySQL Group Replication

## 1. Levantar los contenedores
Ejecuta el siguiente comando para iniciar todos los servicios definidos en el archivo `docker-compose.yml` de manera desacoplada:
```bash
docker-compose up -d
```

---

## 2. Acceder al contenedor del nodo 1
Conéctate al contenedor `mysql-node1` para ejecutar comandos dentro del cliente MySQL:
```bash
docker exec -it mysql-node1 mysql -uroot -p
```

---

## 3. Habilitar Group Replication en el nodo 1
1. Instala el plugin de replicación grupal:
   ```sql
   INSTALL PLUGIN group_replication SONAME 'group_replication.so';
   ```

2. Inicializa el grupo configurando el nodo 1 como el bootstrap del clúster:
   ```sql
   SET GLOBAL group_replication_bootstrap_group=ON;
   ```

3. Inicia la replicación grupal en el nodo 1:
   ```sql
   START GROUP_REPLICATION;
   ```

4. Desactiva el modo bootstrap en el nodo 1 para evitar conflictos futuros:
   ```sql
   SET GLOBAL group_replication_bootstrap_group=OFF;
   ```

---

## 4. Unir los otros nodos al grupo
Para cada nodo adicional (`mysql-node2`, `mysql-node3`), sigue estos pasos:

1. Accede al contenedor del nodo correspondiente (ejemplo para `mysql-node2`):
   ```bash
   docker exec -it mysql-node2 mysql -uroot -p
   ```

2. Dentro del cliente MySQL, instala el plugin de replicación grupal:
   ```sql
   INSTALL PLUGIN group_replication SONAME 'group_replication.so';
   ```

3. Inicia la replicación grupal en el nodo:
   ```sql
   START GROUP_REPLICATION;
   ```

---

## 5. Verificar el estado del clúster
Conéctate a cualquiera de los nodos y ejecuta este comando en MySQL para confirmar que todos los nodos están activos en el clúster:
```sql
SELECT * FROM performance_schema.replication_group_members;
```

Este comando muestra una lista de los nodos activos en el clúster, incluyendo sus IDs y estado actual.

### Salvate con 
```sql
docker exec -it mysql-node1 ping mysql-node2
```
### en el node 1 y soon jeje
```sql
docker exec -it mysql-node1 /bin/bash
```
### Si no te funca 
``` 
microdnf install -y iputils
```
