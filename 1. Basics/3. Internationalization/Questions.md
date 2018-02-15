### 1. Which method is used for translating strings, and on which types of objects is it generally available?


- Module Translation:	CSV in /app/locale
- Theme Translation:	CSV in /app/design/{area}/{package}/{theme{}/locale (theme folder translate)
- Database Translation:	Database (table core_translate)


### 2. In what way does the developer mode influence how Magento handles translations?

[Source](http://excellencemagentoblog.com/question/in-what-way-does-the-developer-mode-influence-how-magento-handles-translations/)

Quand le mode développeur est actif, les traductons globales sont désactivées (pour les cas de traductions dupliquées). Ce qui veux dire que seules les traductions internes au module seront effectives.

Par exemple, si nous avons une traduction `$this->__('XYZ')` dans le module *Mage_Checkout* et que la traduction est présente dans *Mage_Catalog* et *Mage_Contact*, elle ne s'appliquera pas.

Pour simplifier toutes les traductions ambigues sont supprimés.

### 3. How many options exist to add a custom translation for any given string?

- Base de donnée 
- Module 
- Theme 

### 4. What is the priority of translation options?

BDD > Theme > Module 


### 5. How are translation conflicts (when two modules translate the same string) processed by Magento?

**Exemple:**

```php
Mage::helper("module_a")->__("Hello");
Mage::helper("module_b")->__("Hello");
```

Dans la méthode `_addData()` cela va créer 2 clés pour chaque module pour garder les chaines de caractères séparées.
