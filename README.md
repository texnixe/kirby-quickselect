# Kirby Quickselect v1.4 <a href="https://www.paypal.me/medienbaecker"><img width="99" src="http://www.medienbaecker.com/beer.png" alt="Buy me a beer" align="right"></a>

This field is based on the core select field and adds a filter and placeholder functionality. It uses the [select2](https://github.com/select2/select2) jQuery plugin.

When displaying the images of a page with `options: images` it will display thumbs automatically.

## Installation

Simply put the `quickselect` folder into your `site/plugins` folder.

## Example

![Preview](https://user-images.githubusercontent.com/7975568/27504121-082d8ca4-5885-11e7-8983-9159593cdbb4.gif)

````
featured:
  label:       Image
  type:        quickselect
  options:     images
  placeholder: Choose an image...
````

Just like you do with the regular `select` field, you can specify what elements should be listed. `pages`, `children`, `files`, `images`, [...](https://getkirby.com/docs/cheatsheet/panel-fields/select). Even a more complex `query` or options from another field.

Anything that's printed in the list can be searched for. So if you set the `text` of a `query` to `{{title}} ({{uri}})`, you can find pages by their ancestors, too.

Compared to the core `select` field, you can also define a `placeholder` that can make things clearer for the editor.

### Controller option:

This field is extended with an option to use a user specified function to have even more control of the options that will be loaded. The idea is taken from the Controlled List plugin.

#### Example

Create a simple plugin that lets you choose from the panel users.

site/plugins/myplugin/myplugin.php:

```
class MyPlugin {
  static function userlist($field) {
    $kirby = kirby();
    $site = $kirby->site();
    $users = $site->users();

    $result = array();

    foreach ($users as $user) {
      if (!empty($user->firstName()) && !empty($user->lastName())) {
        $result[$user->username()] = $user->firstName() . ' ' . $user->lastName();
      } else {
        $result[$user->username()] = $user->username();
      }
    }

    return $result;
  }
}
```

In your blueprint:

```
users:
  label: Users
  type: relationship
  controller: MyPlugin::userlist
```
