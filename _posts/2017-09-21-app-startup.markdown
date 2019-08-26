---
title:  "iOS App Start-up: avoiding shut-down by watchdog if app launch is slow"
categories: swift
---
If an app launches too slowly then the iOS watchdog process will shut it down. For example, trying to start up Core Data on the main thread can cause this. But what if there's nothing useful the app can do until the start-up process is completed and Core Data is available?

One solution is to display an initial splash screen on start-up. Set up an observer to monitor for when Core Data is ready. Then start up Core Data on a background thread.

For example:

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    NotificationCenter.default.addObserver(self, selector: #selector(WaitScreenViewController.coreDataReadyAction), name: NSNotification.Name(rawValue: "coreDataReady"), object: nil)

    let psc = NSPersistentStoreCoordinator(managedObjectModel: self.managedObjectModel)
    do {
        DispatchQueue.global(qos: DispatchQoS.QoSClass.default).async {    
            let urls = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
            let docURL = urls[urls.endIndex-1]
            let url = docURL.appendingPathComponent("file_name.sqlite")
            do {
                try psc.addPersistentStore(ofType: NSSQLiteStoreType, configurationName: nil, at: url, options: nil)
            } catch {
                fatalError("Error migrating store: \(error)")
            }
           NotificationCenter.default.post(name: Notification.Name(rawValue: "coreDataReady"), object: self)
        }
    }
}

func coreDataReadyAction() {
    CoreDataAPI.sharedInstance.setup()
    NotificationCenter.default.removeObserver(self)
    performSegue(withIdentifier: "leaveWaitScreen", sender: self)
}
```
