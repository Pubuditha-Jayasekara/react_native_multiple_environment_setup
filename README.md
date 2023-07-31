🚀 **React Native Multiple Environment configuration (Android & IOS)**

Many times when developing an application, we developers need to create different builds with different configurations. Facilitating the maintenance and testing process. Usually 3 different builds are created: development, staging and production.  

This repository serves as a comprehensive guide, accompanied by illustrative examples, demonstrating how to efficiently achieve this build differentiation. By following this guide, developers can optimize their workflow and ensure smoother transitions between development stages while enhancing the overall quality of their applications.

🛠️ **Pre-Requisites for React Native Multiple Environment Configuration**

Before you begin setting up multiple environment configurations in your React Native project, ensure you have the following pre-requisites in place:

**1. React Native Development Environment:**

Make sure you have a fully functional React Native development environment set up on your machine, including Node.js, npm (Node Package Manager), and the React Native CLI.

**2. React Native Project:**

Have an existing React Native project ready. If you don't have one yet, create a new project using the React Native CLI.

📱 **Android Configuration:**

1. Install react-native-config package.

   ```
   yarn add react-native-config
   ```

You can find the package on GitHub [here](https://github.com/luggit/react-native-config). The react-native-config package allows you to efficiently manage environment-specific configurations in your React Native project, making it simpler to handle different builds with varying settings.

3. Create separate “**.env**” files for desired environments in the project root directory. In mine I have '**.env**' for **default configurations**,‘**.env.dev**’ for **development configuration**, ‘**.env.prod**’ for **production configuration** and ‘**.env.qa**’ for **qa testing configurations**.

  * EX path for ‘.env.dev’ : projectName/.env.dev
   
<div align="center">
   <img width="362" alt="Screenshot 2023-07-31 at 11 27 10 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/1d104e1a-8b94-4108-9ad2-f3fe31de30cf">
</div>

4. Update each .env file to contain following details.

   ```
   APP_VERSION="1.0"
   BUILD_NUMBER=9
   ```
   <div align="center">
       <img width="1440" alt="Screenshot 2023-07-31 at 11 29 00 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/4c184a57-c2bd-4f68-953e-b77ee017465c">
   </div>

5. Go to the app level ‘build.gradle’ file in "/your project directory/android/app/build.gradle"

   <div align="center">
     <img width="1221" alt="Screenshot 2023-07-31 at 10 14 43 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/c5eca926-d552-4363-9bad-943b4cc84aeb">
   </div>

6. Add the following environment configurations at the very top of the ‘build.gradle’ file but below the apply plugin: "com.android.application" , apply plugin: "com.facebook.react" and import com.android.build.OutputFile lines.

   ```
         //MARK: to handle each environment .env file
         project.ext.envConfigFiles = [
             devDebug: ".env.dev",
             devRelease: ".env.dev",
             qaRelease: ".env.qa",
             qaDebug: ".env.qa",
             prodRelease: ".env.prod",
             prodDebug: ".env.prod"
         ]
   ```

   <div align="center">
      <img width="863" alt="Screenshot 2023-07-31 at 10 26 23 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/d6f91f50-78d5-4643-8fff-b6c80c2f8978">
   </div>
   
7. Next add the following line below the ‘project.ext.envConfigFiles‘ configuration.

   ```
      apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"
   ```
   
   <div align="center">
      <img width="1037" alt="Screenshot 2023-07-31 at 10 29 17 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/4f121d33-452e-4a4d-8018-f30ceb62b358">
   </div>

8. Add the product flavors inside the ‘android{} ‘ below the ‘buildTypes{}’ in ‘build.gradle’ file.

   ```
        //MARK: to handle each environment flavor
        productFlavors {
            dev {
                dimension "default"
                minSdkVersion rootProject.ext.minSdkVersion
                applicationId 'com.daily.dailydev'
                targetSdkVersion rootProject.ext.targetSdkVersion
                resValue "string", "build_config_package", "com.daily"
                resValue "string", "app_name" , "Daily Dev"
                versionCode project.env.get("BUILD_NUMBER").toInteger()
                versionName project.env.get("APP_VERSION")
            }
            qa {
                dimension "default"
                minSdkVersion rootProject.ext.minSdkVersion
                applicationId 'com.daily.dailyqa'
                targetSdkVersion rootProject.ext.targetSdkVersion
                resValue "string", "build_config_package", "com.daily" 
                resValue "string", "app_name" , "Daily Qa"
                versionCode project.env.get("BUILD_NUMBER").toInteger()
                versionName project.env.get("APP_VERSION")
            }
            prod {
                dimension "default"
                minSdkVersion rootProject.ext.minSdkVersion
                applicationId 'com.daily.dailyprod'
                targetSdkVersion rootProject.ext.targetSdkVersion
                resValue "string", "build_config_package", "com.daily" 
                resValue "string", "app_name" , "Daily Prod"
                versionCode project.env.get("BUILD_NUMBER").toInteger()
                versionName project.env.get("APP_VERSION")
            }
        }
   ```

   <div align="center">
      <img width="1123" alt="Screenshot 2023-07-31 at 10 39 22 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/14e30996-4882-45a3-a89f-7e92561c5c85">
   </div>

🪚 **Let's break down the code:**

- **productFlavors**: This is a section that defines multiple product flavors, each representing a different environment configuration.

- **dev**: This flavor represents the development environment. It specifies the following configurations:
    - `dimension "default"`: This sets the flavor dimension to "default."
    - `minSdkVersion rootProject.ext.minSdkVersion`: Sets the minimum SDK version for this flavor, which is retrieved from a variable defined in the root project's gradle.properties.
    - `applicationId 'com.daily.dailydev'`: Sets the application ID (package name) for the development flavor.
    - `targetSdkVersion rootProject.ext.targetSdkVersion`: Sets the target SDK version for this flavor, which is retrieved from a variable defined in the root project's gradle.properties.
    - `resValue "string", "build_config_package", "com.daily"`: This adds a resource value with the name "build_config_package" and the value "com.daily" to the flavor's resources.
    - `resValue "string", "app_name" , "Daily Dev"`: This adds a resource value with the name "app_name" and the value "Daily Dev" to the flavor's resources.
    - `versionCode project.env.get("BUILD_NUMBER").toInteger()`: Sets the version code for the development flavor, which is retrieved from an environment variable named "BUILD_NUMBER."
    - `versionName project.env.get("APP_VERSION")`: Sets the version name for the development flavor, which is retrieved from an environment variable named "APP_VERSION."

- **qa**: This flavor represents the QA testing environment. It has similar configurations to the dev flavor but with different values for the application ID and app name to distinguish it as the QA build.

- **prod**: This flavor represents the production environment. It again shares similar configurations with the previous two flavors but with a unique application ID and app name to identify it as the production build.

9. Once you update the app name in the ‘app.gradle’ file you need to comment or remove the app name from the res file located in "your project directory/android/app/src/main/res/values/strings.xml"

   <div align="center">
     <img width="777" alt="Screenshot 2023-07-31 at 10 46 45 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/6e732727-efa0-4982-a11f-cab33ab59466">
   </div>

**Important**

10. You have to provide a valid flavorDimensions once you add the productFlavors. To achive this you can place the following code inside buildTypes{} before the debug{} and release{} code segments.

    ```
      //MARK: set flavour dimension 
      flavorDimensions "default"
    ```

    <div align="center">
       <img width="804" alt="Screenshot 2023-07-31 at 11 23 09 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/a98853a1-2cff-4fd2-bf1b-a4972cf23fba">
   </div>

📄 **Manage Multiple Google-Services.Json configurations:**

If you have multiple google-services.json files: you need to add following code after adding product flavors code in ‘build.gradle’ file as follows. This also needs to be inside the  ‘android{}’ section.

   ```
        //MARK: to handle google service.json per each product flavor
        applicationVariants.all { variant ->
            variant.productFlavors.each { flavor ->
                // Get the flavor-specific `google-services.json` file path
                def googleServicesJsonPath = "src/${flavor.name}/res/raw/${flavor.name}.json"

                variant.registerGeneratedResFolders(files(googleServicesJsonPath))

                // Apply the `google-services` plugin with the flavor-specific file
                variant.buildConfigField "String", "GOOGLE_SERVICES_JSON_FILE", "\"${googleServicesJsonPath}\""
                variant.resValue "string", "google_services_json_file", googleServicesJsonPath
            }
        }
   ```

   <div align="center">
      <img width="1275" alt="Screenshot 2023-07-31 at 11 37 28 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/f55535d4-d28f-43ab-a885-af80fb93ac2e">
   </div>

**Important**

Once you update multiple google-service.json handling part in app.gradle, you need to update the android folder structure to match each json file for each environment like follows.

   1. Open your Android project folder.
   2. Navigate to the ‘src’ folder inside the project/android/app.
   3. Inside the ‘src’ folder, create three new folders: qa, dev, and prod.
   4. Inside each of the newly created qa, dev, and prod folders, create a folder named ‘res’. 
   5. Now, within each res folder (inside qa, dev, and prod), create a folder named ‘raw’. This raw folder will be used to store the google-service.json files for each environment.
   6. Finally, you need to add the appropriate google-service.json file to each raw folder. Make sure you rename each file to match its respective environment. 

For example, the google-service.json file inside the dev folder should be renamed to dev.json. Similarly, rename the files in the other folders also.

   <div align="center">
        <img width="299" alt="Screenshot 2023-07-31 at 11 00 02 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/6b6f0735-489c-4128-880d-2a49a59224ee">
   </div>
   
**Let's Run Each Flavour**

 1. Update the ‘package.json’ file with following code to reflect the changes and run each environment using yarn/npm.
   
   ```
          "android:dev": "react-native run-android --variant=devDebug --appId com.daily.dailydev",
          "android:dev-release": "react-native run-android --variant=devRelease --appId com.daily.dailydev",
          "android:qa": "react-native run-android --variant=qaDebug --appId com.daily.dailyqa",
          "android:qa-release": "react-native run-android --variant=qaRelease --appId com.daily.dailyqa",
          "android:prod": "react-native run-android --variant=prodDebug --appId com.daily.dailyprod",
          "android:prod-release": "react-native run-android --variant=prodRelease --appId com.daily.dailyprod"
   ```

   <div align="center">
       <img width="1169" alt="Screenshot 2023-07-31 at 11 06 42 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/b041fad6-7b41-42c1-9b21-a45d027ec9d5">
   </div>

2. In order to run each environment you can use the command ‘yarn android:dev’ etc.

**Dev Debug:**

```
yarn android:dev
```
      
**Dev Release:**

```
Yarn android:dev-release
```

**Qa Debug:**

```
yarn android:qa
```

**Qa Release:**

```
Yarn android:qa-release
```

**Prod Debug:**

```
yarn android:prod
```

**Prod Release:**

```
Yarn android:prod-release
```

**Once you run each falvour there should be three different apps installed like below.**

   <div align="center">
   <img width="360" alt="Screenshot 2023-07-31 at 11 47 05 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/14d69bf3-c638-4691-a74c-f3fa8bfe1420">
   </div>


📱 **iOS Configuration:**

1. First we need to add new build configurations.

   *Select project name -> goto info tab -> select configurations.

   <div align="center">
      <img width="1150" alt="Screenshot 2023-07-31 at 12 51 48 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/ebe248e3-9973-4de6-8ff2-69d91bf79319">
   </div>

2. Click on the plus sign below in ‘**Configurations**’ and select ‘**Duplicate Debug Configuration**’ for debug builds and ‘**Duplicate Release Configuration**’ for release builds and rename it to match your desired schemas.

   <div align="center">
      <img width="725" alt="Screenshot 2023-07-31 at 12 57 14 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/5f2c0596-295e-4bf5-afdf-c304d7e1f4a6">
   </div>

<div align="center">
      <img width="798" alt="Screenshot 2023-07-31 at 12 59 40 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/bdd3cada-d6a0-4f98-9202-afebba3ee4b3">
</div>
      
 3. Next we need to create required schemes (dev, prod and qa).

    a. Goto product -> scheme -> edit scheme.

<div align="center">
    <img width="1187" alt="Screenshot 2023-07-31 at 1 08 04 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/51bf8a8e-8c09-4ce8-9f99-fb9be0ad0ffc">
</div>


   b. The menu will be selected to your default scheme and you need to click on the duplicate scheme at the very bottom. This will duplicate your current scheme and you can configure this scheme by changing its name to desired (dev, prod and qa) also we need to update the Run and Archive configuration as described below.

   <div align="center">
      <img width="941" alt="Screenshot 2023-07-31 at 1 10 12 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/4ebee4d9-ca76-418a-8202-0c6a645ee212">
   </div>


   c. Just make sure that schema that you duplicate is “shared”  and has the right build configuration reference in Run and Archive .

   <div align="center">
      <img width="934" alt="Screenshot 2023-07-31 at 1 16 49 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/9554b789-406d-4dd8-8ec4-a3c06dc0c5f6">
   </div>

<div align="center">
   <img width="943" alt="Screenshot 2023-07-31 at 1 23 58 PM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/66575181-3a64-44b2-9bc3-442b7962d722">
</div>

   

   
