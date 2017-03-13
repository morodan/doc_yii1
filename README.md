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



