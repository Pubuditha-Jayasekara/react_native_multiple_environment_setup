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
   
<div align="center">
  <img width="246" alt="Screenshot 2023-07-31 at 10 02 36 AM" src="https://github.com/Pubuditha-Jayasekara/multiple_environment_setup/assets/35820857/e4527a80-bc33-4207-8e54-f46ac6c04d33">
</div>

4. 
