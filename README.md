# mediawiki-pages-DynamicMenu
A pack of pages for easy sidebar menu insertion.

# Installation

## Create a custom namespace
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
        "mediawiki/semantic-result-formats": "^3.2",
        "mediawiki/sub-page-list": "^1.6"
    }
}
```
and run:
```bash
composer update --no-dev -o
```
Then enable them in LocalSettings.php:
```php
enableSemantics( 'parasol' ); #SemanticMediawiki
wfLoadExtension( 'SemanticResultFormats' );
```
Running update.php is required:
```bash
php maintenance/update.php --quick
```

### Git
Clone repos into extensions directory and enable the extensions in LocalSettings.php:
```php
wfLoadExtension( 'PageNotice' );
wfLoadExtension( 'TreeAndMenu' );
wfLoadExtension( 'DynamicSidebar' );
wfLoadExtension( 'UserFunctions' );
wfLoadExtension( 'ParserFunctions' );
wfLoadExtension( 'Gadgets' );
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
wfLoadExtension( 'SubpageFun' );
```
