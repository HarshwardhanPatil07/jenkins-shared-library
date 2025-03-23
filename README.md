# jenkins-shared-library
![SharedLib](/assets/SharedLib.png)
![DockerJMA3.0](/assets/DockerJMA3.0.png)

Below is a detailed step‐by‐step guide on how to configure a Jenkins Shared Library:

---

### 1. Prepare Your Shared Library Repository

- **Create a Repository:**  
  Set up a separate Git repository (e.g., on GitHub, Bitbucket, or GitLab) that will host your shared library.

- **Directory Structure:**  
  Organize the repository with the proper directory structure. A typical layout includes:
  - **`vars/`** – Contains global variables as Groovy scripts (each file defines a callable function).  
  - **`src/`** – Contains Groovy classes and packages.  
  - Optionally, you can include **`resources/`** for external files or configuration.

  **Example structure:**
  ```
  ├── vars
  │   └── myGlobalVar.groovy
  ├── src
  │   └── org
  │       └── example
  │           └── MyClass.groovy
  └── README.md
  ```

---

### 2. Configure the Shared Library in Jenkins

- **Access Global Configuration:**
  1. In Jenkins, click on **"Manage Jenkins"**.
  2. Select **"Configure System"**.

- **Locate the Global Pipeline Libraries Section:**
  Scroll down until you find the **"Global Pipeline Libraries"** section.

- **Add a New Library:**
  - Click **"Add"** to create a new library configuration.
  
  **Fill in the following fields:**
  - **Name:**  
    This is the identifier you will reference in your Jenkinsfile (e.g., `my-shared-library`).

  - **Default Version:**  
    Specify the branch, tag, or commit that should be used by default (e.g., `main` or `master`).

  - **Retrieval Method:**  
    - Choose **"Modern SCM"**.
    - Under **"Source Code Management"**, select your SCM type (e.g., Git).

  - **Repository URL:**  
    Provide the full Git URL to your shared library repository.

  - **Credentials:**  
    If your repository is private, add and select the appropriate credentials.

- **Advanced Options (Optional):**
  - **Implicit Loading:**  
    If you enable this option, the library is automatically loaded for every pipeline without needing to add `@Library` annotation in the Jenkinsfile.  
    *(Typically, it’s better to load libraries explicitly to avoid accidental namespace collisions.)*

---

### 3. Use the Shared Library in Your Pipelines

- **Reference in Your Jenkinsfile:**  
  In your pipeline script, reference the shared library using the `@Library` annotation.

  **Example:**
  ```groovy
  @Library('my-shared-library') _
  pipeline {
      agent any
      stages {
          stage('Example') {
              steps {
                  script {
                      // Assuming you have a global variable defined as "myGlobalVar" in the vars directory.
                      myGlobalVar('Hello from shared library!')
                  }
              }
          }
      }
  }
  ```

- **Calling Library Functions or Classes:**  
  - For global variables (from the `vars` folder), you can call them as functions directly.
  - For classes (from the `src` folder), import and instantiate them as needed:
    ```groovy
    import org.example.MyClass
    def instance = new MyClass()
    instance.doSomething()
    ```

---

### 4. Verify and Test

- **Test Your Configuration:**  
  - Commit and push any changes in the shared library repository.
  - Run a pipeline job that references the shared library to verify that everything loads correctly.
  - Check the Jenkins job log for any errors related to library loading or script execution.