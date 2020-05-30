![Gradle Package](https://github.com/BorderCloud/SPARQL-ANDROID/workflows/Gradle%20Package/badge.svg)

# SPARQL-ANDROID

This project uses the [SPARQL-JAVA](https://github.com/BorderCloud/SPARQL-JAVA) project as a basis.

Examples : look at [com.bordercloud.sparqlAndroidApp.MainActivity](https://github.com/BorderCloud/SPARQL-ANDROID/blob/master/app/src/main/java/com/bordercloud/sparqlAndroidApp/MainActivity.java)


## Install this module inside the Android application

> Steps 1 and 2 can be skipped if already followed while publishing a library

### Step 1 : Generate a Personal Access Token for GitHub
- Inside you GitHub account:
	- Settings -> Developer Settings -> Personal Access Tokens -> Generate new token
	- Make sure you select the following scopes ("read:packages") and Generate a token
	- After Generating make sure to copy your new personal access token. You won’t be able to see it again!

### Step 2: Store your GitHub - Personal Access Token details
- Create a **github.properties** file within your root Android project
- In case of a public repository make sure you  add this file to .gitignore for keep the token private
	- Add properties **gpr.usr**=*GITHUB_USERID* and **gpr.key**=*PERSONAL_ACCESS_TOKEN*
	- Replace GITHUB_USERID with personal / organisation Github User ID and PERSONAL_ACCESS_TOKEN with the token generated in **#Step 1**

> Alternatively you can also add the **GPR_USER** and **GPR_API_KEY** values to your environment variables on you local machine or build server to avoid creating a github properties file

### Step 3: Update build.gradle inside the application
- Add the following code to **build.gradle** inside the application module that will be using the library published on GitHub Packages Repository

```markdown
def githubProperties = new Properties()
githubProperties.load(new FileInputStream(rootProject.file("github.properties")))
```

```markdown
    repositories {
        maven {
            name = "GitHubPackages
            url = uri("https://maven.pkg.github.com/bordercloud/SPARQL-ANDROID")

            credentials {
                username = githubProperties['gpr.usr'] ?: System.getenv("GPR_USER")
                password = githubProperties['gpr.key'] ?: System.getenv("GPR_API_KEY")
            }
        }
    }
```

- inside dependencies of the build.gradle of app module, use the following code
```markdown
dependencies {
    //consume library
    implementation 'com.bordercloud:SPARQL-ANDROID'
	...
}
```

- If you have a problem, try
```markdown
android {
        ...

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

More details : [Sample Android Library Publishing to GitHub Package Registry](https://github.com/enefce/AndroidLibraryForGitHubPackagesDemo/blob/master/README.md)