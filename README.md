# 🚀 TechRetail – Escalando hacia la Nube con Docker Swarm

Este proyecto presenta la modernización de la infraestructura de **TechRetail**, migrando de un servidor físico único a un clúster orquestado con **Docker Swarm**.

---

## 🛠️ Arquitectura del Sistema

- **Frontend**: Nginx (3–5 réplicas)  
- **Backend**: Node.js (2 réplicas)  
- **Base de Datos**: MySQL 8 (persistencia con volúmenes)  
- **Cache**: Redis  
- **Visualizer**: Panel de control del clúster  

---

## 🚀 Instrucciones de Despliegue

### 1️⃣ Inicializar el clúster
```bash
docker swarm init
2️⃣ Crear el secreto para la Base de Datos
echo "TuPasswordSegura" | docker secret create db_password -

3️⃣ Desplegar el stack
docker stack deploy -c docker-compose.yml techretail

📈 Escalado Dinámico

Para aumentar el número de réplicas del frontend:

docker service scale techretail_frontend=5

📸 Monitoreo

Accede al visualizador del clúster en:

http://localhost:8080