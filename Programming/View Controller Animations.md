## UIViewController Animations

### View Controller Presentation Animations

#### Background

##### The UIKit Delegate
`UIKit` asks its `UIViewControllerTransitioningDelegate` whether to use a custom transition by calling the `animationController(forPresented:presenting:source)` delegate method. If this is not implemented (or returns `nil`) UIKit uses the default transition, otherwise if UIKit receives a `UIViewControllerAnimatedTransitioning` object it uses that object as the animation controller for the transition.

##### The animation controller
UIKit first asks the animation controller for the transition duration in seconds via `transitionDuration(using:)`, then calls `animateTransition(using:)` on it. 

The `transitionContext` passed to the animation controller gives you access to the parameters and view controllers of the transition. This includes the *to* and *from* view (destination and source), along with a *transition container* view responsible for handling the animations between them. Traditionally you want to animate out the old view and animate in the new. After the animations have completed you must call `completeTransition(true)` on the context.

#### Implementation
1. Your main view controller (or any other class you create for this purpose) must adopt the `UIViewControllerTransitioningDelegate`.

   1. You must create a custom animation controller and return this to UIKit in the `animationController(forPresented:presenting:source)` delegate method.
   2. Likewise, you must create and return a custom animation controller and return to UIKit in the `animationController(forDismissed:)` delegate method (this can be the same or a different controller as used previously).

   ```swift
   extension MyViewController: UIViewControllerTransitioningDelegate {
     let animationController = MyAnimationController() // in main imp because ext cannot contain stored prop
     
     func animationController(forPresented presented: UIViewController, presenting: UIViewController, source: UIViewController) -> UIViewControllerAnimatedTransitioning? {
       return animationController // alt create anim controller here; can create different controller depending on vc's being animated
     }
     
     func animationController(forDismissed dismissed: UIViewController) -> UIViewControllerAnimatedTransitioning? {
       return nil // nil uses default uikit anim
     }
   }
   ```

2. You must set the `transitioningDelegate` property of the view controller to be presented to the view controller adopting the transitioning delegate.

   ```swift
   let newViewController = storyboard!.instantiateViewController(withIdentifier: "newViewController")
   newViewController.transitioningDelegate = self // or any other class designated for this purpose
   present(newViewController, animated: true, completion: nil)
   ```

3. Create a custom class conforming to the `UIViewControllerAnimatedTransitioning` protocol.

   1. You must implement the required `transitionDuration(using:)` delegate method, which should return the total duration of your animation.
   2. You must implement the required `animateTransition(using:)` delegate method, which will contain all your animation code.

   ```swift
   class MyAnimationController: NSObject, UIViewControllerAnimatedTransitioning {
     let animationDuration = 1.0

     func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval {
       return animationDuration
     }
     
     func animateTransition(using transitionContext: UIViewControllerContextTransitioning) {
       let containerView = transitionContext.containerView
       let toView = transitionContext.view(forKey: .to)!
       
       toView.alpha = 0.0
       containerView.addSubview(toView)
       
       UIView.animate(withDuration: animationDuration, animations: {
         toView.alpha = 1.0
       }, completion: { _ in
       	transitionContext.completeTransition(true)
       }) // this will fade the new view controller in over the old
     }
   }
   ```



### Navigation Controller Animations

#### Background

Navigation controller animations works very similarly to presenting view controllers. There are a few caveats.

##### The navigation controller delegate

Instead of the transitioning delegate, we have to implement the `UINavigationControllerDelegate` protocol. This has one method, `navigationController(navigationController:animationControllerForOperation:fromViewController:toViewController)`, which will be called by the navigation controller requesting a custom animation controller. If this is not implemented (or returns `nil`) the custom animation will be used. The `operation` parameter specifies whether the transition is a `.push` or `.pop`.

#### Implementation

1. Your main view controller (or any class you designate for this purpose) must adopt the `UINavigationControllerDleegate`. You must create and return a custom animation controller in the `navigationController(navigationController:animationControllerForOperation:fromViewController:toViewController)` method. 

   ```swift
   extension MyViewController: UINavigationControllerDelegate {
     func navigationController(_ navigationController: UINavigationController, animationControllerFor operation: UINavigationControllerOperation, from fromVC: UIViewController, to toVC: UIViewController) -> UIViewControllerAnimatedTransitioning? {
      return transition 
     }
   }
   ```

2. You must set the `delegate` of the navigation controller early in the current view controller cycle, before pushing anything on the stack.

   ```swift
   override func viewDidLoad() {
   super.viewDidLoad()
   navigationController?.delegate = self
   }
   ```

3. Create a custom class conforming to the `UIViewControllerAnimatedTransitioning` protocol.

   1. You must implement the required `transitionDuration(using:)` delegate method, which should return the total duration of your animation.
   2. You must implement the required `animateTransition(using:)` delegate method, which will contain all your animation code.

   ```swift
   class MyAnimationController: NSObject, UIViewControllerAnimatedTransitioning {
     let animationDuration = 1.0

     func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval {
       return animationDuration
     }
     
     func animateTransition(using transitionContext: UIViewControllerContextTransitioning) {
       let containerView = transitionContext.containerView
       let toView = transitionContext.view(forKey: .to)!
       
       toView.alpha = 0.0
       containerView.addSubview(toView)
       
       UIView.animate(withDuration: animationDuration, animations: {
         toView.alpha = 1.0
       }, completion: { _ in
       	transitionContext.completeTransition(true)
       }) // this will fade the new view controller in over the old
     }
   }
   ```

   ​