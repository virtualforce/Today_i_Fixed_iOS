# Problem
UIPageViewController doesn't provide auto-sliding functionality of pages (just like web carousel). Usually we use UIPageViewController for horizontally sliding views but user has to swipe pages to switch between pages. 
How to add auto-sliding (with infinite scroll) functionality in Pages of UIPageViewController.md


# Environment
N/A


# How you fix it
=> I will focus only on adding auto-sliding (with infinite scroll) functionality in Pages of UIPageViewController instead of complete setup of UIPageViewController (Initial setup is not part of the scope of this article).
Complete UIPageViewController setup and conform your PageViewController to **UIPageViewControllerDataSource** & **UIPageViewControllerDelegate**.
Then use **setViewControllers(viewControllers: direction: animated: completion:)** method of UIPageViewController to auto slide current page (after defined time interval).

Code snippet is provided below to auto-slide pages using above mentioned function.


# Solution
Here are some minor tweaks you need to do in your PageViewController to add auto-sliding pages functionality.
In below code snippets contentViewController --> instance of your pageView (View which will containt content of one swipe-able page)

# PageViewController
```
class PageViewController: UIViewController {

  // Below mentoioned are some class level veriables that are being used in the code below.
  var pages: [ContentViewController] = []
  var presentedPageIndex = 0
  var pagesData : [DataType] = [DataType]() //DataType --> any type e.g. string, dictionary etc.
  
  // Timer to auto-slide page
  var timer = Timer()
  let autoSlideInterval = 5.0
  
  override func viewDidLoad()
    {
        super.viewDidLoad()
        
        setupPageView() // Initialise UIPageViewController instance, add all the contentViewController instances (pages) to **pages** array according to data, set first instance as starting point. 
    }
    
  override func viewDidAppear(_ animated: Bool) 
    {
        super.viewDidAppear(animated)
        
        // initialise timer to auto-slide page
        self.timer = Timer.scheduledTimer(withTimeInterval: autoSlideInterval, repeats: true, block: { [weak self] _ in
            self?.managePagesPresentation(showNextPage: true)
        })
    }
  
  fileprivate func managePagesPresentation(showNextPage: Bool) {
        
        if showNextPage {
            
            if (self.presentedPageIndex + 1) < self.pagesData.count {
                self.presentedPageIndex += 1
            } else {
                self.presentedPageIndex = 0
            }
            
            self.pageController?.setViewControllers([self.pages[self.presentedPageIndex]], direction: .forward, animated: true, completion: nil)
            
        } else {
            
            if (self.presentedPageIndex - 1) >= 0 {
                self.presentedPageIndex -= 1
            } else {
                self.presentedPageIndex = (self.pagesData.count ) - 1
            }
            
            self.pageController?.setViewControllers([self.pages[self.presentedPageIndex]], direction: .reverse, animated: true, completion: nil)
        }
        
        // Below code will reset timer to present every page for 5 seconds.
        self.timer.invalidate()
        self.timer = Timer.scheduledTimer(withTimeInterval: autoSlideInterval, repeats: true, block: { [weak self] _ in
            self?.managePagesPresentation(showNextPage: true)
        })
    }
    
}
```

