# 🚀 TechRetail – Escalando hacia la Nube con Docker Swarm

Este proyecto presenta la modernización de la infraestructura de TechRetail, migrando de un servidor físico único a un clúster orquestado con **Docker Swarm**.

---

## 🛠️ Arquitectura del Sistema

| Componente       | Tecnología     | Réplicas        |
|------------------|----------------|-----------------|
| **Frontend**     | Nginx          | 3 – 5 réplicas  |
| **Backend**      | Node.js        | 2 réplicas      |
| **Base de Datos**| MySQL 8        | 1 (con volumen) |
| **Cache**        | Redis          | 1               |
| **Visualizer**   | Swarm Visualizer | 1             |

---

## 🚀 Instrucciones de Despliegue

### 0️⃣ Clonar el repositorio

```bash
git clone https://github.com/EmersonFQ7/TechRetail-Project.git
cd TechRetail-Project
```

### 1️⃣ Inicializar el clúster

```bash
docker swarm init
```

> 💡 Si tienes múltiples interfaces de red, especifica la IP del nodo Manager:
> ```bash
> docker swarm init --advertise-addr <IP_MANAGER>
> ```

### 2️⃣ Crear el secreto para la Base de Datos

```bash
echo "TuPasswordSegura" | docker secret create db_password -
```

### 3️⃣ Desplegar el stack

```bash
docker stack deploy -c docker-compose.yml techretail
```

### 4️⃣ Verificar que los servicios estén corriendo

```bash
docker stack services techretail
```

---

## 📈 Escalado Dinámico

Para aumentar el número de réplicas del frontend:

```bash
docker service scale techretail_frontend=5
```

Para verificar el estado de las réplicas:

```bash
docker service ps techretail_frontend
```

---

## 📸 Monitoreo

Accede al visualizador del clúster en:

```
http://localhost:8080
```

Para revisar los logs de un servicio en tiempo real:

```bash
docker service logs techretail_backend
```

---

## 🌐 Gestión del Clúster (Multi-Nodo)

Si deseas agregar nodos Worker al clúster:

```bash
# Obtener el token para Workers
docker swarm join-token worker

# En el nodo Worker, ejecutar el comando resultante:
docker swarm join --token <TOKEN> <IP_MANAGER>:2377

# Ver todos los nodos del clúster
docker node ls
```

---

## 🧹 Limpieza

Para eliminar el stack desplegado:

```bash
docker stack rm techretail
```

Para que un nodo abandone el clúster:

```bash
docker swarm leave --force
```

---

## 📋 Referencia Rápida de Comandos

| Acción                              | Comando                                                    |
|-------------------------------------|------------------------------------------------------------|
| Inicializar Swarm                   | `docker swarm init --advertise-addr <IP_MANAGER>`          |
| Obtener token de Worker             | `docker swarm join-token worker`                           |
| Unir nodo Worker                    | `docker swarm join --token <TOKEN> <IP_MANAGER>:2377`      |
| Ver nodos del clúster               | `docker node ls`                                           |
| Crear un secret                     | `echo "password" \| docker secret create db_password -`   |
| Desplegar el stack                  | `docker stack deploy -c docker-compose.yml techretail`     |
| Ver servicios del stack             | `docker stack services techretail`                         |
| Ver réplicas de un servicio         | `docker service ps techretail_frontend`                    |
| Escalar un servicio                 | `docker service scale techretail_frontend=5`               |
| Ver logs de un servicio             | `docker service logs techretail_backend`                   |
| Eliminar el stack                   | `docker stack rm techretail`                               |
| Salir del clúster                   | `docker swarm leave --force`                               |

---


---

> **Nota:** Asegúrate de tener Docker Engine 20.10+ instalado y que el daemon de Docker esté corriendo antes de ejecutar cualquier comando.