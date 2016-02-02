---
layout: post
title:  "Render Testing and Debugging on Mobile Devices"
date:   2016-02-02 21:54:17
categories: Testing, Debugging
image:
image-subtitle:
---

*Debug all the things!*

I've been spending quite a lot of time render checking and debugging various features recently. Unit and acceptance tests are essential for assessing the functionality of your application, but rendering is an important thing to check too and needs robust methods of testing. This post is an overview of each tool I used, culminating with instructions on how to use Charles Web Proxy Debugger to set conditions.

#### Chrome Dev Tools

This doesn't really require any introduction but here's one anyway for completeness. Chome Dev Tools provide a basic overview of how your app looks on a variety of smartphones, tablets and desktop screen sizes and are a developer's best friend when it comes to diagnosing and fixing most CSS problems.

- Pros: Quick and easy to use, included with your browser already, easy to amend scripts using Fiddler/Charles web proxy, great for development.
- Cons: Not 100% accurate. You will need to test on actual devices or emulators if you want to be sure there are no problems before pushing to production.

#### Xcode Simulator

This can be used to emulate a number of different Apple devices on all sorts of versions of OSX. So far the rendering seems to be true to life and the device applications are updated in real time so you know you're using the most current versions (when Apple broke Safari the other day it also broke on the emulator, which was great fun). Since the simulator runs on your local machine you can just serve to localhost and go. It doesn't take *that* long to load, but it's not exactly the quickest either. You can use Charles or Fiddler to proxy your scripts as you would in Chrome, too.

- Pros: Mirrors real device well, runs on local machine
- Cons: Load time can get annoying if you are having to re-render something frequently, supports Apple devices only

#### Android Studio

I'd be lying if I said I knew a great deal about this tool. I was swiftly warned away from it by one of the developers in my team, who explained that the rendering is not necessarily representative of the device being emulated. The fact that the tool may not render things correctly made it unfit for purpose.

#### iPhone + Safari on Mac

Another Apple only solution (requires a Mac too). It allows you to debug your app on iPhone/iPad and comes with a Console and Network tab. It's pretty easy to set up. First switch the Web Inspector on on your device by navigating to `Settings > Safari > Advanced` and toggling Web Inspector to the On position. Load up the page you want to meddle with on your device. Then plug your device into the Mac using a lightning cable and open Safari. In the Safari menu go to `Develop > [Device Name] > [Name of page]`. You will then be presented with the markup for your page, CSS rules and basically all the things you'd expect from Chrome dev tools. Change the code and watch your page update in real time :)

- Pros: Uses actual devices, debugging possible, breadth of tools that you would normally associate with Chrome
- Cons: CSS rules tricky to edit, not as intuitive as Chrome, Apple devices only, Apple product overload

#### Any Device You Like + Charles proxy debugger

This setup will allow you to view a page on your device of choice and update its scripts. It means you can edit your test conditions (e.g. particular values for data being passed in) and view the outcome on any device you choose. Lovely stuff.

First, you need to [download an SSL certificate] for your particular device or OS. I used the [legacy certificate] even though I've got the most up-to-date version of Charles and it worked just fine. Make sure both your device and your computer are on the same Wifi network. Then go to your phone's Wifi settings and add a proxy. This is fairly straighforward on iPhone, but Android make it tricky by hiding things. If you're on Android, hold down on the name of the Wifi network to make a dialogue box pop up, then click Modify Network. Select 'Show advanced options' and the proxy settings should appear. Enter your computer's IP address and the port number Charles is using (more than likely to be 8888, but check in Charles at `Proxy > Proxy Settings > Proxies`, and yes that is the real path).

Now you should have Charles up and running. Check that a) the page loads on your device and b) when said page loads, the Structure panel in Charles picks up the activity. If either of these don't happen, check your device and computer are on the same Wifi network and that your proxy settings are correct.

##### *Charles Rewrite tool*

This is a very handy tool that lets you modify your requests and responses so you can double check everything is rendering correctly under different conditions. Open up `Tools > Rewrite`. Here you can add a new 'Set' for each set of conditions you want to apply. In the Locations menu add the URL(s) of the page you want to modify and then start adding the Rules in. In my case I substituted the existing value of PortalName in a tag object, to allow a test banner to render for closer inspection on Android.

#### Conclusion

For simple rendering checks whilst developing, Chrome dev tools are awesome and BrowserStack has a huge suite of browsers and devices. If you want to make sure things look pixel perfect on every device, load up your test environment on real devices where possible and on emulators of the devices you don't own. If you want to check elements that only appear when script modificaions are made, use Fiddler on Windows and Charles on Mac using the proxy instructions above.

[download an SSL certificate]: <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
[legacy certificate]: <http://www.charlesproxy.com/documentation/additional/legacy-ssl-proxying/>

Nat x
