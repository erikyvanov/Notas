# Importar y Exportar Datos

## Para importar/Exportar JSON

- mongoimport

  - Manda los datos a la db

    ```bash
    mongoimport --uri "<mongo uri>" --drop=<filename>.json
    ```

    

- mongoexport

  - Exporta datos, los descarga a un directorio

    ```bash
    mongoexport --uri "<cluster-uri> sin la contrasena" --collection="<collection name>" --out=<filename>.json
    ```

  ## Para importar/Exportar BSON

  

- mongorestore

  - Manda los datos a la db

    ```bash
    mongorestore --uri "uri" --drop dump
    ```

    

- mongodump

  - Exporta datos, , los descarga a un directorio

    ```bash
    mongodump --uri "<mongo-uri>"
    ```

    

