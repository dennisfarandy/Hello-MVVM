
#HELLO MVVM
####@dennisfarandy

---

#Resource
###github.com/dennisfarandy/hello-MVVM

---
![](Resource/pic-room-messy.JPG)

---
![](Resource/pic-room-messy.JPG)

- Persistance Manager
- API
- Business Logic
- Localization
- State
- Autolayout
- Analytics

---
> Massive View Controller
-- iOS Engineer

^ and will show to BUGS (next slide)

---
# [FIT]BUGS

^ why : Because each time we adding code controller we also at potential bug. and if one viewcontroller has so many responsibility, and it getting worse if we put more state, because the side effect, and also we cannot test it

---

#[fit]coupled, hard to manage
#[fit]many responsibility, hard to test


---

>  a class or module should have only a single responsibility
-- Robert Cecil Martin

---

![](Resource/pic-room-clean.jpg)

---

![](Resource/pic-room-clean.jpg)
#HOW?

---
###DESIGN PATTERN
#### [FIT]Datasource
#### [FIT]Standard Composition
#### [FIT]Smarter Views
#### [FIT]Coordinator
# *MVVM*

---

#*MVVM*
###better design code, more manageable, less bug?
###
^before that we see that, lets see the mvc first

---
#MVC

###Controller (own) > Model
###Controller (own) > View

---
#BUT FOR REAL
###ViewController (own) > Model

---
#MVVM

###View (own) > ViewModel
###ViewModel (own) > Model

---
#Model 

```swift
struct User {
    let id:Int
    let name:String
    let jobTitle:String
}

struct Post {
    let id:Int
    let author:User
    let caption:String
    let taggedUser:[User]
}
```

^ thanks to swift we can create immutable

---
##VIEWCONTROLLER
```swift
class PostDetailViewController: UIViewController {
    var post:Post
    
    @IBOutlet weak var labelAuthor:UILabel!
    @IBOutlet weak var labelCaption:UILabel!
    @IBOutlet weak var labeltaggedUser:UILabel!
    @IBOutlet weak var labelPostDate:UILabel!
}
```

---
##VIEWCONTROLLER
```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        
        labelAuthor.text = post.author.name + " - " + post.author.jobTitle        
        labelCaption.text = post.caption
        
        var taggedEmployeeText:String?
        if let firsUserName = post.taggedUser.first?.name where post.taggedUser.count >= 3 {
            taggedEmployeeText = firsUserName + "with \(post.taggedUser.count-1) other users"
        } else {
            taggedEmployeeText = post.taggedUser.map { $0.name }.reduce("") { (last, element) -> String in
                return last + " & " + element
            }
        }
        labeltaggedUser.text = taggedEmployeeText
        
        let formater = NSDateFormatter()
        formater.dateFormat = "dd MMM yyyy"        
        labelPostDate.text = formater.stringFromDate(post.createdAt)
    }
```
---
#lets use ViewModels

---
#VIEWMODEL
```swift
class PostDetailViewModel {
    let post:Post
    
    private(set) var authorText:String?
    private(set) var captionText:String?
    private(set) var taggedUserText:String?
    private(set) var dateText:String?
    
    init(post:Post) {
        self.post = post
        bindModel()
    }
}
```

---
##ViewModel
```swift
extension PostDetailViewModel {       
    func bindModel() {
        authorText = post.author.name + " - " + post.author.jobTitle        
        captionText = post.caption
        
        var taggedEmployeeText:String?
        if let firsUserName = post.taggedUser.first?.name where post.taggedUser.count >= 3 {
            taggedEmployeeText = firsUserName + "with \(post.taggedUser.count-1) other users"
        } else {
            taggedEmployeeText = post.taggedUser.map { $0.name }.reduce("") { (last, element) -> String in
                return last + " & " + element
            }
        }
        taggedUserText = taggedEmployeeText
        
        let formater = NSDateFormatter()
        formater.dateFormat = "dd MMM yyyy"
        
        dateText = formater.stringFromDate(post.createdAt)
    }
}
```

---

#ViewController

```swift
   	let viewModel: PostDetailViewModel

	override func viewDidLoad() {
        super.viewDidLoad()
        bindViewModel()
   	}
    
    func bindViewModel() {
        labelAuthor.text = viewModel.authorText
        labelCaption.text = viewModel.captionText
        labeltaggedUser.text = viewModel.taggedUserText
        labelPostDate.text = viewModel.dateText
    }

```

---
#[fit]~~coupled, hard to manage~~
###*View* --own-- *ViewModel* --own-- *Model*
^ if you need to change the view position only changed in view
^ if you need to change the model with same view, then you need changed only in model and viewModel layer
^ reduce conflict if you work together, because the responsibility also separate between logic

---
#[fit]~~many responsibility, hard to test~~
###[fit]ViewController - *View, Layout, animation*
###[fit]ViewModel - *Network, Logic, persistence*


---

#NEXT?
###Handle mutable model with binding *(ReactiveCocoa, ReactiveKit, RxSwift)*


---
#REFERENCES

- https://www.objc.io/issues/1-view-controllers/lighter-view-controllers/
- http://khanlou.com/2014/09/8-patterns-to-help-you-destroy-massive-view-controller/
- http://khanlou.com/2015/01/the-coordinator/
- http://www.sprynthesis.com
- http://www.teehanlax.com/blog/model-view-viewmodel-for-ios/

---
#QUESTION
####@dennisfarandy

---
#THANKS
####@dennisfarandy