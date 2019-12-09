# Instalación de MongoDB sobre Linux o Windows.
La instalación del gestor de base de datos Mongo se va a realizar sobre un sistema Debian 9. Los comandos necesarios son los siguientes:
~~~
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4

echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list

sudo apt-get update

sudo apt-get install -y mongodb-org

sudo apt-get install libcurl3 openssl
~~~

Para continuar la instalación es necesario descargar el paquete desde la página de oficial de Mongo.

~~~
tar -zxvf mongodb-src-r4.4.0.10.tar.gz

export PATH=mongodb-src-r4.4.0.10/bin:$PATH
~~~

Por último, para iniciarlo se usan los comandos:
~~~
service mongod start

service mongod stop

service mongod restart

mongo


> Empleando la utilidad mongoimport introduce los documentos correspondientes a las colecciones productos y zips (códigos postales).

Para usar el comando mongoimport tenemos que indicar una serie de parámetros:
* --db: indica la base de datos donde queremos guardar los datos.
* --collection: indica la colección o tabla donde se guardarán los datos.
* --drop: para borrar todos los datos que hubiese en la colección.
* --file: indica dónde está el fichero en formato json.

~~~
mongoimport --db practica --collection producto --file /home/paloma/CICLO/BASE\ DE\ DATOS/Mongo/Products.json 

mongoimport --db --file practica --collection /home/paloma/CICLO/BASE\ codpostal DE\DATOS/Mongo/zips.json
~~~

Pero antes hay que descargar el paquete de herramientas de MongoDB desde la página oficial.


> Ejercicios de ejemplo
3. Introduce un registro con los datos de tu teléfono móvil.

db.productos.insert({"id" : "XMi8", 
                     "name" : "Mi 8", 
                     "brand" : "Xiaomi", 
                     "type" : "phone", 
                     "price" : 300, 
                     "warranty_years" : 1, 
                     "available" : true  })
         

4. Introduce un registro con los datos de tu tarifa.

db.productos.insert({"id" : "SyT7", 
                     "name" : "Phone Service 7", 
                     "type" : "service", 
                     "monthly_price" : 7, 
                     "limits" : { "voice" : { "units" : "minutes", 
                                             "n" : 100, 
                                             "over_rate" : 0.05 }, 
                                 "data" : { "n" : "1,5GB", 
                                         "over_rate" : 0 }, 
                                 "sms" : { "n" : "unlimited", 
                                         "over_rate" : 0 } }, 
                     "sales_tax" : true, 
                     "term_years" : 2 })


5. Muestra los documentos correspondientes a productos cuyo precio es 200.

db.productos.find({"price" : 200}).pretty()


6. Muestra el número de productos de precio menor o igual que 10.

db.productos.count({"price" : {$lte : 10}})


7. Muestra el nombre y precio de los productos cuyo tipo es teléfono.

db.productos.find({"type" : "phone"}, {"name" : 1, 
                                       "price" : 1, 
                                       "_id" : 0}).pretty()


8. Muestra los datos de los cargadores que sirven para el teléfono AC3.

db.productos.find({"type" : "charger", 
                   "for" : "ac3"}).pretty()


9. Muestra las tarifas que permiten un tráfico de datos ilimitado.

db.productos.find({"type" : "service", 
                   "limits.data.n" : "unlimited"}).pretty()


10. Muestra el nombre de los dos teléfonos más caros.

db.productos.find({"type":"phone"}, {"name":1, 
                                     "_id":0}).limit(2).sort({"price":-1}).pretty()


11. Muestra el nombre de la ciudad más poblada de Alabama.

db.codpostal.find({"state":"AL"}, {"city":1, 
                                   "_id":0}).limit(1).sort({"pop":-1}).pretty()


12. Muestra el nombre de las ciudades de Michigan situadas al norte del paralelo 46.

db.codpostal.find({"state": "MI", 
                   "loc.1":{$gt:46}}, {"city": 1,
                                       "_id":0})


13. Muestra el número de ciudades agrupadas por estado.

db.codpostal.aggregate([{$group:{_id:"$state",
                                 ciudades:{$sum:1}}}])


14. Muestra la ciudad más poblada de cada estado.

db.codpostal.aggregate([{$group:{_id:"$state",
                                 ciudad:{"$city":1}}}])


15. Muestra la población total de cada estado.

db.codpostal.aggregate([{$group:{_id:"$state",
                                 poblacion:{$sum:"$pop"}}}])


