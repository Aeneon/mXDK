# moXen Developer Kit

**Hello Friends,**

if you own a Mac running macOS Mojave or newer, you've surely noticed, there's a Function called &quot;Time-shifting Wallpapers&quot;. Wouldn't it be nice, if iOS does have such a Feature with Performance in Mind and being Battery Friendly&nbsp;? [moXen](https://macthemes.me/package.php?id=com.macthemes.moxen) make your Dreams come true and it ships with three great Backgrounds&nbsp;: [Mojave](https://macthemes.me/package.php?id=com.macthemes.moxen.mojave), [Catalina](https://macthemes.me/package.php?id=com.macthemes.moxen.catalina) and [Big Sur](https://macthemes.me/package.php?id=com.macthemes.moxen.bigsur)&nbsp;…

<p align="center"><img src="https://macthemes.me/developer/mXDK_Header.png" /></p>

Now it's Time to create our own &quot;Time-shifting Wallpapers&quot;, so we can use it on every iPhone, iPod Touch or iPad running iOS 10 and above.


## Table of Contents

1.	[Introduction](#1-introduction)
2.	[Requirements](#2-requirements)
3.	[Creating the Folder](#3-creating-the-folder)
4.	[Building &quot;Contents.json&quot;](#4-building-"contents.json")
5.	[Wallpaper-Time](#5-wallpaper-time)
6.	[On-Device-Test](#6-on-device-test)
7.	[Creating a Debian-Package](#7-creating-a-debian-package)
8.	[Last Words](#8-last-words)
9.	[Greetings](#9-greetings)


## 1. Introduction

This Developer Kit allows you to create your own &quot;Time-shifting Wallpapers&quot;&nbsp;(&nbsp;we call them &quot;Wallpaper-Sets&quot;&nbsp;), based on the two Algorithms moXen supports : Solar-based&nbsp;(&nbsp;used in any macOS Wallpaper&nbsp;) and Time-based&nbsp;(&nbsp;used in custom Wallpapers; much easier to use&nbsp;). Booth of them are nearly equal and to being Battery-friendly, we did it without using a Service for getting the Solar Position for your Location.

If we're working with a Path, you'll see an opened Folder-Emoji, following by the Path&nbsp;: `📂 Step 4 ● Time-based`.


## 2. Requirements

If you would like to use this Developer Kit, the first Application you need is a Text Editor&nbsp;(&nbsp;we're using [Coda](https://panic.com/coda/) on macOS, but TextEdit does also the Job&nbsp;). The second App you need is an Image Editor, like [Adobe Photoshop](https://www.adobe.com/de/products/photoshop.html) or [Pixelmator](https://itunes.apple.com/de/app/pixelmator/id407963104?mt=12) from the Mac App Store.

If you don't know what a JSON-File is, please read the [Wikipedia-Article](https://en.wikipedia.org/wiki/JSON), and have a Look for the Syntax and Character Encoding.

The last Thing you need is **Patience**.

## 3. Creating the Folder

This is fairly simple, create a Folder with the Name of your Wallpaper. Please **don't** use Spaces, these are not supported. The Wallpaper-Set &quot;Cherry Blossom&quot; uses an Underscore, instead Spaces.

**Example&nbsp;:** `📂 Step 3 ● moXen ● Assets ● Cherry_Blossom`.

## 4. Building &quot;Contents.json&quot;

Now we can create a File, named `Contents.json`, in our created Folder. The first Thing we need to specify is a Title, so open the File in your Text Editor, copy the following Lines and set a Title&nbsp;(&nbsp;in Difference to the Folder, are Whitespaces allowed&nbsp;)&nbsp;:

```json
{
	"Title": "macOS Mojave"
}
```

------

Next, we have to choose one of the two Algorithms, which are supported by moXen&nbsp;:

- **Solar-based**

This Algorithm is used in all Wallpapers shipped with macOS, it's a little bit complicated, since the Wallpapers consider the Position of the Sun. Currently the Attribute `altitude` won't be used in moXen, so we're using only `azimuth`. Booth Values are using Degrees, so `altitude` allows Values between -179.99 and 179.99 and `azimuth` Values between 0 and 359.99.

If you want to learn more about this Algorithm, have a Look at Marcin Czachurski's Article &quot;[macOS Mojave dynamic wallpaper](https://itnext.io/macos-mojave-dynamic-wallpaper-fd26b0698223)&quot; on ITNEXT.

**Example&nbsp;:** `📂 Step 4 ● Solar-based ● moXen ● Assets ● Mojave ● Contents.json`

As you can see, you have to specify `solar` as Type.

```json
{
	"Type": "solar",
	"Mojave":
	[
		{ "id": "Mojave_01", "altitude": -52.7531813787993, "azimuth": 2.1750965538675473 }
	]
}
```

If you want to use this Algorithm, Casio Computer offers a [Website](https://keisan.casio.com/exec/system/1224682277) to calculate the Position of the Sun, but consider to think about the Differences for Summer and Winter.

**Pro Tip&nbsp;:** If you want to go more into the Deep, you can use [`wallpapper -e`](https://github.com/mczachurski/wallpapper) to extract the Metadata from a downloaded Wallpaper to get the Values for `altitude` and `azimuth`. 

- **Time-based**

This Algorithm is much easier to use. Instead of dealing with Altitude and Azimuth, we're using the Time of Day in Percent. We know a Day has a Total of 86.400 Seconds and if we want to change the Wallpaper at 09:30 AM, we need to do the following Math&nbsp;: (9&nbsp;* 60&nbsp;* 60)&nbsp;+ (30&nbsp;* 60)&nbsp;= 34,200. Divided by 86,400, we get a Value of &quot;0.39583˜3&quot;. Optionally you can also add Seconds, but it's not necessary.

Let us express this in a Formulae&nbsp;:
$$
((Hours*60*60)+(Minutes*60)+Seconds)/86400=X
$$
**Example&nbsp;:** `📂 Step 4 ● Time-based ● moXen ● Assets ● Cherry_Blossom ● Contents.json`

As you can see, you have to specify `time` as Type.

```json
{
	"Type": "time",
	"Cherry_Blossom":
	[
		{ "id": "Cherry_Blossom_01", "time": 0 }
	]
}
```

------

If you'd chosen one of the Algorithms, copy the Code in your `Contents.json`. And replace `altitude`, `azimuth` and respectively `time` with your Values. This is only for one Image, you can&nbsp;(&nbsp;theoretically&nbsp;) add up to 86,400 Images. To prevent Performance Issues, we've set a Maximum of 1,440 Images. Every Minute, one Image. Not really useful, but possible. Just add more Images, until your Array of Images looks like the following&nbsp;: 

```json
{
	"Cherry_Blossom":
	[
		{ "id": "Cherry_Blossom_01", "time": 0 },
		{ "id": "Cherry_Blossom_02", "time": 0.25 },
		{ "id": "Cherry_Blossom_03", "time": 0.333333333333333 },
		{ "id": "Cherry_Blossom_04", "time": 0.416666666666667 },
		{ "id": "Cherry_Blossom_05", "time": 0.708333333333333 },
		{ "id": "Cherry_Blossom_06", "time": 0.791666666666667 },
		{ "id": "Cherry_Blossom_07", "time": 0.916666666666667 }
	]
}
```

The Value `id` holds the Filename. As the same as for the Folder, please don't use Spaces. moXen requires an Index at the End of every Image, as you can see above. Just add a `_01`&nbsp;… `_n` to your Filename. Additionally, after every Line a Comma is needed, except for the last Line in the Array. This is also necessary for the Values `Title` and `Type`.

With all the Values, your `Contents.json` should look like this&nbsp;:

```json
{
	"Title": "Cherry Blossom",
	"Type": "time",
	"Cherry_Blossom":
	[
		{ "id": "Cherry_Blossom_01", "time": 0 },
		{ "id": "Cherry_Blossom_02", "time": 0.25 },
		{ "id": "Cherry_Blossom_03", "time": 0.333333333333333 },
		{ "id": "Cherry_Blossom_04", "time": 0.416666666666667 },
		{ "id": "Cherry_Blossom_05", "time": 0.708333333333333 },
		{ "id": "Cherry_Blossom_06", "time": 0.791666666666667 },
		{ "id": "Cherry_Blossom_07", "time": 0.916666666666667 }
	]
}
```

To test your newly created JSON-File, just copy the whole Code into the [JSON-Validator](https://www.freeformatter.com/json-validator.html) provided by FreeFormatter.com.

If everything is fine, the Validator should display the following Messages&nbsp;:

<p align="center"><img src="https://macthemes.me/developer/mXDK_Contents_Validator.png" /></p>

Save your Changes and we can head over to the NextSTEP.

## 5. Wallpaper-Time

To offer the best Performance and decrease the Size of the Wallpaper-Sets, we've decided, that a Wallpaper-Set can only hold Images for one Display Size. It doesn't help the User, if he needs to download much more Images, which he can't use, because his Device supports only one Display Size.

**Example&nbsp;:** You don't need Images with 2340 &nbsp;× 1080 Pixels, if your Device has a Display Size of 1334&nbsp;× 750 Pixels.

Let's have a Look on the Display Sizes, which every Device uses&nbsp;:

|               iDevice                | Display Size in Pixels |
| :----------------------------------: | :--------------------: |
|              iPhone 4S               |       960 × 640        |
| iPhone 5 \| 5S \| 5C \| SE 1st Gen.  |       1136 × 640       |
|     iPod Touch 5th Gen - 7th Gen     |       1136 × 640       |
|  iPhone 6 \| 7 \| 8 \| SE 2nd Gen.   |       1334 × 750       |
|           iPhone XR \| 11            |       1792 × 828       |
|        iPhone 6+ \| 7+ \| 8+         |      1920 × 1080       |
|        iPad 3 \| 4 \| 5 \| 6         |      2048 × 1536       |
|           iPad Air 1 \| 2            |      2048 × 1536       |
|      iPad mini 2 \| 3 \| 4 \| 5      |      2048 × 1536       |
|          iPad Pro 9.7-inch           |      2048 × 1536       |
|             iPad 7 \| 8              |      2160 × 1620       |
|              iPad Air 3              |      2224 × 1668       |
|          iPad Pro 10.5-inch          |      2224 × 1668       |
|            iPhone 12 mini            |      2340 × 1080       |
|              iPad Air 4              |      2360 × 1640       |
| iPad Pro 11-inch 1st Gen \| 2nd Gen  |      2388 × 1668       |
|       iPhone X \| XS \| 11 Pro       |      2436 × 1125       |
|         iPhone 12 \| 12 Pro          |      2532 × 1170       |
|     iPhone XS Max \| 11 Pro Max      |      2688 × 1242       |
| iPad Pro 12.9-inch 1st Gen - 4th Gen |      2732 × 2048       |
|          iPhone 12 Pro Max           |      2778 × 1242       |

As you can see, a lot of Devices share their Display Size with other Devices, so you can create a Wallpaper-Set for an iPhone 6 and it runs on an iPhone 6S, 7, 8 and on the 1st generation iPhone SE too.

How do you create Images&nbsp;? It's very simple&nbsp;:

1. Pick a few Images you want to use for your Wallpaper-Set. You can find a lot of Time-shifting Wallpapers at [Dynamic Wallpaper Club](https://dynamicwallpaper.club/), depending on your desired Screen Size you’ll need to do some digging around the Gallery, but you have a wide Array of Choices ranging from real-world Photography to Artist Renditions of Video Games, Cityscapes, Animals and more.

   **Be careful&nbsp;:** A lot of Wallpapers are re-uploaded by the Users and they're sometimes looking pixelated and blurred.

   Alternatively you can visit [Dynwalls](http://dynwalls.com/), but they're offering only six Wallpapers at the Moment.

2. Open your downloaded Image in Preview.

3. Now select every Image and Drag&nbsp;&amp; Drop them into your created Folder, where also the File `Contents.json` is located.

   <p align="center"><img src="https://macthemes.me/developer/mXDK_Wallpaper_Preview.png" /></p>

   You should now have a lot of Images in your Folder.

4. Rename the Images with the Name you've chosen as `id` in `Contents.json`.

5. Start your Image Editor, open all Images, resize and crop them, so we can use it for our desired Display Size.

   **Pro Tip&nbsp;:** If you're using Photoshop, you can use Batch Processing, but also macOS provides a very good App for this Task&nbsp;: Automator.

6. Save them as PNG.

7. You should now have a Folder with a `Contents.json` and a few Images in it.

   **Example 1&nbsp;:** `📂 Step 5 ● Solar-based ● moXen ● Assets ● Mojave`

   **Example 2&nbsp;:** `📂 Step 5 ● Time-based ● moXen ● Assets ● Cherry_Blossom`

## 6. On-Device-Test

It's Time to verify, that our Wallpaper-Set is working. Transfer `Contents.json` and the Images in the Folder `Developer` on your iPhone, iPad or iPod Touch&nbsp;:

- Homescreen&nbsp;: `/var/mobile/Library/Widgets/Homescreen/moXen/Developer`
- Lockscreen&nbsp;: `/var/mobile/Library/Widgets/Lockscreen/moXen/Developer`

Now head over in Preferences, select &quot;Xen HTML&quot; and choose between &quot;Lockscreen&quot; and &quot;Homescreen&quot;. Select &quot;Background Widgets&quot; and &quot;moXen&quot;, tap on the Settings-Icon, choose &quot;Developer Options&quot; and switch on &quot;Developer Mode&quot;.

If everything is working, you should now see your running Wallpaper-Set&nbsp;…

<p align="center"><img src="https://macthemes.me/developer/mXDK_OnDevice_Preview.png" /></p>

The Developer Mode has a Function to run through a Day in 24 Minutes, which means you can see how moXen cycles through the Images of your Wallpaper-Set during a Day&nbsp;…

If everything is working, disable &quot;Developer Mode&quot; and remove all Files from the Folder `Developer`.

## 7. Creating a Debian-Package

During this Step, we'll create a Debian-Package, so you can easily share your Wallpaper-Set with the Community.

**Pro Tip&nbsp;:** If you don't want to publish your Creation and keep it private, place your Folder, containing `Contents.json` and the Images in the Folders `Assets`&nbsp;:

- Homescreen&nbsp;: `/var/mobile/Library/Widgets/Homescreen/moXen/Assets`
- Lockscreen&nbsp;: `/var/mobile/Library/Widgets/Lockscreen/moXen/Assets`

Now open a Terminal-App right on your iPhone, iPad or iPod Touch, for Example [Prompt 2](https://apps.apple.com/app/id917437289) or [NewTerm 2](cydia://package/ws.hbang.newterm2) and run the Command Line Utility `moxen` with the Argument `-u`.

What does this Command do&nbsp;?

Let's have a Look at `moxen --help`, which shows the &quot;Help&nbsp;&amp; Usage&quot;&nbsp;:

```
NAME
  moXen -- Simply powerful. On your Mac and also on iOS.

VERSION
  2103 ( March 8, 2021 )

SYNOPSIS
  moxen [ -u | -l ]

DESCRIPTION
  This Command Line Utility allows Developers to update the Backgrounds-Array and list available Wallpaper-Sets.

  The Options are as follows:
    -u    Updates Backgrounds-Array in config.plist; --update is for the non-lazy Ones.

    -l    Lists Wallpaper-Sets in Assets; --list is for the non-lazy Ones.

    -h    Displays Help & Usage; --help is for the non-lazy Ones.

NOTES
  moxen comes with no Warranty, neither express nor implied. If it blows up your Widgets, eats your Children, or causes you bodily Harm, all you'll get is a little Pity and maybe an Apology.
```

As you can see&nbsp;: The Command `moxen -u` updates the &quot;Background&quot;-Array in the File `config.plist`.

Now head over in Preferences, select &quot;Xen HTML&quot; and choose between &quot;Lockscreen&quot; and &quot;Homescreen&quot;. Select &quot;Background Widgets&quot; and &quot;moXen&quot;, tap on the Settings-Icon, look for your Wallpaper-Set in &quot;Background&quot; and select it.

------

To create a Debian-Package, we need the Command Line Tools for Xcode, [Homebrew](https://brew.sh/) and the Packages `dpkg` and `md5sha1sum`. The following Steps will guide you through the Installation of all these Requirements&nbsp;:

1. Open &quot;Terminal&quot; on your Mac. Homebrew requires the Command Line Tools for Xcode to be installed, so we'll install them first. Copy the following Line into Terminal and hit Enter&nbsp;…

   ```bash
   xcode-select --install
   ```
   
2. When the Installation has finished, we're able to install Homebrew. Just paste the following Line and hit Enter.
   
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
   
   
   This Step can take up a few Minutes.
   
3. Now we have to install `dpkg` and `md5sha1sum`, you can do it by pasting the Line in the Terminal&nbsp;(&nbsp;and hitting Enter again&nbsp;).

   ```bash
   brew install dpkg md5sha1sum
   ```

Now we've installed all Requirements and we're able to build a Debian-Package from the Command Line. This can be very difficult and to simplify this Step, we built an Automator-Service.

1. Copy the File `Services_CreateDebianPackage.workflow` located in `📂 Step 7 ● Tools`.

2. Go to the Finder, hit the Menu &quot;Go&quot; and navigate down to &quot;Go to Folder&quot;.

   **Pro Tip&nbsp;:** Hit **Command+Shift+G** from the Desktop or a Finder Window.

3. Paste the following Path into the new appearing Sheet&nbsp;: `~/Library/Services`.

4. Paste the previously copied File.

If you're now opening the Context Menu on a Folder, you should see a new Entry, named &quot;Build Debian-Package&nbsp;…&quot;.

------

Let's get started with the major Part of this Step&nbsp;: Every Debian-Package contains a File `control`, with Informations about the Package. Navigate to `📂 Step 7` and you'll see a lot of Folders. In Step 5 &quot;[Wallpaper-Time](#5-wallpaper-time)&quot;, you've chosen a Display Size for you Wallpaper-Set, now pick the corresponding Debian-Folder and copy the Folder of your Wallpaper-Set into the following Folders&nbsp;:

 `📂 Step 7 ● * ● var ● mobile ● Library ● Widgets ● Homescreen ● moXen ● Assets` or

 `📂 Step 7 ● * ● var ● mobile ● Library ● Widgets ● Lockscreen ● moXen ● Assets`

Replace the `*` with a Debian-Folder from the following List&nbsp;…

|               iDevice                | Display Size in Pixels |         Debian-Folder         |
| :----------------------------------: | :--------------------: | :---------------------------: |
|              iPhone 4S               |       960 × 640        |   com.xyz. …**-2x-iphone**    |
| iPhone 5 \| 5S \| 5C \| SE 1st Gen.  |       1136 × 640       | com.xyz. …**-568h-2x-iphone** |
|     iPod Touch 5th Gen - 7th Gen     |       1136 × 640       | com.xyz. …**-568h-2x-iphone** |
|  iPhone 6 \| 7 \| 8 \| SE 2nd Gen.   |       1334 × 750       | com.xyz. …**-667h-2x-iphone** |
|           iPhone XR \| 11            |       1792 × 828       | com.xyz. …**-896h-2x-iphone** |
|        iPhone 6+ \| 7+ \| 8+         |      1920 × 1080       |   com.xyz. …**-3x-iphone**    |
|        iPad 3 \| 4 \| 5 \| 6         |      2048 × 1536       |    com.xyz. …**-2x-ipad**     |
|           iPad Air 1 \| 2            |      2048 × 1536       |    com.xyz. …**-2x-ipad**     |
|      iPad mini 2 \| 3 \| 4 \| 5      |      2048 × 1536       |    com.xyz. …**-2x-ipad**     |
|          iPad Pro 9.7-inch           |      2048 × 1536       |    com.xyz. …**-2x-ipad**     |
|             iPad 7 \| 8              |      2160 × 1620       | com.xyz. …**-1080h-2x-ipad**  |
|              iPad Air 3              |      2224 × 1668       | com.xyz. …**-1112h-2x-ipad**  |
|          iPad Pro 10.5-inch          |      2224 × 1668       | com.xyz. …**-1112h-2x-ipad**  |
|            iPhone 12 mini            |      2340 x 1080       | com.xyz. …**-780h-3x-iphone** |
|              iPad Air 4              |      2360 × 1640       | com.xyz. …**-1180h-2x-ipad**  |
| iPad Pro 11-inch 1st Gen \| 2nd Gen  |      2388 × 1668       | com.xyz. …**-1194h-2x-ipad**  |
|       iPhone X \| XS \| 11 Pro       |      2436 x 1125       | com.xyz. …**-812h-3x-iphone** |
|         iPhone 12 \| 12 Pro          |      2532 × 1170       | com.xyz. …**-844h-3x-iphone** |
|     iPhone XS Max \| 11 Pro Max      |      2688 × 1242       | com.xyz. …**-896h-3x-iphone** |
| iPad Pro 12.9-inch 1st Gen - 4th Gen |      2732 × 2048       | com.xyz. …**-1366h-2x-ipad**  |
|          iPhone 12 Pro Max           |      2778 × 1242       | com.xyz. …**-926h-3x-iphone** |

Did you noticed the first Part of the Name of every Debian-Folder&nbsp;?

**Example&nbsp;:** `com.xyz.moxen.abc`

Looks a Bit generic and that's true. Here you can create your own Package-Identifier, by replacing `xyz` with your Company Name&nbsp;(&nbsp;or Nickname, we're using `macthemes`&nbsp;) and `abc` with the Name of your Wallpaper-Set&nbsp;(&nbsp;for Example&nbsp;: `cherryblossom`&nbsp;). The whole Name of this Folder will be your Package-Identifier. Please **don't** remove the Part `moxen` in the Name, because it prevent &quot;Chaos&quot;, especially if you're planing to release more than one Wallpaper-Set with your Company Name&nbsp;(&nbsp;or Nickname&nbsp;).

**Pro Tip&nbsp;:** If you're using the same Wallpaper-Set for Homescreen and Lockscreen, you can reduce the Size of the Debian-Package by using Symbolic Links.

Now let's edit the File `control`. Navigate to `📂 Step 7 ● * ● DEBIAN` and open `control` in your Text Editor. The File already contain a few Informations and Example Data about the Package&nbsp;…

```
Package: com.xyz.moxen.abc-667h-2x-iphone
Version: 2009
Section: XenHTML ( Assets )
Name: macOS Mojave ( iPhone 4.7" )
Author: Aeneon <me@macthemes.me>
Maintainer: Aeneon <me@macthemes.me>
Icon: file:///usr/share/macthemes/sections/xenhtml@2x.png
Pre-Depends: cy+model.iphone, gsc.main-screen-height (= 1334)
Depends: com.macthemes.moxen (>= 2007)
Architecture: iphoneos-arm
Description: macOS Mojave meets "moXen" …
Depiction: http://macthemes.me/package.php?id=com.macthemes.moxen.mojave
```

Let's explain these Fields. If you want more Information about the Fields, have a Look at &quot;[Control files and their fields](https://www.debian.org/doc/debian-policy/ch-controlfields.html#binary-package-control-files-debian-control)&quot;&nbsp;…

|    Value     | Description                                                  |
| :----------: | :----------------------------------------------------------- |
|   Package    | The Identifier of your Package, should have the same Name like your Debian-Folder. |
|   Version    | Yes, here you can set a Version.                             |
|   Section    | Allows to Group the Wallpaper-Sets in a Package Manager.     |
|     Name     | The Name of your Wallpaper-Set, with an Addition for the Display Size. |
|    Author    | Your Name ( and optionally your E-Mail-Address ).            |
|  Maintainer  | The Name of the Repo-Maintainer ( and optionally your E-Mail-Address ). |
|     Icon     | Changes the Icon of the Package in a Package Manager.        |
| Pre-Depends  | Contains a List of Pre-Depends, we're using it to detect the Device Model and Display Size. |
|   Depends    | Contains a List of Dependencies, we're using this to install moXen, if it's not already installed. |
| Architecture | The Architecture of your Package.                            |
| Description  | A short Description of your Wallpaper-Set.                   |
|  Depiction   | Here you can set a Link to a Webpage, which contains more Information about your Package. |

The Fields `Section`, `Icon`, `Pre-Depends`, `Depends` and `Architecture` should **not** be changed, as it contains Values for Core-Functionality of this Package.

Did we forget anything&nbsp;?

No&nbsp;?

Then we can create the Debian-Package from our Debian-Folder. Open the Context Menu and select the Service &quot;Build Debian-Package&nbsp;…&quot;. A few Moments later, a new Debian-Package appears in Finder.

## 8. Last Words

That's it&nbsp;! We hope you use the Developer Kit to create some amazing new Wallpaper-Sets for moXen. If you would like to contribute to this Project, let us know.

If you respect our hard Work, you can help us with a small Donation via [PayPal.me](https://www.paypal.com/donate/?hosted_button_id=F5EPEFSLDBHPQ)&nbsp;😃&nbsp;…


## 9. Greetings

We would like to thank *Matt Clarke*&nbsp;(&nbsp;aka *_Matchstic*&nbsp;) for his amazing Work he did and the countless Hours he spend with the Development of Xen HTML.

Thank you.
