# Windows setup and install guide

## Step 1: Install the **Java Development Kit ("JDK")**
  * Latest Windows version: https://www.oracle.com/java/technologies/downloads/#jdk21-windows
    * Recommend one of the installer options (e.g. "**x64 MSI Installer**")
    * ⚠️ Take note of the directory you installed this in (e.g. `C:\Program Files\Java\jdk-21`)
  * There's a chance Forge has fewer bugs with older versions of JDK based on posts I've read, but I don't know for sure; the latest works well enough for me
    * [JDK 11](https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html) (lowest version supported by the Eclipse IDE)
    * [JDK 8](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html] (lowest overall supported version, some Forge devs stick to to this)
    * All archived Java editions: https://www.oracle.com/java/technologies/downloads/archive/
    * ⚠️ **I recommend ignoring these old versions and sticking to the latest, at least for initial setup and tinkering** (just have these for reference later or potential troubleshooting if you have issues with the latest)
    * ⚠️ Installing more than one version will make the next steps a little harder

## Step 2: Set the `JAVA_HOME` environment variable
  - *(Guide I used to write these steps: https://stackoverflow.com/questions/1672281/how-to-set-the-environment-variables-for-java-in-windows)*
  - a) **Windows 11**: Go to **This PC** ➡️ **Properties** ➡️ **Advanced** ➡️ **Environment Variables**
  - b) Under "**User variables for [*login name*]**", click **New**
    - Set the **variable name** to "`JAVA_HOME`"
    - Set the **variable value** to the Java installation's version directory
      - e.g. `C:\Program Files\Java\jdk-21` if you installed version 21 (the latest as of this writing) to the Program Files folder
  - ⚠️ You will need to update ***this variable only*** (repeat this step) if you update JDK or change its install location

## Step 3: Set the other Java environment variables
  - a) Repeat the process above to add the following USER (not system) name/value pairs:
    - *(variable name)* ➡️ *(value)*
    - `JDK_HOME`&nbsp;➡️ `%JAVA_HOME%`
    - `JRE_HOME`&nbsp;➡️ `%JAVA_HOME%\jre`
    - `CLASSPATH`&nbsp;➡️ `.;%JAVA_HOME%\lib;%JAVA_HOME%\jre\lib`
    - `PATH`&nbsp;➡️ `your-unique-entries;%JAVA_HOME%\bin`
    - `JAVA_TOOL_OPTIONS`&nbsp;➡️ `-Dfile.encoding="UTF-8"`
  - b) Under "**System variables**", select and edit the "**Path**" variable
    - Remove Oracle's variables (e.g. `C:\ProgramData\Oracle\Java\javapath;`)
      - These installed with your JDK installer in **Step 1**
      - We remove these because they may not get updated with JDK...
      - ...whereas we now only have to update the "`JAVA_HOME`" variable's path (**Step 2**) after an update since the rest of these variables/paths are derived from it (using `%JAVA_HOME%`)
  - Java's environment variables should now be set up
    - You can test by opening command prompt and typing "`java`"

## Step 4: Install Apache Maven
  - *(Guide I used: https://maven.apache.org/install.html)*
  - Download a Maven *binary* (not *source*) archive: https://maven.apache.org/download.cgi
  - Extract it anywhere (e.g. `E:\Studio\_\Maven\apache-maven-3.9.5`)
  - Set Maven's environment variable
    - Add its "`bin`" subdirectory to your ***System***'s "`Path`" environment variables (as in **Step 3b**; *not* the User variables from the earlier steps )
    - e.g. `E:\Studio\_\Maven\apache-maven-3.9.5\bin`
  - Confirm the installation with "`mvn -v`" in a new shell

## Step 5: Clone down the Forge repo (*Card-Forge/forge*)
  * ⚠️ **You will need at least 1.2 GB of space — possibly as much as 20 GB depending on card image downloads — for this + stuff from other steps**
  * https://github.com/Card-Forge/forge
    * Their steps say to fork the repo to your GH profile, then clone it down
    * I just straight cloned it down without forking; I don't think it led to any of my jankiness and I'm able to work just fine, but it may be better to do exactly as they say — whichever you prefer
  * In a shell/terminal, go to the cloned repository's root directory (e.g. `cd "E:\Studio\Development\Card Forge\forge"`) and run "`mvn -U -B clean -P windows-linux install`"
    - This will get Maven to see/build all the random project files I don't understand yet

## Step 6: Install the Eclipse IDE
  - You can code in whichever IDE you want (I use VS Code), but I have found no other way of successfully *building* the Forge program and testing it than using Eclipse; if I figure out a way to get VS Code to build the app, I'm uninstalling Eclipse
  - Download page: https://www.eclipse.org/downloads/
    - Direct download page if their page design baffles you as much as it did me: https://www.eclipse.org/downloads/download.php?file=/oomph/epp/2023-09/R/eclipse-inst-jre-win64.exe

## Step 7: Setting up the Forge environment in Eclipse
  - Open Eclipse
  - Go to **File** ➡️ **Import...**
  - Expand the **Maven** directory, select "**Existing Maven Projects**", and click **Next >**
  - Next to "**Root Directory:**" click the **Browse...** button and open the cloned repo's root directory (e.g. `E:\Studio\Development\Card Forge\forge`)
  - Make sure *all* the boxes are checked and click the **Finish** button
    - If you're wondering, the top-level `/pom.xml` file is the overall build instructions for the `forge` repo's Maven (Java framework) projects; all of the nested `forge-(blah)/pom.xml` files are sub-projects, like the separate mobile and desktop apps

## Step 8: Building a testable instance of Forge
  - *(Feel free to make changes in your IDE of choice, like changing a card's text, to test this; Step 8 here is only for testing any changes as you develop)*
  - In the **Project Explorer** pane, right-click on `forge-gui-desktop [forge master]`
  - Select **Run As** ➡️ **Java Application**
  - In the window that appears, scroll down until you find **Main - forge.view** then click **OK**
    - 📌 Later, **Main - forge.view** should appear *above* this long list since it will now be in your history; less scrolling!
  - You should now see the Forge splash/startup screen
  - If you get errors while it's loading, you may be fine
    - This happens to me every time: I get exactly 10 errors in a row and click **Discard** each time, then the program runs without any issue I've noticed
    - I haven't troubleshot this yet; no idea what's causing it but could be my Java version being too new, or straight cloning down the repo rather than forking it, or nothing I did, or any number of things — I'm not really concerned for now but I'll let you know if I do figure it out

## That's everything for dev! If you want to just *play* Forge, here's the [User Guide](https://github.com/Card-Forge/forge/wiki/User-Guide)
  - The direct download "page" that links to: https://downloads.cardforge.org/dailysnapshots/
    - ⚠️ **You will need about 700 MB of space for this, possibly much more than that as you download card images**
    - Download the `forge-desktop` file (should end in `-installer.jar`)
    - Extract it anywhere; it's basically Java's idea of a `zip` file
    - 📌 **To play, open the `forge-java8.exe` file, not the `forge.exe` file**
