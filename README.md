Java 11 is suggested

```Console
git clone https://github.com/Azure-Samples/serviceconnector-webapp-keyvault-java

cd serviceconnector-webapp-keyvault-java

mvn clean package -DskipTests 

az webapp connection create keyvault --system-identity --resource-group <group-name> --name <app-name> 

az webapp deploy --resource-group <group-name> --name <app-name> --src-path ./target/keyvault-0.0.1-SNAPSHOT.jar --type jar
```