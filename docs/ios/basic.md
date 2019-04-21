# iOS snippets

###  按下done/next/go...关闭键盘

```swift
@IBAction func textFieldDoneEditting(sender:UITextField) {
    //在storyboard中关联textField的"Did End On Exit"事件到这个方法
    sender.resignFirstResponder()
}
```
或者
```swift
func textFieldShouldReturn(textField: UITextField) -> Bool {
    //让该类遵守UITextFieldDelegate
    //将textField的delegate设置为该类
    //实现该方法
    textField.resignFirstResponder()
}
```



### 触摸背景关闭键盘

```swift
override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
    view.endEditing(true)
    super.touchesBegan(touches, withEvent: event)
}
```
或者
```swift
override func touchesBegan(touches: Set<UITouch>, withEvent event: UIEvent?) {
    textField.resignFirstResponder()
    super.touchesBegan(touches, withEvent: event)
}
```
或者
```swift
@IBAction func backgroudTab(sender: UIControl) {
    //将背景的Class变为UIControl，然后将背景的Touch Down事件连接到这个方法
    textFiled.resignFirstResponder()
}
```

### 按下Next将输入焦点转移到下一个输入框
和"关闭键盘"同理，在方法 ~~体内~~ 做相应的修改即可
```swift
func textFieldShouldReturn(textField: UITextField!) -> Bool {
    if textField == firstTextFiled {
        secondTextField.becomeFirstResponder()
    } else {
        textField.resignFirstResponder()
    }
}
```

### 避免键盘挡住视图
```swift
override func viewWillAppear(animated: Bool) {
    super.viewWillAppear(true)
    //添加通知观察者
    NSNotificationCenter.defaultCenter().addObserver(self, selector: "keyboardWillShow:", name: UIKeyboardWillShowNotification, object: nil)
    NSNotificationCenter.defaultCenter().addObserver(self, selector: "keyboardWillHide:", name: UIKeyboardWillHideNotification, object: nil)
}
override func viewWillDisappear(animated: Bool) {
    super.viewWillDisappear(true)
    //let app = UIApplication.sharedApplication()
    //NSNotificationCenter.defaultCenter().removeObserver(self, name: UIApplicationWillEnterForegroundNotification, object: app)
    NSNotificationCenter.defaultCenter().removeObserver(self, name: UIKeyboardWillShowNotification, object: nil)
    NSNotificationCenter.defaultCenter().removeObserver(self, name: UIKeyboardWillHideNotification, object: nil)
}
func keyboardWillShow(notification: NSNotification) {
    let info = notification.userInfo!
    let keyboardFrame: CGRect = (info[UIKeyboardFrameEndUserInfoKey] as! NSValue).CGRectValue()
    let duration: Double = info[UIKeyboardAnimationDurationUserInfoKey] as! Double

    //检查当前视图位置，防止一再向上移动视图
    if self.view.frame.origin.y >= 0 {
        UIView.animateWithDuration(duration, animations: { () -> Void in
            self.view.frame.origin.y -= keyboardFrame.size.height
        })
    }
}
func keyboardWillHide(notification: NSNotification) {
    let info = notification.userInfo!
    let keyboardFrame: CGRect = (info[UIKeyboardFrameEndUserInfoKey] as! NSValue).CGRectValue()
    let duration: Double = info[UIKeyboardAnimationDurationUserInfoKey] as! Double

    //检查当前视图位置，防止一再向下移动视图
    if self.view.frame.origin.y < 0 {
        UIView.animateWithDuration(duration, animations: { () -> Void in
            self.view.frame.origin.y += keyboardFrame.size.height
        })
    }
}
```
