###### Update: Gruntjs Port of the ant scripts
Recently ported this over to a Gruntjs task.
- on git: https://github.com/scarrillo/grunt-chrome-compile
- on npm: https://npmjs.org/package/grunt-chrome-compile


### Chrome Extension/App Ant build script

Build script to prepare and package a Google Chrome App or Extension.

The script generates a zip file for the Chrome Webstore and a .crx for hosting on a private domain.

##### Features
 * The packages are prepared with auto updating in mind.
     - Creates an unpackaged build folder that can be loaded in chrome's "Load Unpackaged Extension" for rapid development and testing of only production ready files.
     - Generates a deployable extension.crx and updates.xml for hosting on your own website
     - Generates a deployable extension.zip file for uploading to the Chrome webstore. key.pem is added to the zip, but will be stripped out by the webstore.
     - Reuse for fast updates to the webstore. Just increment the version in build.properties, rebuild, and reupload. 

 * Exclude private project files from being added to the final packages.
     - *Examples:* uncompiled source files, build scripts, and private key files.
     - See the exclude file list in build-extension.xml

 * [NewExtension](https://github.com/scarrillo/ChromeExtensionPackage/tree/master/NewExtension):
    This folder contains an example Chrome extension to demonstrate the workflow.
    Including a demonstration of using the google closure compiler to optimize, obfuscate, and concat javascript.
    You can use the main build script alone or combined with the build scripts under "NewExtension/.build/*" to compile your project and package it for deployment in one shot.

##### Deprecated: publish-extension-ant-build.xml

    This script has been superseded by build-extension.xml and build.properties.

  
##### Quick Start

###### Expected project structure - See [NewExtension](https://github.com/scarrillo/ChromeExtensionPackage/tree/master/NewExtension) for reference.
    /Extension/
        /.build/
            build-extension.xml
            build.properties

        /.cert/
            Extension.pem

        manifest.json
        updates.xml    


 1. Create a /.build/ subfolder and add build-extension.xml and build.properties
 2. Create a /.cert/ subfolder and add your private key

    Ex: /.cert/NewExtension.pem - Where 'NewExtension' is the name of your extension.

 3. Edit build.properties:

    Change 'name', 'version', and the full path to the Google Chrome installation path. Windows and Mac path examples provided.

 4. Review manifest.json:

    Make any custom changes your extension requires.
    Note that 'name' and 'version' will be added from the values set in the previous step. Defined in build.properties.
    Also, take note that the 'background' property is pointing at the script generated by the closure compiler: /js/background.js. This is a sample file to get started with.


#### Further Details

    - build-extension.xml

        The main build script. Add files to the exclude list to keep them out of your .zip and .crx builds.

    - build.properties

        This is where your extensions properties are defined for 'build-extension.xml' ant script.
        Set name, version, id, and the location of Chrome application on your computer.

    - Output Files:

        All generated files will live under the .build folder. OSX users need to unhide this folder.
        http://osxdaily.com/2009/02/25/show-hidden-files-in-os-x/

        + /.build/Extension/
            - Uncompressed folder, used for loading directly into chrome during development

        + /.build/Extension.zip
            - Compressed zip for uploading directly into Google Chrome webstore
            - Warning: This zip file contains key.pem, which is stripped out during upload to the webstore.

        + /.build/Extension.crx
            - Compressed package for hosting your own extension with automatic updates

        + /.build/updates.xml
            - This file is for use with a hosted .crx - Enables auto updated by incrementing the version

