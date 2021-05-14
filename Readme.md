TestPluginReactor

_blueprinter-maven-plugin:print_ goal generates a _blueprinter_ directory containg _.html_ and _puml_ files.

__blueprinter-maven-plugin__ module contains:

1) the sources of the plugin itself
2) unit tests
3) integration tests (under _src/it_ folder) fired by _invoker_ plugin, that verifies correctness of generated _html/puml_ files

__blueprinter-test__ module is configured to use the _blueprinter-maven-plugin_ to generate _html/puml_ files

On first issue of 

    mvn clean install

1) .m2 repository does not contain the plugin
2) maven reactor build, install, and execute integration tests of the __blueprinter-maven-plugin__ module
3) maven reactor build and install __blueprinter-test__ module, using the _blueprinter-maven-plugin_ just installed to create the _TestPluginReactor/blueprinter_

On subsequent issues of

    mvn clean install

1) .m2 repository contain the previous build of the plugin
2) maven reactor build, install, and execute integration tests of the __blueprinter-maven-plugin__ module
3) maven reactor build and install __blueprinter-test__ module, using the newly created _blueprinter-maven-plugin_ just installed to create the _TestPluginReactor/blueprinter_

To verify that, look at 

_TestPluginReactor/blueprinter/index.html_ __title__ tag should contain _"index"_.

Then, modify _HTMLWriter.getBodyElement_ __titleElement.textContent__

    val titleElement = toReturn.createElement("title")
    titleElement.textContent = "modified content"

and issue again

    mvn clean install

Now, 

_TestPluginReactor/blueprinter/index.html_ __title__ tag  should contain _"modified content"_.
