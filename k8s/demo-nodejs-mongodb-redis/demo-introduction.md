kubectl create secret generic demo-db-secret --from-literal=DB_USER=myuser --from-literal=DB_PASSWORD=myuserpass
        envFrom:      
        - secretRef:
            name: postgres-test-secret
kubectl create secret generic demo-redis-secret --from-literal=REDIS_PASSWORD=redispass
File bash script phải được tab
Trong docker thêm command để kiểm tra kết nối tới redis
apk --update add redis 

redis-cli -h demo-redis-service -p 6379 -a redispass
# Test scale hpa by request CPU
kubectl run -it --rm --restart=Never loadgenerator --image=busybox -- sh -c "while true; do wget -O - -q https://motthang.ga; done"

# Install nfs server 
kubectl apply -f .\nfs-server\
# Get IP local service nfs-server in file nodejs-pvc.yaml
# Install nodejs-pvc
kubectl apply -f .\demo-nodejs-mongodb-redis\nodejs-pvc.yaml 
# Create secret Redis
kubectl create secret generic demo-redis-secret --from-literal=REDIS_PASSWORD=redispass
# Deploy redis service and deployment
kubectl apply -f .\demo-nodejs-mongodb-redis\redis-service-deployment.yaml
# Deploy config map to config to mongodb
kubectl apply -f .\demo-nodejs-mongodb-redis\redis-service-deployment.yaml
# Create secret ( user, password) connect to mongodb
kubectl create secret generic demo-db-secret --from-literal=DB_USER=myuser --from-literal=DB_PASSWORD=myuserpass
# Deployment nodejs
kubectl apply -f .\demo-nodejs-mongodb-redis\nodejs-deployment.yaml
# Create service nodejs
kubectl apply -f .\demo-nodejs-mongodb-redis\nodejs-service.yaml
# Config ingress nginx to public nodejs
kubectl apply -f .\ingress-nginx\choitet.ga\ingress-serivce.yaml

