---
layout: page.njk
---

Taiko features can be extended via plugins which can allow users to take more advantage of ChromeDevtoolsProtocol 
when core Taiko concentrating on functionalities around UI automation tests. It can also help users simplify their workflow.
Below are a few scenarios where plugins can help extending taiko:

- combine set of taiko actions and provide apis for it (eg: taiko-paypal - which can let users do paypal payment)
- expose more of ChromeDevtoolsProtocols apis (eg: taiko-diagnostics - which can let users perform diagnostics actions 
over the website, taiko-screencast - which can let users record a video of the script running)
- provide selectors based on frameworks (eg: taiko-react - which can give selectors specific to react frameworks)
- override taiko apis (eg: taiko-android - which can override openBrowser api to launch browser in an android device)

### Conventions

- Plugin names should have the prefix ‘taiko-’. eg: taiko-android, taiko-screencast
- Plugins should not have taiko as dependency they should use the same instance as users script.
- Plugins should implement ‘init()’ method which will be called by taiko on load passing taiko and event handler instances.
- Taiko will communicate with plugins via events.
- Plugins should implement ‘exec()’ method to extend taiko CLI to execute any action. 
- Plugins should add capability in their package.json if they have execute functionality.
 ```
capability:[‘subcommands’]` - in plugins package.json 
```
Only if the package.json has the above capability added taiko will consider it to be a subcommand. 

### Plugin Workflow

![Taiko plugin workflow](https://user-images.githubusercontent.com/6358540/59250814-ebe81b80-8c45-11e9-80ab-6ab17df24aa5.png)

#### Example:
```
const { openBrowser, goto, write, press, closeBrowser , inputField, into, screencast} = require('taiko');

(async () => {
    try {
         await openBrowser();
	   await screencast.start();	
         await goto('google.com');
         await write('taiko', into(inputField()));
         await press('Enter');
   } catch (e) {
        console.log(e);
  } finally {
        await closeBrowser();
        await screencast.stop(); 
  }
})();
```

- On requiring taiko, taiko will look for any runtime plugins given to be loaded via `TAIKO_PLUGIN` env variable if 
not it will load plugins from users package.json dependencies by calling `init` method in plugins passing taiko and 
eventhandler instances to plugin to get started.
- Plugin instances are given to user script via taiko as objects with name removing the prefix `taiko-` eg. `screencast` in the above script
- All apis that are overridden will be accessible from taiko other additional apis will be part of plugin object

### Running scripts with plugins

- For running scripts using taiko launch runner or REPL using `--plugins` option 
eg: `taiko code.js --plugin 'android'`
- Use env variable `TAIKO_PLUGINS` to load plugins on runtime when using other runners.


### Execution capability in plugin
Example: `taiko diagnostics http://gauge.org`
- Loads plugins installed globally (local modules when using local taiko instance) as a subcommand when initializing commander.js
- `taiko -help` will list all possible subcommands
- Looks for capability execution in plugins package.json to avoid loading plugins that does not have execution capability
- `exec()` function in plugin will be executed for the subcommand

### Examples
- Taiko-android       - https://github.com/saikrishna321/taiko-android/
- Taiko-screencast  - https://github.com/getgauge-contrib/taiko-screencast
- Taiko-diagnostics - https://github.com/saikrishna321/taiko-diagnostics/