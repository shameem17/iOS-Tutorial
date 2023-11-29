# App Without Using Storyboard

Though there are some advantages of using storyboard like storyboard allows us to visualize the control follow of view controller across the app but there are some disadvantages of storyboard. The number of disadvantages are large in number than the it's benefits. However, you can find the disadvantages of storyboard [here](https://www.quora.com/Why-do-some-iOS-developers-not-use-storyboards).

---

Here is the process of building ios app without using a stroyboard.

1. Create a new project or open an existing project
2. Delete the Main.storyboard file
3. Delete the SceneDelegate file
4. Remove the UISceneSession Lifecycle section from AppDelegate class. Mainly remove the following codes

```swift 

   // MARK: UISceneSession Lifecycle

    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        // Called when a new scene session is being created.
        // Use this method to select a configuration to create the new scene with.
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }

    func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // Called when the user discards a scene session.
        // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
        // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
    }


```

5. Inside AppDelegate class define a global variable window of type UIWindow
6. Inside didFinishLaunchingWithOptions function add the following code

```swift

     window = UIWindow(frame: UIScreen.main.bounds)
     window?.rootViewController = ViewController() //add your first viewcontroller here instade of ViewController()
     window?.makeKeyAndVisible()


```

7. Now go to the info section of the app. You can find it by Clicking to app name at the top left side of xcode.
8. Remove the Application Scene Manifest and Main Storyboard file base name

![info](https://github.com/shameem17/iOS-Tutorial/assets/53037559/c08ca8ab-30e8-4374-97db-c9196859879d)

9. Now all done. Design your screen programmatically to show content.


