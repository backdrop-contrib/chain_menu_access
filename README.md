Chain Menu Access
=================

Chain Menu Access API allows your module to chain its own menu access callback functions into other modules' menu router entries.

This module is an API module. It has no functionality on its own. Install it only if some other module requires it.

Rationale
---------

In Backdrop menu access is determined very early during the page loading process. If your module wants to alter the access to some other other module's paths, then the cleanest way to do this is to implement hook_menu_alter() and to change the values of the "access callback" and "access argument" keys of the corresponding menu router items (see hook_menu()).

The trivial approach is to simply replace the values of those two keys with your own and take over the access checking completely. However, this is rarely appropriate. The usual case is that your module wants to either restrict or extend access, and the resulting behavior should be a joint effort between your module and the module that owns the menu item.

Even more generally, there may be additional third-party modules that are interested in restricting or extending access, so simply replacing the values is a no-no! Rather, you want to install your callback in such a way that Backdrop calls both your callback and the original one(s) and that it figures out the final result from the two votes.

The original callback doesn't know about any of this, so you have to take the lead and manage it. This is called chaining: In the general case, several modules have already installed their callbacks, then your module inserts its own callback in the front of the existing chain, and some other modules may come after you and add even more callbacks in front of you.

This is done by implementing hook_menu_alter() and changing the "access callback" and "access argument" values of the desired menu items. For each item you need to save the old values, install your own callback, that when the callback is called, you need to evaluate the old access and the new one defined by your module, and then merge the two.

This can certainly be done 'manually,' but it's not quite easy to get right in all cases, and if you need to do it for multiple menu items, then it can become tedious. This is the one thing that Chain Menu Access API does, and it does it very well.

How to Use
----------

Implement hook_menu_alter(), and in the simplest case just call

```
chain_menu_access_chain($menu, 'the/existing/path', '_mymodule_new_access_callback');
```

where `_mymodule_new_access_callback` is an access callback function just like you would implement for your own menu items. If the function requires access arguments, then pass them in an array as the fourth argument:

```
chain_menu_access_chain($menu, 'the/existing/path', '_mymodule_new_access_callback', array('arg1', 'arg2'));
```

Installation
------------

Install this module using the official Backdrop CMS instructions at https://backdropcms.org/guide/modules.

LICENSE
-------

This project is GPL v2 software. See the LICENSE.txt file in this directory for complete text.

CREDITS
-----------

This module is based on the Chain Menu Access module for Drupal.

Current Maintainers on Drupal:

- salvis <https://www.drupal.org/u/salvis>
- chx <https://www.drupal.org/u/chx>

MAINTAINERS
-----------

- Seeking maintainers

Ported to Backdrop by:

- herbdool <https://github.com/herbdool>