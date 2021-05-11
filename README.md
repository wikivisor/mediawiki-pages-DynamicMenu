# mediawiki-pages-DynamicMenu
An installable collection of pages for easy menu insertion into Mediawiki sidebar.

- [Usage](#usage)
  * [Global menu](#global-menu)
  * [Category menu](#category-menu)
  * [Project menu](#project-menu)
    + [Manual menu](#manual-menu)
    + [Automatic menu](#automatic-menu)
    + [Display title](#display-title)
  * [Category tree](#category-tree)
  * [Other features](#other-features)
- [Requirements](#requirements)
- [Setup](#setup)
  * [Create a custom namespace](#create-a-custom-namespace)
  * [Install and configure required extensions](#install-and-configure-required-extensions)
    + [Composer](#composer)
    + [Git](#git)
    + [Variables](#variables)
  * [Optional](#optional)
  * [Import the package](#import-the-package)
    + [via PagePort](#via-pageport)
    + [via PageExchange](#via-pageexchange)
  * [Apply styles](#apply-styles)

# Usage

## Global menu

A multilevel collapsible menu (the first level is always expanded). Served via [Mediawiki:Sitenotice](/Mediawiki/Sitenotice.mediawiki) using [Template:Global menu](/Template/Global%20menu.mediawiki). 

**Advantage:** Appears on all pages.

Navigate to [Mediawiki:Global-menu](/Mediawiki/Global-menu.mediawiki) and plan your menu in format:
```php
* [[RootPageName]]
** [[RootPageName/SubpageName1|SubpageName1]]
** [[RootPageName/SubpageName2|SubpageName2]]
*** [[RootPageName/SubpageName2/SubSub1|SubSub1]]
*** [[RootPageName/SubpageName2/SubSub2|SubSub2]]
** [[RootPageName/SubpageName3|SubpageName3]]
...
```

## Category menu

A **flat** menu block with the title. Menu is shown if the current page belongs to the given category. Create a page `Mediawiki:Sidebar/Category:NameOfYourCategory` and plan your menu in format:
```php
* MenuBlockTitle
** PageName1|LinkText1
** PageName2|LinkText2
** PageName3|LinkText3
...
```
For details refer to the [extension documentation](https://www.mediawiki.org/wiki/Extension:DynamicSidebar)

## Project menu

A multilevel collapsible menu. 

"Project" means hierarchy of subpages derived from the root page. Menu is shown if there is a page in `Menu:` namespace matching the name of the **root page** of the current page / subpage. It means the menu will be shown on the root page and on all subpages.

### Manual menu 

**Advantage:** You can manage the order and level of pages.

Create a page in `Menu:` namespace matching the name of the **root page** of the current page / subpage and plan your menu in format:
```php
* [[RootPageName]]
** [[RootPageName/SubpageName1|SubpageName1]]
** [[RootPageName/SubpageName2|SubpageName2]]
*** [[RootPageName/SubpageName2/SubSub1|SubSub1]]
*** [[RootPageName/SubpageName2/SubSub2|SubSub2]]
** [[RootPageName/SubpageName3|SubpageName3]]
...
```
### Automatic menu

**Advantage:** Menu is auto updating as new subpages are added.

Create a page in `Menu:` namespace matching the name of the **root page** of the current page / subpage and put a call to the [Template:Automenu](/Template/Automenu.mediawiki):
```php
{{automenu|1}}
```
where `1` is the level menu should be started at (for nested menus). For details refer to the [extension documentation](https://www.mediawiki.org/wiki/Extension:TreeAndMenu)

### Display title

To simplify the display title of the subpages, please, use [Template:Subpage title](/Template/Subpage%20title.mediawiki), like this:
```
{{subpage title}}
```

## Category tree

Using a category tree for site navigation is not recommended. We just provide a link to the `Special:CategoryTree` via [Mediawiki:Sidebar](/Mediawiki/Sidebar.mediawiki). In the `Category` namespace the link is changing to `View as a tree` and takes visitors to a category tree built from the current category.

## Other features

### User and Group menus

The User- and the Group-specific menus are also available as a part of DynamicSidebar extension: 
* Group menus must be created under `MediaWiki:Sidebar/Group:<group>`.
* Users are able to create their own custom sidebar blocks under `User:<username>/Sidebar`.

### SemanticMediawiki

:exclamation: **Not included. Needs additional development**

Adding semantic annotations to pages will make it possible to generate hierarchical/nesting trees with logical sorting (eg. using `SemanticResultFormats`. etc), which can be injected into the infrastructure created: just replace manually created menus with the semantic query code.

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

The package is designed to work with the **Vector** sidebar. The work with other skins featuring a sidebar may require ajustments to the package CSS, JS and/or HTML. 

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
Run from Mediawiki install directory:
```php
COMPOSER=composer.local.json composer require --no-update mediawiki/semantic-media-wiki
COMPOSER=composer.local.json composer require --no-update mediawiki/sub-page-list
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

* add the following to the bottom of your `LocalSettings.php`: 
```php
$wgPageExchangePackageFiles[] = 'https://raw.githubusercontent.com/wikivisor/mediawiki-pages-DynamicMenu/master/page-exchange.json';
```
* navigate to `Special:Packages` and install the package
* (optional) from the wiki root, run `php maintenance/runJobs.php`

## Apply styles

Import styles by adding the following into `MediaWiki:Vector.css`:
```php
@import url("/w/index.php?action=raw&ctype=text/css&title=MediaWiki:DynamicMenu.css");
```
**Note**: Adjust the URL according to your paths/rewrite rules.
