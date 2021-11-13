# Create db develop
db.createUser({user: 'mongodb_develop', pwd: 'mongodb_develop', roles:[{role:'dbOwner', db: 'test_db_develop'}]});
# Create secret for develop
kubectl create secret generic demo-db-develop-secret --from-literal=DB_USER=mongodb_develop --from-literal=DB_PASSWORD=mongodb_develop

