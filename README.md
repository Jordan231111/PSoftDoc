Here's the updated **Sam's Guide to the Gradliest VSCode** with **Step 6 removed** and adjustments based on the provided `build.gradle` file for further clarification. This guide is updated for Java 21 and Gradle 8.14 (as of 2025).

---

# Guide to the Gradliest VSCode

This guide will help you set up a Visual Studio Code (VSCode) environment using Gradle for working with Java in **psoft**, specifically for your cloned repository.

---

## Handy Tips
- **Command Palette**: Press `CTRL + SHIFT + P` (or `CMD + SHIFT + P` on Mac) to open a textbox to interact with VSCode.
- **Restart Java Language Server**: `>Java: Clean Java Language Server Workspace` fixes common Java-related problems after setup changes.

---

## Setup Instructions

### 1. **Git Clone Your Repository**
Navigate to the directory where you want to clone the repository using `cd`. Then run this command:
```bash
git clone https://submitty.cs.rpi.edu/git/u25/csci2600/HW_Folder/yej6
```

- Replace `HW_Folder` with the homework folder name (e.g., `hw00`, `hw01`).

After cloning, your directory structure will look like this:
```
<your-selected-directory>/
    yej6/
        (contains src, answers, docs, and other folders)
```

---

### 2. **Rename the Folder to Match the Required Project Name**
The professor's notes specify that the project folder should be named `csci2600-HW_Name` (e.g., `csci2600-hw0` for homework 0). To meet this requirement, rename the `yej6` folder accordingly.

For example, if you're working on `hw00`, run:
```bash
mv yej6 csci2600-hw0
```

Your directory structure should now look like:
```
<your-selected-directory>/
    csci2600-hw0/
        src/
        answers/
        docs/
```

This ensures your project matches the professor's requirement.

---

### 3. **Open the Renamed Folder in VSCode**
- Open VSCode and select the renamed folder (`csci2600-hw0`) as your workspace.

---

### 4. **Install Required Plugins**
- Install the [Java Extension Pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack).
- Install the [Gradle for Java VSCode Plugin](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-gradle).

---

### 5. **Download and Replace the Default `build.gradle`**
Download the `build.gradle` file from [this GitHub repository](https://github.com/Jordan231111/PSoftDoc).

#### Use `wget` to Download `build.gradle`:
Run this command in your terminal:
```bash
wget -O build.gradle https://raw.githubusercontent.com/Jordan231111/PSoftDoc/refs/heads/main/build.gradle
```

The downloaded `build.gradle` file looks like this (for reference):
```groovy
/**
 Template for Gradlew-y PSOFT projects
 */

// plugin for just running stuff
plugins {
    id "application"
}
// java !
apply plugin : "java" 
apply plugin: 'jacoco'

// repo we can yoink dependencies (like JUnit) from
repositories {
    // Use Maven Central for resolving dependencies.
    mavenCentral()
}

// tell it what our Main file is. Needs full package name and class name.
// So src/main/java/hw0/folder/anotherFolder/RandomHello.java would be hw0.folder.anotherFolder.RandomHello
ext {
    // MAKE SURE YOU CHANGE THIS FOR YOUR PROJECT !!!
    // it needs to be a class with a main(String[] args) method
    javaMainClass = "hw0.RandomHello"
    // so for hw0 this might be `hw0.RandomHello`
}

// tell the Application plugin what our main class is
application {
    mainClassName = javaMainClass
}

// add JUnit
dependencies {
    // Use JUnit 5
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.1'
    testImplementation 'org.hamcrest:hamcrest:2.2' // Needed for hw3
}

// make the test task use JUnit
test {
    // Use JUnit Platform for unit tests.
    useJUnitPlatform()
}

jacocoTestReport {
    dependsOn test // tests are required to run before generating the report
}
```

**Note**: The updated Gradle configuration for Java 21 support (as of 2025) would use these updated dependencies:
```groovy
// Updated JUnit 5 dependencies for 2025
testImplementation 'org.junit.jupiter:junit-jupiter:5.12.2' 
testImplementation 'org.hamcrest:hamcrest:3.0'
testRuntimeOnly 'org.junit.platform:junit-platform-launcher:1.12.2'

// Java 21 toolchain support
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}
```

---

### 6. **Set the Main Class**
You must configure the main class of your project in `build.gradle`. Look for this section:
```groovy
ext {
    // MAKE SURE YOU CHANGE THIS FOR YOUR PROJECT !!!
    javaMainClass = "hw0.RandomHello"
}
```

Update `javaMainClass` to point to your main class. For example, if your main class is in `src/main/java/hw0/Main.java`, set it to:
```groovy
javaMainClass = "hw0.Main"
```

This tells Gradle where to find the `public static void main(String[] args)` method.

---

### 7. **Update Gradle Wrapper (if needed)**
If your Java version is 21, update the Gradle wrapper to the compatible version 8.14:
```bash
./gradlew wrapper --gradle-version=8.14 && ./gradlew wrapper
```

---

### 8. **Verify Directory Structure**
Before proceeding, ensure your project structure looks like this:
```
csci2600-hw0/
    src/
        main/
            java/    (Java source files)
        test/
            java/    (JUnit test files)
    docs/            (Homework documentation)
    answers/         (Answers to assignment problems)
    build.gradle
    settings.gradle
    gradlew
```

The `src` folder should include:
- `main/java`: For your main Java files.
- `test/java`: For your test cases.

---

## How to Use Gradle
Use Gradle commands to build, run, and test your project. Run these commands from the project directory (`csci2600-hw0`).

### Running Code
- **Run the Main Method Without Arguments**:
  ```bash
  ./gradlew run
  ```
- **Run the Main Method With Arguments**:
  ```bash
  ./gradlew run --args="your args here"
  ```

### Running Tests
- **Run All Tests**:
  ```bash
  ./gradlew test
  ```

### Generate Coverage Report
- **Jacoco Coverage Report**:
  ```bash
  ./gradlew jacocoTestReport
  ```
  (Generates a report in `build/reports/jacoco`).

---

## JavaFX Setup (Optional)
If your homework requires JavaFX, update your `build.gradle` file to include the JavaFX plugin and configuration:
```groovy
plugins {
    id 'application'
    id 'java'
    id 'jacoco'
    id 'org.openjfx.javafxplugin' version '0.1.0'
}
javafx {
    version = '23.0.2'
    modules = ['javafx.controls', 'javafx.fxml']
}
```

---

## Java 21 Features (Optional)
Java 21 offers several powerful features you might want to use:

- **Virtual Threads**: Lightweight threads for high-throughput applications
- **Record Patterns**: For type-safe data extraction
- **String Templates**: More readable string formatting (Preview feature)

---

## Final Tips
1. **Run Tests and Code in VSCode**:
   - Look for "Run" and "Debug" buttons above `main` methods and next to test cases.
2. **Commit and Push**:
   - Use Git regularly to save your progress:
     ```bash
     git add .
     git commit -m "Meaningful commit message"
     git push origin main
     ```
3. **Submit and Grade on Submitty**:
   - Push your changes and click **Grade My Repository** to test your code on Submitty.
4. **Troubleshooting**:
   - If you see "Invalid source release: 21", update your IDE's Java version in Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JVM.

---

Let me know if this version works better for you!
