# mediawiki-pages-DynamicMenu
A pack of pages for easy sidebar menu insertion.

# Requirements
* **SemanticMediaWiki** Provides Modification date
* **DynamicSidebar** Provides CATEGORY-SIDEBAR
* **Gadgets** Loads JS
* **PageNotice** Provides Mediawiki:Bottom-notice-ns-0
* **ParserFunctions** (with `$wgPFEnableStringFunctions = true;`)
* **SubpageFun** Provides useful magic words (incl. TOPLEVELPAGE)
* **SubPageList** Provides a parser function for menu content auto creation
* **TreeAndMenu** Provides expandable multilevel menus
* **UserFunctions** Provides `{{#ifsysop: ... }}`
* **PageExchange** or **PagePort** for importing content

# Setup

## Create a custom namespace
We'll store TreeAndMenu menus in a custom namespace. A menu for the page `SomePage` will be stored as `Menu:SomePage`.

Add this to LocalSettings.php:
```php
define("NS_MENU", 3000); // This MUST be even.
define("NS_MENU_TALK", 3001); // This MUST be the following odd integer.

$wgExtraNamespaces[NS_MENU] = "Menu";
$wgExtraNamespaces[NS_MENU_TALK] = "Menu_talk";
$wgNamespacesWithSubpages[NS_MENU] = true;
$smwgNamespacesWithSemanticLinks[NS_MENU] = true;
```

## Install and configure required extensions

### Composer
Add this to composer.local.json:
```php
{
    "require": {
        "mediawiki/semantic-media-wiki": "^3.2",
        "mediawiki/sub-page-list": "^1.6"
    }
}
```
and run:
```bash
composer update --no-dev -o
```
Then enable SMW in LocalSettings.php:
```php
enableSemantics( 'parasol' ); #SemanticMediawiki
```
Running `update.php` is required:
```bash
php maintenance/update.php --quick
```

### Git
Clone repos into extensions directory and enable the extensions in LocalSettings.php:
```php
wfLoadExtension( 'DynamicSidebar' );
wfLoadExtension( 'Gadgets' );
wfLoadExtension( 'PageNotice' );
wfLoadExtension( 'ParserFunctions' );
wfLoadExtension( 'SubpageFun' );
wfLoadExtension( 'SubPageList' );
wfLoadExtension( 'TreeAndMenu' );
wfLoadExtension( 'UserFunctions' );
```
### Variables
Set in LocalSettings.php:
```php
$wgAllowSiteCSSOnRestrictedPages = true;
$wgRestrictDisplayTitle = false;
$wgNamespacesWithSubpages[NS_MAIN] = true;
$wgDynamicSidebarUsePageCategories = true;
$wgUFAllowedNamespaces = array_fill( 0, 4000, true );
$wgPFEnableStringFunctions = true;
$wgEnableSidebarCache = false;
$GLOBALS['egSPLAutorefresh'] = true;
```
## Optional
Optionally install and enable:
```php
wfLoadExtension( 'VisualEditor' );
```
## Import the package

### via PagePort 

* download the repository
* run `php extensions/PagePort/maintenance/importPages.php --source ~/mediawiki-pages-DynamicMenu`

### via PageExchange

* add the following to the bottom of your `LocalSettings.php`: `$wgPageExchangePackageFiles[] = 'https://raw.githubusercontent.com/wikivisor/mediawiki-pages-DynamicMenu/main/page-exchange.json';`
* navigate to `Special:Packages` and install the package
* (optional) from the wiki root, run `php maintenance/runJobs.php`
