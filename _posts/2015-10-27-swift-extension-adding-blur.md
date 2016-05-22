---
layout: post
title:  "Swift extension: Adding blurred background to iOS apps"
date:   2015-10-27 19:06:23 +0600
categories: iOS
permalink: /:slug
comments: true
---
Extensions allow to include key functionalities such as providing new initializers, adding new computer properties and instance methods, defining new nested types, etc. to not only custom classes, structures, and enumerations, but also private ones to which source code you do not have access. However, you cannot override any existing functionality which for the most cases, is not what you aim for.

In this post, I have taken this opportunity to explore how we can blur any screen's background, technically speaking blurring any view by conveniently using Swift extension. By the way, I like Swift becaues it's powerful, modern and noise-free, and compared to another language known for simplicity, Python, it's nearly 10 times faster in many cases. I wish Swift was designed for server-side workloads as well. However, in comparison, iOS API needs to be made far modern, otherwise coding in Swift would have been more fun.

The following is the basic user interface that I have built. There is a UISegmentedControl to let user choose the blur type and an image that is going to give us the blurred vision.

![Swift extension: Adding blurred background to iOS apps]({{ site.url }}/assets/swift-extension-blurred-back.png)

The extension class below is quite self-explanatory. This class applies to all UIViewControllers without any explicit binding. 

{% highlight swift %}
import UIKit

extension UIViewController
{
    // Any Int value you want the views to be identified
    enum blurViewEnum: Int { case ID = -999 }

    func BlurExtraLight(imgView: UIImageView)
    {
        Blur(imgView, effectType: UIBlurEffectStyle.ExtraLight)
    }

    func BlurLight(imgView: UIImageView)
    {
        Blur(imgView, effectType: UIBlurEffectStyle.Light)
    }

    func BlurDark(imgView: UIImageView)
    {
        Blur(imgView, effectType: UIBlurEffectStyle.Dark)
    }

    func BlurReset()
    {
        for v in view.subviews
        {
            if (v.tag == blurViewEnum.ID.rawValue)
            {
                v.removeFromSuperview()
            }
        }
    }

    func Blur(imgView: UIImageView, effectType: UIBlurEffectStyle)
    {
        if !UIAccessibilityIsReduceTransparencyEnabled()
        {
            let effect = UIBlurEffect(style: effectType)
            let effectView = UIVisualEffectView(effect: effect)

            // Identify these created views with a tag for future reset
            effectView.tag = blurViewEnum.ID.rawValue
            
            // if you want the whole view to be blurred,
            // use view.bounds instead
            effectView.frame = imgView.bounds   
                                                
            view.addSubview(effectView)
        }
        else
        {
            self.view.backgroundColor = UIColor.blackColor()
        }
    }
}
{% endhighlight %}

I have introduced 4 instance methods for UIViewControllers which can be invoked from any UIViewController as long as this extension class is present in the solution.

You may download/fork from [github](https://github.com/tsaqib/samples/tree/master/backgroundExtensionDemo).