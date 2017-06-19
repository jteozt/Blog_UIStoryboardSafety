# UIStoryboard: Safer with Enums, Protocol Extensions and Generics
## Because String literals are so yucky.

### Medium post
[UIStoryboard: Safer with Enums, Protocol Extensions and Generics](https://medium.com/p/7aad3883b44d/)

### tl;dr
Turn this:

````
let name = "News"

let storyboard = UIStoryboard(name: name, bundle: nil)

let identifier = "ArticleViewController"

let viewController = storyboard.instantiateViewControllerWithIdentifier(identifier) as! ArticleViewController

viewController.printHeadline()
````

Into this:

````
let storyboard = UIStoryboard.storyboard(.News)

let viewController = storyboard.instantiateViewController(ArticleViewController.self)

viewController.printHeadline()
````

Alternative:

````
protocol StoryboardInstantiable { }
extension StoryboardInstantiable where Self: UIViewController {
  static var className: String {
    return String(describing: Self.self)
  }
init(from storyboard: UIStoryboard) {
  let controller = storyboard.instantiateViewController(withIdentifier: Self.className)
  guard let viewController = controller as? Self else {
    fatalError("Could not instantiate controller from class name '\(Self.className)'")
  }
  self = viewController
  }
}
extension UIViewController: StoryboardInstantiable { }
````

To use: 

````
let storyboard = UIStoryboard.storyboard(.news)
let viewController = ArticleViewController(from: storyboard)
````

Credit: Dinesh Raja Soundrapandian
