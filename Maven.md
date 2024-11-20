# MAVEN: Project Management Tool

Maven is a powerful **project management tool** used primarily for Java-based applications. It helps with project builds, dependency management, and other tasks such as version control, plugins, and external library integration.

---

## Why Use Maven?

- **External Libraries**: Maven helps you include external libraries in your project. For example, if you want to connect Java to a database, such as PostgreSQL, Maven makes it easy to include the PostgreSQL connector.
  
- **Easy Dependency Updates**: It simplifies the process of updating or upgrading your dependencies. Once the dependencies are defined, Maven handles their download, update, and maintenance.

- **Version Control**: It ensures that the versioning of dependencies is in sync. This minimizes the risk of version mismatches.

- **Plugins**: Maven offers a variety of plugins that help automate and streamline different tasks in the build process, such as compiling code, packaging, testing, and deployment.

- **Beginner-Friendly**: Maven is relatively easy to use and understand, making it ideal for beginners.

---

## Maven Alternatives

There are other build tools in the ecosystem, with **Gradle** and **Ivy** being the most notable alternatives to Maven. Both have their own strengths, but Maven is particularly favored for its stability and wide community support.

---

## Maven in IntelliJ IDEA

### 1. Creating a Maven Project in IntelliJ IDEA

To set up a Maven project in IntelliJ IDEA:
- Choose **Maven** as the build tool when creating a new project.
- Set the **build path** to Maven. This ensures that the project uses Maven for dependency management and builds.
<img src="https://github.com/adarshpandey18/notes/blob/main/images/intellij-gradle-project-create.png" width="600">
  
> **Note:** In some cases, projects created in Eclipse might face issues when opened in IntelliJ IDEA. However, with Maven, the folder structure remains consistent across different IDEs.

### 2. Maven Lifecycle

On the top right of IntelliJ IDEA, you will see the Maven logo. Clicking on it will show you the **Maven lifecycle**, which outlines the steps involved in building your project (e.g., **compile**, **test**, **package**, **install**).

<img src="https://github.com/adarshpandey18/notes/blob/main/images/maven-lifecycle.png" width="600"> 

---

### Getting Dependencies

1. **Search for Dependencies**:
   - To add external libraries (like a MySQL connector for Java), you can download the JAR file manually. However, maintaining version control is important, so you should use **Maven Repository** (https://mvnrepository.com/).

2. **Add Dependency to `pom.xml`**:
   - Go to [Maven Repository](https://mvnrepository.com/) and search for your desired dependency (e.g., "MySQL connector for Java").
   - Click on the **Maven** link for the correct version and copy the `<dependency>` tags provided.

3. **Paste in `pom.xml`**:
   - Open your `pom.xml` file and paste the copied dependency inside the `<dependencies>...</dependencies>` section.
   - **Reload** Maven to download the jar from the repository.

   Example:
   ```xml
   <dependencies>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.30</version>
       </dependency>
   </dependencies>
### Key Elements in Dependencies

- **Group Id**: A unique identifier for the group or organization responsible for the artifact.
- **Artifact Id**: The name of the artifact (library or module).
- **Version**: The version of the artifact being used.

### Transitive Dependency

A **transitive dependency** is a dependency of a dependency. This means that if your project relies on a library that itself has dependencies, Maven will automatically manage and download those as well.

---

## Effective POM

The **Effective POM** in Maven refers to the final configuration that Maven uses during the build process. It is a combination of:
- The project's own `pom.xml`.
- Any parent `pom.xml` files.
- Active profiles.
- Default values.

To view the Effective POM in IntelliJ:
- Right-click on `pom.xml` and select **Maven** > **Show Effective POM**.

---

## Maven Archetype

A **Maven Archetype** is a template provided by developers to quickly create a new project structure. It contains predefined project files and setup, making it easier for developers to start a project with standard configurations.

### Creating a Project Using Archetype:
- When creating a new project, you can select the **Maven Archetype** option.
- **Catalog Options**:
  - **Internal**: Archetypes stored locally on your system.
  - **Maven Central**: Archetypes available from Maven's online repository.
<img src="https://github.com/adarshpandey18/notes/blob/main/images/maven-archetype.png" width="600">

**Maven Archetypes** make it much easier to get started with common Java project structures, ensuring that all the required files and configurations are set up automatically.

---
