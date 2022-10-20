# How to run manual tests on real devices using LambdaTest

## Goal

In this tutorial, you will learn what manual testing is, what LambdaTest is, and how to use Codemagic with LambdaTest to test a Flutter application.

## Prerequisites

- A basic understanding of how to use Codemagic workflows
- Basic Flutter knowledge

## What is testing?

Before software products are shipped to the market, dedicated software testers are hired to test these products to ensure they are not substandard and meet a specific level of performance. Software testing is the process of evaluating a software product to ensure it works as expected.

## Types of testing

Different testing techniques can be organized into two main categories:

**Manual testing**

Usually, testers are hired to test software products without using any automated tools. In this case, they simulate the actions an end user might take when using the product. The purpose of manual testing is to check for bugs, performance issues, and other defects.

Manual testing is imperative, as automation is not possible 100% of the time. All newly created software is manually tested before conducting automated testing. Although manual testing is more “primitive” than automated testing, it can still be very effective.

**Automation testing**

Though manual testing can help uncover many issues in a software application, it is still not sufficient for performing other repetitive tasks that are difficult to carry out manually, like unit testing, smoke testing, regression testing, and API testing. It’s usually not feasible to test the written logic of your app manually.

To use automation tests efficiently, you have to write test scripts or use automation tools to help you perform these tasks.

## What is LambdaTest?

Testing products over different devices (and view port sizes) can be difficult, costly, and time consuming, especially when you have to run them on real devices. Even if it is possible to have some of these real devices, how many can you buy to cover all the different screen sizes and other specifications? And even if you get different devices for each platform, managing various versions might not be so easy. You would need to delete a particular version to install a new one, but what if you need it for some other product?

This is where LambdaTest, a Continuous Quality Testing Platform on Cloud, can be extremely useful since it supports manual and automated app testing on a Real Device Cloud. This lets you test your applications without worrying much about the procurement and maintainance of real mobile devices.

[Wali - Can you please add details about the frameworks we support, device landscape, and any further information that will add value to the blog]

## Uses of LambdaTest

Some of the main features of LambdaTest include:

**The ability to test responsiveness on all screens**

With LambdaTest, you can test your application on different screen sizes and resolutions all in one place with one click. It is possible to test locally hosted apps using LambdaTest tunnel.

**Compatibility testing**

With LambdaTest, you can perform compatibility tests of your mobile and web applications across different real devices, screen sizes, and operating systems. You can also perform compatibility testing across different browsers, browser versions, operating systems, and resolutions in real time.

**Testing across the latest desktop browsers**

Have you been wondering about how you can test a website or web application on different browsers and even operating systems?

LambdaTest has made it simple to test across the latest desktop browsers, such as Google Chrome, Internet Explorer, Brave, Edge, Mozilla Firefox, Safari, Opera, and many more. You can run cross browser tests using manual as well as automated approach. LambdaTest suports automated web testing using popular test automation frameworks like Selenium, Cypress, Playwright, and Puppeteer.

**Integration with Tools and Frameworks**

LambdaTest offers real and live team support. LambdaTest supports integration with close to 120+ tools and frameworks, ranging from CI/CD tools, codeless automation tools, reporting tools, team communication tools and more. You can find more about [LambdaTest integrations](https://www.lambdatest.com/integrations) to deep dive into LambdaTest integrations landscape.

## How to use Codemagic with LambdaTest

LambdaTest provides two environments: **real-time testing** and **automation testing**. To use the Real-Time Testing environment in LambdaTest, Codemagic has to build our app in debug mode and make a cURL request. Automation testing needs to detect written test scripts to be able to use the automation environment.

Furthermore, the test scripts are written with a test framework like [Appium](https://appium.io/docs/en/about-appium/getting-started/index.html). For Flutter, you can use [Appium Flutter Driver](https://github.com/appium-userland/appium-flutter-driver).

In this tutorial, we will be using the real-time environment to test our app manually. The app we will be using fetches the author details from an endpoint and displays their name in a ListWidget on the initial page. When the user taps an author, more detailed data is shown on another page.

The state management solution used is flutter_bloc with unit testing.

`**Note: You can use an already existing app or build your own.**`

To get started, you can clone the app [here](https://github.com/maxiggle/Flutter-LambdaTest.git).

## codemagic.yaml file

Codemagic has a workflow UI editor for Flutter applications to enable you to easily set up a CI/CD pipeline. But to work with LambdaTest, we need to use `codemagic.yaml` because we will be writing a custom script.

You can find the complete script in my [repository](https://github.com/maxiggle/Flutter-LambdaTest/blob/master/codemagic.yaml).

          - name: Submitting app to LambdaTest
            script: |
              cd ./build/app/outputs/apk/release
              curl --location --request POST "https://manual-api.lambdatest.com/app/upload/realDevice" --header "Authorization: Basic $LAMBDATEST " --form "name='lambda1'" --form "appFile=@app-release.apk"

The above script finds the APK file after it has been built on Codemagic’s remote machine using the bash command `cd ./build/app/outputs/apk/release` and uploads the APK file as packets to LambdaTest.

- `$LAMBDATEST`: holds my credentials to log in to the server
- `--form "name='lambda1'"`: the display name for the upload APK
- `--form "appFile=@app-release.apk"`: holds the APK file for upload

For this script to work, you will need two credentials: your username and access token. You can find them in the `App Automation` section of your dashboard in LambdaTest.

![](https://paper-attachments.dropbox.com/s_ABDEC71411CB38C7426C7AF547234560ABBACB1338C0393CA42CA7D5B87B81D5_1659064574411_Screenshot+2022-07-29+at+04.13.15.png)

To use these credentials, you need to convert them into base64 in the format `Username:AccessToken`. You can convert these words using your dev tool in the browser or use this [tool](https://mixedanalytics.com/knowledge-base/api-connector-encode-credentials-to-base-64/) to make the conversions easily.

Once you have converted your credentials, you can encrypt them as environmental variables and inject them into the cURL (client URL).

You can use the provided tool on Codemagic to perform the encryption. To read more about environmental variables, check out the [documentation](https://docs.codemagic.io/variables/environment-variable-groups/).

![](https://paper-attachments.dropbox.com/s_ABDEC71411CB38C7426C7AF547234560ABBACB1338C0393CA42CA7D5B87B81D5_1659066531085_Screenshot+2022-07-29+at+04.45.43.png)

Once a new build is triggered, your app should be uploaded to LambdaTest.

![](https://paper-attachments.dropbox.com/s_ABDEC71411CB38C7426C7AF547234560ABBACB1338C0393CA42CA7D5B87B81D5_1659073131703_Screenshot+2022-07-29+at+06.28.25.png)

## Testing our Flutter application on real devices

In this section, we will look at how to install our Flutter app on a device and how it works manually.

![](https://paper-attachments.dropbox.com/s_ABDEC71411CB38C7426C7AF547234560ABBACB1338C0393CA42CA7D5B87B81D5_1659074357083_Screenshot+2022-07-29+at+06.57.44.png)

Based on the image above, our app has been uploaded successfully on LambdaTest. To start manual testing, you need to hit the start button to install the app.

Once the application is installed, you can test your app on different devices with various screen sizes. You can also integrate Slack and other communication channels for your team.

Here is a video that shows how it works:

//screen record here [Screen Recording 2022-07-29 at 07.18.46.mov](https://www.dropbox.com/aa/view_file?file_id=id:pvCrckYDq0YAAAAAAAAAjw&role=personal)

## Conclusion

In this article, we have learned about the basics of LambdaTest and its features and uses. We also learned about software testing and its two main categories, namely, manual and automation testing. We then described how to publish our application to LambdaTest to use the real-time testing environment. For further reading and resources, check the resources section.

## Resources

- [GitHub](https://github.com/maxiggle/Flutter-LambdaTest.git)
- [LambdaTest integration](https://docs.codemagic.io/integrations/lambdatest-integration/)
- [Read more about LambdaTest](https://thenewstack.io/how-lambdatest-automates-cross-browser-testing-from-the-cloud/)
