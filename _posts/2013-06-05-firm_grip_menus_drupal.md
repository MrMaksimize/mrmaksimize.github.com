---
layout: post
title: "Firm Grip on Menus in Drupal"
description: ""
category: drupal
tags: 
image: http://www.avgjoeguide.com/wordpress/wp-content/uploads/2013/02/lockdown.jpg
---
{% include JB/setup %}

For many that have worked with Drupal in teams before, I'm sure you have run into problems with managing menus. 
There are many ways to create a menu item in Drupal, one of them being from the menu editor, and others when creating content or views, or panels pages.
 
The menu items created when you're creating content, views or panels override any configurations you may have set in the menu editor, hence the source of the problem. 
 
In addition, the way that menu items are stored in features, they get explicitly linked to node id.  So if you have a page on dev that's node/5 and the same page that lives on staging that's at node/6 and prod is node/7, it's extremely different to move menu configurations across environments. 
 
This was causing a ton of wasted time and even more frustration. 

<!--break-->
 
Anyway, long story short, I needed a way to manage the menus on the site with an extremely firm grip. 
 
I found this great module called [Menu Import](https://drupal.org/project/menu_import);  The original purpose was to be able to create IA on a drupal site by simply creating a JSON file and running a drush command.  The module will create the needed pages for you and the menu structure.
 
Let's look at an example menu export file: (Grabbed it from the [module page](https://drupal.org/project/menu_import))
 
    Pages {"url":"pages/all"}
    - Site {"url":"http://external-site.com/","description":"Visit our site."}
    - Story {"url":"node/3","description":"A very interesting story!"}
    - Story {"url":"content/about","description":"About us"}
    -- Some node {"url":"node/4","lang":"da","description":"Links is in Danish language"}
    Archive
    - Admin zone {"url":"admin/appearance","description":"Internal link with description."}
    A page {"description":"Description only, no link.","hidden":true,"expanded":true}
 
As you can see - you can definitely use router paths like `node/3` but you can also use alias paths for nodes as well like 'content/about'.  This means now we don’t have to worry about the node id, only the path.
 
Menu Import also has a nice little piece of functionality where you can export your current menu structure to a json file.  So that's what I did -
 
    drush menu-export main-menu-export.txt main-menu
 
I then saved the file inside of the feature where I wanted my menu structure to be. 
 
And subsequently, I added the following to the feature's .module file:
 
    function my_menu_feature_pre_features_revert($component) {
      // I actually had two menus, that's why the foreach loop.
      $menu_export_files = array(
        'main-menu' => 'main-menu-export.txt',
      );
      $results = array();
      if ($component == 'menu_custom') {
        if (!function_exists('menu_import_file')) {
          module_enable(array('menu_import'));
        }
        $path = drupal_get_path('module', 'my_menu_feature');
        foreach ($menu_export_files as $menu_name => $menu_export_file) {
          $result = menu_import_file($path . '/' . $menu_export_file, $menu_name, array(
            'link_to_content' => TRUE,
            'create_content' => FALSE,
            'remove_menu_items' => TRUE,
          ));
          $results[$menu_name] = $result;
        }
        drush_print_r($results);
      }
    }
 
And the following to the .info file:
 
    features[menu_custom][] = main-menu
 
That's it! Now, when I run `drush fr -y --force commons_main_menu`, it will force revert the feature and run the menu-import to update my menu items, deleting any previous problems before doing so. 
 
The reason I need to do a `--force` is because features may not detect any menu change configurations, so I'm forcing it to revert.
 
That is all friends. Enjoy your menu stability!
