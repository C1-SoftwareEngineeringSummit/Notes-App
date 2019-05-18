# Step By Step Instructions

## Initial Project and Storyboard Setup

* New Project -> Single View App -> Choose Swift
* When you create a new project, Xcode provides a default `ViewController.swift` file. Start by renaming `ViewController.swift` to `DetailViewController.swift`
  * Click on the `ViewController.swift` file from the **Identity Inspector (⌥⌘3)**, highlight `ViewController` in the source code, then right click -> Refactor -> Rename
* Head over to `Main.storyboard` and drag a `Table View Controller` object onto the Storyboard.
  * Click on the **Object Library (⇧⌘L)** and drag it onto `Main.storyboard`.
* Click on the new `Table View Controller` and in the **Attributes Inspector (⌥⌘4)** check the "Is Initial View Controller" option.
* Go to **Project Navigator** (⌘1) and add a new file to the project
  * Use the **Cocoa Touch Class** option then hit next
  * Update "Subclass of" to `UITableViewController`
  * Update "Class" to `MasterViewController`
  * Click Next and then Create
* Go back to `Main.storyboard` and click/select your `Table View Controller` scene
* Once selected, go to the **Identity Inspector (⌥⌘3)** and under Class make sure you select your newly created `MasterViewController`
  * ![MasterViewController Identity](/Assets/MarkdownAssets/MasterViewControllerIdentity.png)
  * **NOTE:** If you can't see your newly created class in the dropdown menu, then you probably havn't selected the Table View Controller correctly. Use the **Document Outline** view to properly select it.

## Configuring MasterViewController

### A few notes on `UITableViewController`

When you subclass `UITableViewController` it brings in a lot of pre-wired functionality. The `UITableViewController` subclass creates a `UITableView` and also sets itself as the delegate and data source of the table view. If you were to create a `UIViewController` that has a table view and does not inherit from `UITableViewController` you would have to set up the data source and the delegate of your table view yourself.

`UITableView` declares two protocols: `UITableViewDataSource` and `UITableViewDelegate`. The delegate protocol is used to inform the class about cells that have been selected and to provide an interface for modifying the table view behavior. The data source protocol is used by the table view to determine the content it needs to display. The delegate is there to respond to events from and guide the behavior of the table view, and the data source is there to provide data rather than control it.

In other words, `UITableViewController` comes with boilerplate code ready for you to use and also comes pre-wired to implement the `UITableViewDataSource` and `UITableViewDelegate` protocols.

### Creating the Model

* Create a new swift file which we will use to declare our `Note` model
  * File -> New -> File -> Swift File
  * Name the file `Note.swift`
  * Inside this file we will declare our model as a class called `Note` for storing information about Notes:
    ```swift
    class Note {
      var content: String?
      let dateCreated = Date()

      init(withContent content: String) {
        self.content = content
      }
    }
    ```
    > Notice the `init()` function has both an *argument label* (`withContent`) and a *parameter name* (`content`). The argument label is used when calling the function; each argument is written in the function call with its argument label before it. The parameter name is used in the implementation of the function.

* Now lets create our `notes` model inside of `MasterViewController` by creating an array of `Note`'s
  * Add the following code just below `class MasterViewController: UITableViewController {`:
    ```swift
    var notes = [Note]()
    ```
* Inside the `viewDidLoad()` lifecycle function add a few `Note`'s to the `notes` array:

  ```swift
  super.viewDidLoad()

  notes.append(Note(withContent: "Hello"))
  notes.append(Note(withContent: "World!"))
  ```

### Implement the `UITableView` protocols

* Find the `numberOfSections()` table view data source function and change the return value to: `1`

  ```swift
  return 1
  ```

  * Note that `numberOfSections()` is an **Optional** function
* Find the `tableView(numberOfRowsInSection)` table view data source function and change it to return the count of `notes`:

  ```swift
  return notes.count
  ```

  * Note that `tableView(numberOfRowsInSection)` is a **Required** function
* Uncomment the `tableView(cellForRowAt indexPath)` function
* In `Main.storyboard` find the "Table View Cell" in the Master View Controller scene, then inside the **Attributes Inspector (⌥⌘4)** give the table cell the reuse identifier: `noteCell`

  > **PRO TIP:** Use the **Document Outline** sidebar section to easily highlight different UI components

  ![Table View Cell](/Assets/MarkdownAssets/TableViewCell.png) ![Table View Cell Identifier](/Assets/MarkdownAssets/TableViewCellIdentifier.png)

* Back in `MasterViewController.swift` create a `String` constant for the identifier and use it with the `dequeuResuableCell()` function inside of the `tableView(cellForRowAt indexPath)` function
  * Under the `notes` array add the following:

    ```swift
    let noteCellIdentifier = "noteCell"
    ```

  * In the `cellForRowAt` function:

    ```swift
    let cell = tableView.dequeueReusableCell(withIdentifier: noteCellIdentifier, for: indexPath)
    ```

* Under "Configure the cell" comment inside the `tableView(cellForRowAt indexPath)` function, set the `textLabel` property of each table view cell equal to the `content` of your `notes` model using the appropriate index.
  * You can use `indexPath.row` to give you the row or index the cell is on

  ```swift
  cell.textLabel?.text = notes[indexPath.row].content
  return cell
  ```

* Build and run the app **(⌘R)**. You should see 2 cells displayed with whatever content you populated the Notes array with.

## App Navigation: Creating a Segue

A *segue* defines a transition between two view controllers in your app’s storyboard file. We will use segue's to transition from our `MasterViewController` to our `DetailViewController` whenever a user clicks on a tableView cell or wants to add a new note.

Segue's also provide a way to pass data from one controller to the next. In our case, we will use the segue to pass the appropriate `Note` from the `MasteViewController` to the `DetailViewController`.

* In `Main.storyboard` create a segue from the `MasterViewController` tableView cell to `DetailViewController`.
  > **PRO TIP:** Use the **Document Outline** sidebar section to easily highlight different UI components
  * Control + Drag from the `noteCell` component to the `DetailViewController` scene
  * Select `Show` in the popup to create a segue of type `Show`
  * Click on the new segue in the storyboard then on **Attributes Inspector**. Give it the Identifier name: `showNote`
* In `MasterViewController.swift` create a `String` constant to match the Identifier

  ```swift
  let showNoteSegue = "showNote"
  ```

* Uncomment the `prepare(for segue)` code towards the bottom of `MasterViewController.swift`
* Run the app to test out your segue functionality

## Configuring the DetailViewController

* In `Main.storyboard` add a **Text View** to the Detail View Controller scene
  * From the **Object Library (⇧⌘L)** drag and drop a **Text View** onto the scene and adjust it to fit properly. Align it to fit within the margins.
* Connect the **Text View** as an `IBOutlet` to the `DetailViewController`
  * Make sure you **Show the Assistant Editor** so you can see the Storyboard and `DetailViewController.swift` class side by side
    > **PRO TIP:** You can hold `Option + Shift` then click on a file to give you different presentation options
  * Create the outlet by control clicking and dragging from the **Text View** onto `DetailViewController.swift`
  * Make sure **Outlet** is selected in the popup and give it the name: `textView`
  * Once this is done, the `@IBOutlet` inside `DetailViewController.swift` should look something like this:
    ```swift
    @IBOutlet weak var textView: UITextView!
    ```
* Add a variable to `DetailViewController.swift` which will be used to hold your `Note`:

  ```swift
  var note: Note?
  ```

  * **NOTE:** Make sure the `var` is optional `?`

* Write a function called `configureView()` to set `textView.text` equal to the value of the note that was passed in from the segue:

  ```swift
  func configureView() {
    if let note = note {
      textView.text = note.content
    }
  }
  ```

* Finally, make sure to call your new `configureView()` function from the `viewDidLoad()` lifecycle function

## Passing Data From Master to Detail Using the Segue

* In `MasterViewController` we need to update the `func prepare(for segue)` function to properly send `Note`'s to the `DetailViewController` when we segue.
* Inside the `prepare(for segue)` function we need to do two things:
  * Grab a reference to the `DetailViewController` using `segue.destinationViewController`
  * Pass the selected `Note` to `DetailViewController`
* The `prepare(for segue)` function should look like the following:

  ```swift
  if segue.identifier == showNoteSegue {
    if let indexPath = tableView.indexPathForSelectedRow {
      let note = notes[indexPath.row]
      let detailViewController = segue.destination as! DetailViewController
      detailViewController.note = note
    }
  }
  ```

* Build and run the app and verify that you are passing data correctly to the `DetailViewController`

## Navigation: Back button

* In the storyboard, click the `Table View Controller` then in the Xcode dropdown menu's go to Editor -> Embed In -> Navigation Controller.
  > A navigation controller is a container view controller that manages one or more child view controllers in a navigation interface. They allow us to move between different view controller scenes by adding or popping view controllers from a stack.  This will natively add a back button for us.
* Click on the new `Navigation Controller` and in the **Attributes Inspector (⌥⌘4)** check the "Is Initial View Controller" option.

## Adding Notes To The Model

* In `MasterViewController.swift` find the `viewDidLoad()` method and uncomment the `navigationItem` line of code
* Change the code to use the `leftBarButtonItem` instead: `self.navigationItem.leftBarButtonItem = self.editButtonItem`
* Build and run the app. We should now have a "Edit" button on the left side of the navigation bar
* In `Main.storyboard` go to the **Object Library (⇧⌘L)** and find a **Bar Button Item**
* Drag a Bar Button Item onto the right side of the navigation bar of the Master View Controller scene
* In the **Attributes Inspector (⌥⌘4)** make the "System Item" equal to "Add"
  ![Table View Cell](/Assets/MarkdownAssets/BarButtonItem.png)

> When a user presses the + button we want the app to create a new Note and then navigate to the detail page for editing. To accomplish this we'll need to create a new segue.

* Create a new segue from the + bar button item to the `DetailViewController` scene
  > **PRO TIP:** Use the **Document Outline** sidebar section to easily highlight different UI components
  * Control Drag from the `+` button to the `DetailViewController` scene
  * Select `Show` in the popup to create a segue of type `Show`
  * Click on the new segue in the storyboard then on **Attributes Inspector (⌥⌘4)**. Give it the Identifier name: `showNewNote`
* In `MasterViewController.swift` create a `String` constant to match the Identifier
  * You should not have the following constants declared:
    ```swift
    let cellIdentifier = "noteCell"
    let showNoteSegue = "showNote"
    let showNewNoteSegue = "showNewNote"
    ```
* Update the `prepare(for segue)` function in `MasterViewController.swift` to handle two different segue's and also add a new `Note` to the model:

  ```swift
  if segue.identifier == showNoteSegue {
    if let indexPath = tableView.indexPathForSelectedRow {
        let note = notes[indexPath.row]
        let detailViewController = segue.destination as! DetailViewController
        detailViewController.note = note
    }
  } else if segue.identifier == showNewNoteSegue {
      // Insert a new Note to the model before transitioning to DetailViewController
      notes.insert(Note(withContent: "New note"), at: 0)
      let indexPath = IndexPath(row: 0, section: 0)
      // Insert a new row in our tableView
      tableView.insertRows(at: [indexPath], with: .automatic)
      let detailViewController = segue.destination as! DetailViewController
      detailViewController.note = notes[0]
  }
  ```

* Build and run your app to test the + (add note) functionality
* Note that on return to the Master View Controller the notes are not being updated...

## Updating the Notes Model On Return to Master

We want to be able to edit notes in `DetailViewController` and have the model in `MasterViewController` get updated. To accomplish this we will make use of the `viewWillDisappear()` controller lifecycle method. Whenever the `DetailViewController` is about to disappear from the screen we want to update the note to reflect what is currently on the `textView`.
> [Understand the View Controller Lifecycle](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/WorkWithViewControllers.html#//apple_ref/doc/uid/TP40015214-CH6-SW3)

Add the following function and code to `DetailViewController`:

```swift
override func viewWillDisappear(_ animated: Bool) {
  super.viewWillDisappear(animated)
  
  if let note = note {
    note.content = textView.text
  }
}
```

* Because we made our `Note` model a `class` instead of a `struct`, we are passing notes by reference
  * This allows us to modify our model from anywhere in our app as long as we have a reference
* Build and run the app to see if it's working as intended

## Refreshing the TableView

If you ran the app, you will have noticed that notes are still not being updated when we return to `MasterViewController`. This is because we still need to tell the `UITableView` to reload the data.

This time we'll take advantage of another view controller lifecycle method: `viewWillAppear()`. Add the following code to the `MasterViewController` class:

```swift
override func viewWillAppear(_ animated: Bool) {
  super.viewWillAppear(animated)

  tableView.reloadData()
}
```

## Deleting Notes From The Model

To delete notes from the model we will take advantage of some of the boilerplate code the comes with `UITableViewController`.

* In `MasterViewController.swift` find the `tableView(canEditRowAt IndexPath)` data source function and uncomment it.
* Find the `tableView(commit edityingStyle, forRowAt indexPath)` data source function and uncomment this one as well.
* Edit the `tableView(commit edityingStyle, forRowAt indexPath)` function to properly delete a `Note` from your model whenever it is called:

  ```swift
  if editingStyle == .delete {
    // Delete the row from the data source
    notes.remove(at: indexPath.row)
    tableView.deleteRows(at: [indexPath], with: .fade)
  }
  ```

* Build and run your app to verify you can delete cells

## References

* [Understand the View Controller Lifecycle](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/WorkWithViewControllers.html#//apple_ref/doc/uid/TP40015214-CH6-SW3)
* [Delegate Pattern](https://developer.apple.com/library/mac/#documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html)
* YouTube tutorial: [iOS Swift Basics Tutorial: Protocols and Delegates](https://www.youtube.com/watch?v=9LHDsSWc680)
* ["WEAK, STRONG, UNOWNED, OH MY!" - A GUIDE TO REFERENCES IN SWIFT](https://krakendev.io/blog/weak-and-unowned-references-in-swift)
