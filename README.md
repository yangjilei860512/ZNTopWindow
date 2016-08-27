# ZNTopWindow
在当前Window存在多个UITableView和UIScrollView时,系统的点击状态栏置顶的功能将无法使用,因为系统不知道应该返回哪一个控件的顶部,该代码实现了当前控制器存在多个含有UIScrollView控件属性的控制器时,点击当前的状态栏,将当前含有UIScrollView控件的控制器置顶的功能.在README文件中,复制粘贴到一个ZNTopWindow的类文件中,并在didFinishLaunchingWithOptions方法中调用ZNTopWindow.show()方法就好了,顶部区域的颜色自己可以设置.




import UIKit
var topWindow: ZNTopWindow?
class ZNTopWindow: UIWindow {
    
    class func show() {
        
        topWindow = ZNTopWindow(frame: CGRectMake(0, 0, UIScreen.mainScreen().bounds.size.width, 20))
        topWindow!.backgroundColor = UIColor.redColor()
        topWindow!.rootViewController = UIViewController()
        topWindow!.windowLevel = UIWindowLevelAlert
        topWindow!.hidden = false
    }
    override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
        // 点击窗口
        // 让tableView滚到最顶部，获取tableView
        // 遍历主窗口中的所有子控件，判断是否是tableView
        let keyWindow = UIApplication.sharedApplication().keyWindow;
        
        self.findTableView(keyWindow!)
    }
    func findTableView(view: UIView) {
        
        for childView in view.subviews {
            
            self.findTableView(childView)
            
            if childView.isKindOfClass(UITableView.classForCoder()) {
                
                let tableView = childView as? UITableView
                
                tableView?.setContentOffset(CGPointMake(0, -(tableView?.contentInset.top)!), animated: true)
            }
        }
    }
}
