# Girder 3 is officially released

## Zach_Mullen on 2019-08-10T17:44:38.348Z

We are pleased to announce the release of Girder 3\.0! This major version release represents a significant amount of effort spent to make working with Girder simpler and more streamlined. The major breaking change in 3\.0 is that plugins are now required to be implemented as standalone Python packages. This decoupling makes plugin development and deployment much more straightforward and modular, with much less dependence on learning the intricacies of the older Girder plugin build and integration processes. There have also been many code cleanup and testing updates that will make Girder more maintainable in the future.


See the [Migration Guide](https://girder.readthedocs.io/en/stable/migration-guide.html) for more details on what’s new and what’s changed. You may also make use of the new [Girder Plugin cookiecutter template](https://github.com/girder/cookiecutter-girder-plugin) to update 2\.x plugins and build new 3\.0 plugins.


In parallel to the 3\.0 effort which has focused primarily on backend updates, the Girder community is also working on a set of modern UI components for accessing the Girder REST API in a separate [Girder Web Components](https://github.com/girder/girder_web_components) library. This library is utilizing the modern web UI frameworks Vue.js and Vuetify to refresh the current Backbone.js client. If you are building a new Girder\-based frontend application, please give it a look.


Thanks to the Girder community for making this release possible!


---

## Zach_Mullen on 2020-03-03T19:26:28.337Z


---

