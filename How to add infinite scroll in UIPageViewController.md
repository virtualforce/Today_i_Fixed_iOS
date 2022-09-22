# Problem
UIPageViewController doesn't provide infinite horizontal scrolling. Usually we use UIPageViewController for horizontally sliding views but user can only slide and go from 1st page to the last one and then user needs to swipe back one by one to reach at the first page i.e. user can't swipe from last page to the first. 
How to add infinite scroll on UIPageViewController so that user can swipe from last page to first and vice versa?


# Environment
N/A


# How you fix it
=> I will focus only on adding infinite scroll in UIPageViewController instead of complete setup of UIPageViewController (Initial setup is beyond the scope of this article).
Conform your PageViewController to **UIPageViewControllerDataSource** & **UIPageViewControllerDelegate** and provide defination of the below mentioned methods:
- viewControllerBefore
- viewControllerAfter
- didFinishAnimating
- presentationCount

Code snippets for all of the above methods are provided in below section.


# Solution
Here is the minor tweaks you need to do in your viewController to add infinite scrolling.
In below code snippets contentViewController --> instance of your pageView (View which will containt content of one swipe-able page)

# PageViewController
```
class PageViewController: UIViewController {

  // Below mentoioned are some class level veriables we will use in our code below
  var pages: [ContentViewController] = []
  var presentedPageIndex = 0
  var pagesData : [DataType] = [DataType]() //DataType --> any type e.g. string, dictionary etc.
  
  override func viewDidLoad()
    {
        super.viewDidLoad()
        
        setupPageView() // Initialise UIPageViewController instance, add all the contentViewController instances (pages) to **pages** array according to data, set first instance as starting point. 
    }
    
}
```

# PageViewController Extension 
Making extension is not necessary, you can confrom to UIPageViewControllerDataSource & UIPageViewControllerDelegate protocols in original scope instead of extension, I am making extension for code cleanliness.
```
extension PageViewController: UIPageViewControllerDataSource, UIPageViewControllerDelegate {
    
    func pageViewController(_ pageViewController: UIPageViewController, viewControllerBefore viewController: UIViewController) -> UIViewController? {
        
        guard let currentVC = viewController as? ContentViewController else {
            return nil
        }
        
        var index = currentVC.parentDataObjectIndex // parentDataObjectIndex --> Property defined in ContentViewController, assign dataObject index (index of data object in PageViewController) to this property while creating instance of ContentViewController to setup pages.
        
        if index == 0 {
            
            index = self.pagesData.count - 1
            
        } else {
            
            index -= 1
        }
        
        // Get previous contentViewController instance
        let previousVC = self.pages[index]
        
        return previousVC
    }
    
    func pageViewController(_ pageViewController: UIPageViewController, viewControllerAfter viewController: UIViewController) -> UIViewController? {
        
        guard let currentVC = viewController as? ContentViewController else {
            return nil
        }
        
        var index = currentVC.parentDataObjectIndex // parentDataObjectIndex --> Property defined in ContentViewController, assign dataObject index (index of data object in PageViewController) to this property while creating instance of ContentViewController to setup pages.
        
        if index >= self.pagesData.count - 1 {
            
            index = 0
            
        } else {
            
            index += 1
        }
        
        // Get next contentViewController instance
        let nextVC = self.pages[index]
        
        return nextVC
    }
    
    func pageViewController(_ pageViewController: UIPageViewController, didFinishAnimating: Bool, previousViewControllers: [UIViewController], transitionCompleted: Bool) {
        
        if transitionCompleted {
            if let currentPage = self.pageController?.viewControllers?.first as? ContentViewController {
            
                //Set currently presented page index (to handle corner cases when animation is completed after swiping multiple pages)
                self.presentedPageIndex = currentPage.parentDataObjectIndex // parentDataObjectIndex --> Property defined in ContentViewController, assign dataObject index (index of data object in PageViewController) to this property while creating instance of ContentViewController to setup pages.
            }
        }
    }
    
    func presentationCount(for pageViewController: UIPageViewController) -> Int {
        return self.pagesData.count
    }
    
}
```

