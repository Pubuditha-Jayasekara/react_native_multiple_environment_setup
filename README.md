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

3. Create separate “**.env**” files for desired environments in the project root directory. In mine I have ‘**.env.dev**’ for **development configuration**, ‘**.env.prod**’ for **production configuration** and ‘**.env.qa**’ for **qa testing configurations**.

  * EX path for ‘.env.dev’ : projectName/.env.dev
   
<div align="center">
  <img width="246" alt="Screenshot 2023-07-31 at 10 02 36 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/e4527a80-bc33-4207-8e54-f46ac6c04d33">
</div>

4. Update each .env file to contain following details.

   ```
   APP_VERSION="1.0"
   BUILD_NUMBER=9
   ```
   <div align="center">
      <img width="1013" alt="Screenshot 2023-07-31 at 10 11 18 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/17171cdd-e8fb-48cf-8d1c-908402c68593">
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

📄 **Manage Multiple Google-Services.Json configurations:**

If you have multiple google-services.json files: you need to add following code after adding product flavors code in ‘build.gradle’ file as follows. This also needs to be inside the  ‘android{}’ section.

   ```
       // applicationVariants are e.g. debug, release
            applicationVariants.all { variant ->
                variant.outputs.each { output ->
                    // For each separate APK per architecture, set a unique version code as described here:
                    // https://developer.android.com/studio/build/configure-apk-splits.html
                    // Example: versionCode 1 will generate 1001 for armeabi-v7a, 1002 for x86, etc.
                    def versionCodes = ["armeabi-v7a": 1, "x86": 2, "arm64-v8a": 3, "x86_64": 4]
                    def abi = output.getFilter(OutputFile.ABI)
                    if (abi != null) {  // null for the universal-debug, universal-release variants
                        output.versionCodeOverride =
                                defaultConfig.versionCode * 1000 + versionCodes.get(abi)
                    }
                }
            }
   ```

   <div align="center">
         <img width="1201" alt="Screenshot 2023-07-31 at 10 52 07 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/f79b85fa-35e3-4c15-b4ff-aa88b043701b">
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

*Dev Debug:

   ```
         yarn android:dev
   ```
      
*Dev Release:
   ```
      Yarn android:dev-release
   ```
*Qa Debug:
   ```
      yarn android:qa
   ```
*Qa Release:
   ```
      Yarn android:qa-release
   ```
*Prod Debug:
   ```
      yarn android:prod
   ```
*Prod Release:
   ```
      Yarn android:prod-release
   ```
