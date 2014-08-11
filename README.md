
As3ToHaxe command-line utility 
================

As3ToHaxe is a simple As3 to Haxe 3 transcoder, which takes an AS3 directory and converts all .as (AS3 classes) to .hx (Haxe classes).  An additional installer shell script and execution script have been added to speed up installation and execution of the As3ToHaxe tool. 

This version of As3ToHaxe is a Haxe 3 port of the [original Haxe 2 version](http://pastebin.com/s0VccheL) was authored by Tarwin Stroh-Spijer.

* [As3ToHaxe Haxe 2 on Haxe.org](http://haxe.org/doc/flash/usingas3classes/tarwins_as3_to_haxe_conversion_script)
* [Haxe official website](http://haxe.org/)

Installation
------------

After cloning the repo, execute the `install.sh` script, which will compile neko version of As3ToHaxe, follow the instructions to install.

Once install simply run:
```
as3ToHaxe <source_as3_folder> <export_haxe_folder>
```

For additional help run:
```
as3ToHaxe -h
```

Todo
------------
###Getters/Setters

AS3:

    function get x():Number {
        return _x;
    }
    function set x(value:Number):Number {
        _x = value;
    } 

Should be replaced to this:

    public var x(getX, setY):Float
    function getX ():Float
    {
        return _x;
    }

    function setX (value:Float):Float
    {
        return _x = value;
    }

###cast

AS3:

    sprite as Sprite
    
Should be replaced to this:

    cast(sprite, Sprite)

Where sprite is instance, Sprite is a class.

###for each

AS3 For each loops

    for each(var i:Item in items)
    {
    }
    
Should be ported to:

    for (i in items)
    {
    }

###for

AS3 Loops like this:

    for (var i:int=0; i<10; ++i)
    {
    }
    
Should be ported to:

    for (i in 0...10)
    {
    }

###is

AS3 "is" like 

    (1 is Int)
    
Should be changed to 

    Std.is(1, Int)

###Haxe doesn't support lower case imports like this:

    import flash.utils.getQualifiedClassName;
    import flash.utils.getTimer;
    import flash.utils.setTimeout;
    
So convertor should remove imports and put in code those strings, like in this example:

AS3:

    var a:int = getTimer();
    
Should be changed to:

    var a:int = flash.utils.getTimer();

###Convert Vector to Array

Replace Vector with Array

    Vector.<Sprite>
    
To

    Array<Sprite>

###Change Vector arrays initializations to Array
AS3 code like this:

    var a:Vector.<MyClass> = new <MyClass> [new MyClass()];
    
To

    var a:Array<MyClass> = new Array<MyClass>();
    a.push(new MyClass());

###Replace those dynamic params

    function test (...params):void {
    
    }

Change to:

    function test (params:Array<Dynamic>) {
    
    }
    
###Final vars should be replaced with this tag 

    @:final
     
###Events meta tags like this
 
AS3:

    [Event(name="test",type="Foo")]

To:

    @:meta(Event(name="test",type="Foo"))
    
And should be checked other things and changed like in this article.
http://www.nme.io/developer/guides/actionscript-developers/
