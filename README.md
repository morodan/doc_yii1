#instalare Yii
- se downloadeaza versiunea 1.1.17
- se muta `framework` folder in afara folderului accesibil de pe web, la mine ii in `/vagrant/framework` si situl ii in `/vagrant/site`
- se ruleaza comanda pentru a instala Yii ca aplicatie din folderul `framework`:

`./yiic webapp /vagrant/site`

- se creaza folderele:

```
assets - pt jquery - asta trebuie sa fie writabil de catre apache
css
images
protected - unde ii situl cu continut
themes - teme pt site
index-test.php
index.php
```

#configurare Yii
mare parte din configurari se fac in `protected/config/main.php`
`index.php` este un bootstrap file - ceea ce inseamna ca toate interactiunile userului vor trece pe aici
fisierul `index-test.php` este la fel ca `index.php` doar ca fisierul de configurare este `protected/config/test.php`


#validation
se fac in `models` folder in `funcion rules()` ca de exemplu:

```php
// NOTE: you should only define rules for those attributes that
// will receive user inputs.
return array(
    array('email_uid, email, name, others', 'required'),
    array('email_uid', 'length', 'max'=>13),
    array('email, name', 'length', 'max'=>64),
    array('email', 'length', 'min' => 6),
    array('email', 'email'),
    // The following rule is used by search().
    // @todo Please remove those attributes that should not be searched.
    array('email_uid, email, name', 'safe', 'on'=>'search'),
);
```

tot aici in fisierul respectiv poti seta atributele pt campuri, care sa fie searheable, etc.

#variabile la care sa fi atent
`Yii:app()` se refera la aplicatia ca intreg
`Yii:app()->request` se refera la pagina curenta cum a fost accesata
`Yii:app()->request->baseUrl` se refera root url 

de exemplu cum se foloseste:

```html
<link rel="stylesheet" type="text/css" href="<?php echo Yii::app()->request->baseUrl; ?>/css/main.css" />
```

#Migrations
ca sa poti face migrate, trebuie sa fii in folderul `protected` aici o sa mai vezi un folder `migrations` unde se salveaza php-urile cu migrari

##ca sa incepi migrarea se da comanda: 
`./yiic migrate`

##ca sa faci o migrare noua: 
`./yiic migrate create create_new_table` unde create_new_table ii numele noii migrari

##ca sa aplici toate migrarile ce sunt in folderul `migrations` 
se da comanda: `./yiic migrate`

##daca vrei sa faci migrate doar la una anume atunci: 
`./yiic migrate up 3` de exemplu, 

sau daca stii numele migrarii cu care vrei sa te joci: 
`./yiic migrate to 101010_123233` de ex.

##sa dai migrarile inapoi, reverting migrations 
se foloseste comanda: 

`./yiic migrate down [step]` 

unde `step` ii optional si reprezinta cate migrari vrei sa faci inapoi, default ii 1

##daca vrei sa faci redoing migrations
inseamna ca prima data faci reverting unde vrei sa ajungi si apoi aplici migrarea dorita, cu comanda: 

`./yiic migrate redo [step]` 

unde step ii cati pasi vrei sa dai inapoi, default ii 1

##afiseaza informatii despre migrare
folosesti comenzile:

`./yyic migrate histoy [limit]`

`./yiic migrate new [limit]`

unde `limit` specifica numarul de migrari de afisate. default sunt toate.
prima comanda afiseaza migrarile care s-au aplicat, iar cea de a doua doar migrarile care inca nu s-au aplicat

##mai multe despre migrari
poti gasi [aici - documentatia oficiala pt Yii 1.1](http://www.yiiframework.com/doc/guide/1.1/en/database.migration)

##Generic Column Types
Here are the generic column types you can use to keep migrations DB independent, and what they mean (as of Yii 1.1.7)

-    pk: an auto-incremental primary key type, will be converted into "int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY"
-    string: string type, will be converted into "varchar(255)"
-    text: a long string type, will be converted into "text"
-    integer: integer type, will be converted into "int(11)"
-    boolean: boolean type, will be converted into "tinyint(1)"
-    float: float number type, will be converted into "float"
-    decimal: decimal number type, will be converted into "decimal"
-    datetime: datetime type, will be converted into "datetime"
-    timestamp: timestamp type, will be converted into "timestamp"
-    time: time type, will be converted into "time"
-    date: date type, will be converted into "date"
-    binary: binary data type, will be converted into "blob"

The implementation uses `CDbSchema->getColumnType()`, and you can find more information here: [official documentation](http://www.yiiframework.com/doc/api/1.1/CDbSchema#getColumnType-detail)


#Alte variabile de folosit si de stiut

curent user ID se poate afla cu:

`Yii::app()->user->id` sau `Yii::app()->getUser()->getId()`

#Working with request
You can access the request component in your web application by using Yii::app()-
>getRequest() . So, let's review the most useful methods and their usage, methods that
return different parts of the current URL. In the following table, returned parts are marked with a bold font.

|getUrl|http://cookbook.local/**test/index?var=val**|
|getHostInfo|**http://cookbook.local**/test/index?var=val|
|getPathInfo|http://cookbook.local/**test/index**?var=val|
|getRequestUri|http://cookbook.local/**test/index?var=val**|
|getQueryString|http://cookbook.local/test/index?**var=val**|

