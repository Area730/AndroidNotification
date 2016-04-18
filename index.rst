.. Android Plugin documentation master file, created by
   sphinx-quickstart on Tue Apr 12 02:04:32 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Admob Plugin
===================================

------
Import
------

* Download this plugin_ 
* Watch this_ tutorial: 
.. _plugin: https://www.assetstore.unity3d.com/en/#!/content/57268
.. _this: https://www.youtube.com/watch?v=S7F9enxcvTE

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://www.youtube.com/embed/S7F9enxcvTE" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe><br/>
    </div>

| 

* Or follow these steps: 


1. Drag Admob prefab from *Area730/Admob/Prefabs* into your scene. You need to add this prefab to your game only once as **AdmobManager** is a singleton. 


.. image:: _static/1.png
    :align: center

2. Set Admob unit ids and you are almost done 

.. image:: _static/2.png
    :align: center

3. To show banner  call *AdmobManager.Instance.RequestBanner(AdPosition position)* where position is **enum** and can be::

    AdPosition.Top
    AdPosition.Bottom
    AdPosition.TopLeft
    AdPosition.TopRight
    AdPosition.BottomLeft
    AdPosition.BottomRight

You can also specify the size of the banner as second parameter. The default size is **320x50**.
To show interstitial you have first to load it by calling **AdmobManager.Instance.RequestInterstitial()**. After the interstitial is loaded, the **OnInterstitialLoaded()** callbacks will be invoked. Admob prefab comes with default attached callback functions (see *Area730/Admob/Scripts/AdmobController.cs*). You can also add your own if you want. 

.. image:: _static/3.png
    :align: center

After the interstitial is loaded, you can show the ad by calling **AdmobManager.Instance.ShowInterstitial()**.  

-----------------
Building projects
-----------------

After configuring your mediation you need to build your game to mobile devices. Sometimes it might cause some errors this section helps you to solve it

-----------------
IOS common errors
-----------------

1. When you use old version of Unity(4.6 and lower) the included GoogleMobileAds.framework_ in folder *Assets/Plugins/IOS* will not be automatically linked to your XCode project. To solve this problem copy GoogleMobileAds.framework_ to the root of your exported project. 

.. _GoogleMobileAds.framework: https://developers.google.com/admob/ios/download

.. image:: _static/framework.png
    :align: center

2. After that you need to config your **build settings**  

| 
 
    2.1 Disable bit code 

    .. image:: _static/bitcode.png
        :align: center

| 

    2.2 Enable Modules (C and Objective-C) 
    
    .. image:: _static/modules.png
        :align: center


3. If you use xCode 7.2  or higher you could get the following error because of incompatibility new xCode and Unity generated files. 

.. image:: _static/Noreturn.png
    :align: center


to solve this problem just delete circled ```NORETURN``` statment. 

---------------------
Android common errors
--------------------- 

When you trying to build your **apk** unity might throw build error. It might be caused of old Android API Level. The latest version of Admob library requires **API Level 23** . To download this packages go to you **Android SDK** (the path for it find here). 

.. image:: _static/androidSDKPath.png
    :align: center

Then install packages from SDK manager.


.. image:: _static/SDK.png
    :align: center

If you have another errors please contact us support@area730.com 
