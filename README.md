# ## Packaging Java 11 + OpenJFX 11 Application in slim runtime

This is a simple sample on how to create a custom jre for a javafx maven project.
Make sure your java_home points to a proper JDK 11.


## 1. Build project with maven command
`mvn clean compile package exec:java`

## 2. Create custom jre

- Place your openjfx 11 .jmod files in the folder _jmods_
- run `mvn clean compile package` and check if your projects .jar is placed in the target folder
- run jlink-command with --module-path pointing to your jmods and your target jar.
- add --add-modules and pass your module name as an argument (your module name can be found in your module-info.java file)
- define the output folder which will be created, in my example it is "jre", althoug technically the output contains the 
runtime dependencies as well as your own module
- additionally add the flag --compression={0|1|2} 
- CAUTION: there seems to be a bug currently with the AdoptOpenJdk that uses OpenJ9 as JVM when you chose compression level 2!
If you get some weird errors, use the HotSpotVM!


`jlink --module-path "jmods/;target\example-0.0.1.jar" --add-modules example --output jre`

You can use this simple project to test functionality before applying the process to your own Project.


## 3. Run application
Run your Main class with the java command. This will use your newly created custom runtime.
First part is the path of your custom runtime, the last part consists of {modulename, as declared in module-info}/{fully.qualified.classname.of.Main}
`jre/bin/java -m example/de.soenkekrusche.example.Main`

Or start the .bat file (windows only) that you can find in the root directory of this project.
Adjust the paths in the start.bat to your desired paths/filenames.

Note: non-tech users might be afraid to run .bat files on their systems. I will provide a simple .cpp file that you can 
compile on your own to create a simple .exe as a runner for the commands used in the .bat file. Needs optimization though.
Windows only. 

## features to come
Whenever I get to have some spare time I might write a simple c/c++ app where you can chose your custom jre+module path to create a custom runner.


