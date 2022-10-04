---
title: "Diving Into World Construction Kit"
date: "2011-11-02"
categories:
- "site-updates"
tags:
- "actionscript"
- "box2d"
- "wck"
aliases:
- "/blog/diving-into-world-construction-kit/"
blachnietWordPressExport: true
---

In the game that I’ve been working on recently I’ve been using this amazing little library called World Construction Kit (WCK). This framework has saved me tons of time and made setting up Box2D worlds and bodies really easy. Emanuele Feronato wrote up a nice getting started tutorial and there are 2 very useful pages on the GitHub Wiki:

- [Box2d Flash Alchemy Port + World Construction Kit](http://www.emanueleferonato.com/2010/09/07/box2d-flash-alchemy-port-world-construction-kit/) –Feronato
- [World Construction Kit](https://github.com/jesses/wck/wiki/world-construction-kit) –github Wiki
- [Box2D Flash Alchemy Port](https://github.com/jesses/wck/wiki/box2d-flash-alchemy-port) –github Wiki This is great material for getting started. This post is going to assume you have read these and have managed to get a sample up and running. Make sure you check out the demo folder in the source. This has all the source for the demo seen on the [WCK homepage](http://www.sideroller.com/wck/). Below are a few notes that would have made my life easier when working with WCK. Hopefully they will be helpful to you.

## Initialization and Destruction

The BodyShape class indirectly derives from Entity, which provides constructor and destructor functionality for stage instances based on when they are added or removed from the stage. When the instance is added to the stage, create()is called and when the instance is removed from the stage destroy() is called. This is important because BodyShape overrides create and calls createBody() as part of its creation process. When you extend the BodyShape class and need to modify the underlying b2Body object, you will need to make sure that you do not try to access the b2Body before it is created. You do this by overriding the createBody() method, and doing your object specific initialization after calling super.createBody();

```
override public function createBody():void 
{
    super.createBody(); 
    b2body.SetUserData( 
        new BodyData(BodyData.WALL, this));
}
```

## Initialization Order

The order in which objects are initialized on stage is not guaranteed. The Entity class provides functions to allow you to make sure an object you are dependent on is properly created before you access it. Every Entity has an ensureCreate() and ensureDestroy(). These functions will call create() if it has not already been created, or destroy() if it has not already been destroyed.

## Event Listeners

Entity provides a function, listenWhileVisible(), which is an alternative way of adding event listeners. Any events added with this function will be automatically removed when the object's destroy function is called. You can call stopListening() if you need to remove the listener before then. You don't have to use this approach, but it does make life a little easier when you don't have to worry about remembering to remove event listeners.

## Frame Code

If you have already looked at the demo source in the WCK library, you may have noticed that there is frame code defining some of the bodies. If not, open up demo.fla and take a look at BodyShapeStuff\\PolyCustom. The body for this is being defined on the Layer 2 frame code. You are not required to define custom bodies on frame code, but it is an option you have. If the only reason you would need a class file is so that you could define the custom body, I would just put it on the frame code. If you are uncomfortable with this, don't do it.

## Scripts

The script directory in the WCK source has some very useful JSFL scripts for use in Flash Pro. Open the directory and copy the files to your Flash pro user data directory (on Windows 7 it is C:\\Users\[USERNAME\]\\AppData\\Local\\Adobe\[FLASH VERSION\]\\en\_US\\Configuration\\Commands\\WCK). Then when you open Flash Pro and click on the Commands menu item, you will be able to execute these scripts. Their function is pretty self explanatory, so try each of them out.

## Use V2

I know this was already mentioned in the github documentation, but it is very important. If you are used to the [BorisTheBrave port](http://www.box2dflash.org/) or any other version of the Box2D library, then you are used to creating b2Vec2s. You need to break this habit when working with WCK and the Box2D Flash Alchemy Port. **Only use V2.** See the github wiki page [here](https://github.com/jesses/wck/wiki/box2d-flash-alchemy-port) if you want a better explanation as to why you should do this.
