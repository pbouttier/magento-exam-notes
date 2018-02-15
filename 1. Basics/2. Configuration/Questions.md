### 1. How does the framework discover active modules and their configuration?

```php
Mage_Core_Model_Config->_loadDeclaredModules()
```

Cette méthode va chercher tous les fichiers XML dans *app/etc/modules* et va charger les modules correspondant.
En premier il appelle *_getDeclaredModuleFiles* pour récupérer tous les fichiers.

Ensuite il boucle sur les fichiers et fait:

* Il charge la configuration du module
* Il ajoute le nom du module dans un tableau
* Il trie les modules listés dans ce tableau avec *_sortModuleDepends*
* Il créer la configuration *Mage_Core_Model_Config_Base* et ajoute les nodes XML *<config><modules/></config>* à la classe
* Il ajoute le module à la configuration
* Il charge les modules depuis cette configuration

### 2. What are the common methods with which the framework accesses its configuration values and areas?

```php
Mage::->getStoreConfig($path);
Mage::app()->getStore()->getConfig($path);
Mage::getStoreConfigFlag($path); //Checks if a value exists
Mage::getConfig()->getNode($path);
```

### 3. How are per-store configuration values established in the XML DOM?

Pour chaque boutique (store) les valeurs de configuration sont définies dans le fichier *config.xml* sous les nodes *<stores>* et *<store_code>*.

**Exemple:** Si l'on veux surcharger le thème

```xml
<stores>
    <default>
        <design>
            <package>
                <name>default</name>
            </package>
            <theme>
                <default>modern</default>
            </theme>
        </design>
    </default>
</stores>
```

On peut également le faire avec les nodes *<websites>* et *<website_code>*

```xml
<websites>
    <base>
        <design>
            <package>
                <name>default</name>
            </package>
            <theme>
                <default>modern</default>
            </theme>
        </design>
    </base>
</websites>
```



**Note:** Informations récupérer depuis [http://magestudyguide.com/#how-are-per-store-configuration-values-established-in-the-xml-dom?](http://magestudyguide.com/#how-are-per-store-configuration-values-established-in-the-xml-dom?)


### 4. By what process do the factory methods and autoloader enable class instantiation?

- Varien_Autoload
- spl_autoload_register

Pour plus d'informations voir [Explain how Magento loads and manipulates configuration information](https://github.com/pbouttier/magento-exam-notes/blob/master/1.%20Basics/2.%20Configuration/1.Explain%20how%20Magento%20loads%20and%20manipulates%20configuration%20information.md)

### 5. Which class types have configured prefixes, and how does this relate to class overrides?

Les 3 préfixes de classes sont: Model, Helper, Block.

Dans *config.xml* quand nous les surchargeons nous devons ajouter ces préfixes

In our config.xml when we rewrite we need put it under these 3 prefixes

```xml
<blocks>
   <catalog>
      <rewrite>
         <list>New_Block_Class</list>
      </rewrite>
   <catalog>
</blocks>
```


### 6. Which class types and files have explicit paths?

Not sure what this means?

### 7. What configuration parameters are available for event observers?

**type**

- model
- object
- none
- singleton

**class**

Le nom de la classe de l'observer est par exemple NameSpace_NameModule_Model_Observer


**method**

Le nom de la méthode appellée

### 8. What are the interface and configuration options for automatically fired events?

Il faut dispatch des évènments.

### 9. What is the structure of event observers, and how are properties accessed therein?

Un évènement est dispatché de la façon suivante: 

```php
Mage::dispatchEvent('catalog_product_get_final_price', array('product' => $product, 'qty' => $qty));
```

Et on accède à ces paramètres comme suit:

```php
public function myObserverMethod($observer)
{
  $product = $observer->getEvent()->getProduct();
  $qty = $observer->getEvent()->getQty();
  /* Do something */
}
```

### 10. What configuration parameters are available for cron jobs?

Sous la node global -> events -> crontab -> name_of_cron nous avons:

- schedule -> cron_expr
- run -> model

Exemple:

```xml
<global>
  <crontab>
      <jobs>
          <namespace_namemodule_cron>
              <schedule>
                 <cron_expr>*/1 * * * *</cron_expr>
              </schedule>
              <run>
                  <model>namespace_namemodule/observer_cron::runCustomCronJob</model>
              </run>
          </namespace_namemodule_cron>
      </jobs>
  </crontab>
</global>
```
