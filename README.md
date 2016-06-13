# PhoenixJsroutes

![build](https://travis-ci.org/tiagoengel/phoenix-jsroutes.svg?branch=master)

Brings phoenix router helpers to your javascript code.

This project consist of a mix compiler task that generates a javascript
module containing helpers to access your server routes.

## Installation

Add the latest version to your ```mix.exs``` file:
```elixir
def deps do
  [{:phoenix_jsroutes, "~> 0.0.1"}]
end
```

## Getting started

Add the ```jsroutes``` compiler to the list of compilers in your project.

```elixir
def project do
    [app: :jsroutes_test,
    ...
    compilers: [:phoenix, :gettext] ++ Mix.compilers ++ [:jsroutes]
    ...]
end
```
**This compiler should be placed after the elixir compiler, this is important because your router module should be already compiled when this compiler runs**

After that, if you run ```mix compile``` the file ```web/static/js/phoenix-jsroutes.js```
will be generated.

You can clean up the generated file by running the ```mix clean```.

### Using the javascript helpers
The code generated by this compiler is compatible with AMD, Commonjs and Global builds.

The functions are generated with different names from the ones in the server in order to follow javascript best practices. For example:

	user_path(:index)                => userIndex()
	user_path(:create)               => userCreate()
	user_path(:update, 1)            => userUpdate(1)
	user_friends_path(:update, 1, 2) => userFriendsUpdate(1, 2)


If you are using the default phoenix configuration with ```brunchjs``` (or any other commonjs compatible build tool like webpack and browserify) you can use the helpers like this

```javascript
import routes from './phoenix-jsroutes'
routes.userIndex(); // /users
routes.userCreate(); // /users
routes.userUpdate(1); // /users/1
routes.userFriendsUpdate(1, 2); // /users/1/friends/2
```

For AMD builds
```javascript
require(['phoenix-jsroutes'], routes => {
  routes.userIndex();
})
```

For Global builds
```javascript
PhoenixJsRoutes.userIndex();
// If the name conflicts with anything...
var myBeautifulName = PhoenixJsRoutes.noConflict();
```

## Live reload

If you want the javascript module to be generated automatically when your router changes, just add this compiler to the list of ```reloadable compilers```.

```elixir
# config/dev.exs

config :my_app, MyApp.Endpoint,
  http: [port: 4000],
  ...
  reloadable_compilers: [:gettext, :phoenix, :elixir, :jsroutes]
```

## Configuration

Key | Type | Default | Description  |
| --- | --- | --- | --- |
output_folder | String | web/static/js | Sets the folder used to generate files
include | Regex | nil | Will include only paths that matches this regex
exclude | Regex | nil | Will include only paths that doesn't matches this regex

Configurations should be added to the key ```:jsroutes``` in your application.
```elixir
config :my_app, :jsroutes,
  output_folder: "priv/static/js",
  include: ~r[/api],
  exclude: ~r[/admin]
```

## TODO

- [ ] Integration tests with a real phoenix application
- [ ] Add a task to print the the functions and its paths, similar to the phoenix.routes task
- [ ] Add support to umbrella applications
- [ ] Get the template from a configuration

## Contributing

Contributions are very welcome. If you want to contribute to this project first
open an issue so we can discuss your ideas, if you're confident in implementing it, fork the project and create a pull request. Thank's in advance :).

## Contributors

Special thanks to this guys who helped me starting this project.

<br/>[Marcio Junior](https://github.com/marcioj)
<br/>[Horacio Fernandes](https://github.com/horaciosystem)
<br/>[Diogo Kersting](https://github.com/diogovk)
<br/>[Paulo](https://github.com/paaulo)
