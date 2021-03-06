---
title: "Archivos YAML para guardar parámetros en Rails"
cover: '/img/picyaml.png'
isPost: true
active: true
excerptOther: ''
postDate: '2014-05-13 00:12:02 -0400'
tags:
 - rails
 - yaml
 - web
---

Toda aplicación tiene parámetros de configuración que conviene que estén parametrizados para evitar repetición en el código y para tener la facilidad de cambiar los mismos en un solo lugar. Lo que en la herga de Rails diríamos: para estar [DRY (Don't Repeat Yourself)](http://en.wikipedia.org/wiki/Don't_repeat_yourself)

Si bien en una aplicación en Rails se podrían crear variables globales la mejor opción a futuro es la de crear un modelo que guarde toda esta información en la base de datos. Sin embargo esto implicaría crear todas las pantallas y controladores de administración que muchas veces no justifica porque estos valores no cambian con frecuencia.

Como una solución intermedia al problema se podría usar archivos **YAML**, similares a los que se usa en Rails para la configuración de la base de datos `database.yml`

Primeramente crearemos el archivo `yml` que contendrá 3 keys con nuestros parámetros de configuración

##### /config/config.yml

``` ruby
store_name: ‘myStore'
store_phone: ‘1-212-328-3212‘
store_email: 'mystore@gmail.com'
```

Luego es necesario cargar este archivo al iniciar la aplicación por ejemplo en el `/config/application.rb`

##### /config/application.rb

``` ruby
CONFIG = YAML.load(File.read(File.expand_path('../config.yml', __FILE__)))
CONFIG.symbolize_keys!
```

La primer línea lee el archivo `config.yml` y lo convierte en un **hash** que será guardado en la variable `CONFIG`. La segunda línea habilitará este **hash** para que pueda ser llamado utilizando símbolos como veremos a continuación.

Desde una vista podremos llamar a nuestros parámetros de la siguiente manera: 

``` erb
<%= CONFIG[:store_name] %>
<%= CONFIG[:store_phone] %>
<%= CONFIG[:store_email] %>
```

Esta es una manera muy sencilla y limpia de usar archivos **YAML** para guardar parámetros de configuración, la cual puede ser extendida para muchos otros usos. Para más información de la sintaxis de los archivos YAML visitar el sitio offical [http://www.yaml.org](http://www.yaml.org) o la página de Wikipedia [YAML](http://en.wikipedia.org/wiki/YAML).