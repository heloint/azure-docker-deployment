COMMAND THAT MIGHT BE USED
==========================

# DOCKER


## 1.1 BUILD FROM Dockerfile

* DO NOT FORGET THE CONTEXT DIRECTORY!!! THE DOT "." !!! *

```bash
docker build -t <name_for_the_new_image> .
```

## 1.2 RUN AN IMAGE DETACHED

```bash
docker container run --name <name_for_container> -dit <image_name> bash
```

## 1.3 RUN AN IMAGE DETACHED ALTERNATIVELY A DIRECTORY MOUNTED INSIDE

```bash
docker container run \
    --name <name_for_container> 
    --mount src=</source/dir/absolute/path>,target=</target/dir/absolute/path>,type=bind
    -dit <image_name> bash
```

## 2. COPY FILES TO CONTAINER

```bash
docker cp </source/dir/absolute/path> <container_name>:</target/dir/absolute/path>
```

## 3. COMMIT A CONTAINER AS AN IMAGE (MOUNTED DIRECTORY WON'T BE AVAILABLE!)

```bash
docker commit <container_name> <name_for_the_new_image>:</target/dir/absolute/path>
```

---

# MICROSOFT AZURE

## 1. CREATE RESOURCE GROUP

```bash
az group create --name <group_name> --location <location_name, like francecentral or westeurope>
```

## 2. ADD APP SERVICE PLAN TO RESOURCE GROUP

```bash
az appservice plan create --name <appservice_name> --resource-group <r_group_name from 1.> --is-linux --sku F1
```

## 3. ADD EMPTY WEBAPP TO APP SERVICE PLAN OF A RESOURCE GROUP

```bash
az webapp create --name <webapp_name> --resource-group <r_group_name from 1.> --plan <appservice_name from 2.> --runtime 'PYTHON|3.9'
```

## 4.1. ASSIGN A DOCKER IMAGE TO THE PREVIOUSLY CREATED WEBAPP. (VIA GITLAB OR DIRECTLY FROM DOCKERHUB)

*VIA GITLAB*

```bash
az webapp config container set --docker-custom-image-name registry.gitlab.com/xtec/flask --name <name_of_app> --resource-group <r_group_name from 1.>
```

*DIRECTLY VIA DOCKERHUB*

```bash
az webapp config container set --docker-custom-image-name registry.gitlab.com/xtec/flask --name <name_of_app> --resource-group <r_group_name from 1.>
```

## 4.2 DEPLOY TO THE PREVIOUSLY CREATED WEBAPP BUT NOT AS DOCKER, JUST PLAIN PYTHON WEB APP

### 1. ASSIGN DEPLOYMENT PARAMETERS TO ENVIRONMENT VARIABLES FABRICATED FROM A TSV OUTPUT
==============================

```bash
export APPNAME=$(az webapp list --query [0].name --output tsv)
```

```bash
export APPRG=$(az webapp list --query [0].resourceGroup --output tsv)
```

```bash
export APPPLAN=$(az appservice plan list --query [0].name --output tsv)
```

```bash
export APPSKU=$(az appservice plan list --query [0].sku.name --output tsv)
```

```bash
export APPLOCATION=$(az appservice plan list --query [0].location --output tsv)
```

### 2. START THE DEPLOYMENT BY EXECUTING THE FOLLOWING COMMAND FROM THE ROOT DIRECTORY DIRECTORY OF THE APPLICATION*
==============================
```bash
az webapp up --name $APPNAME --resource-group $APPRG --plan $APPPLAN --sku $APPSKU --location "$APPLOCATION"
```
