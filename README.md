# QLImageSizeIssue

## [Origin Ref](https://github.com/Nyx0uf/qlImageSize/issues/45#issuecomment-610852166)

## Reason:
#### This issue is actually caused by the macOS 10.15.4 won't call the function [QuickLookGeneratorPluginFactory](https://github.com/Nyx0uf/qlImageSize/blob/ae8d124dcf86cca584ed148c2c1269d442378e63/qlgenerator/src/main.m#L203) of custom plugin which is not same as the macOS 10.14.6;

## Detail:
#### How I debug with this project by the steps:

1. ``` git clone https://github.com/Nyx0uf/qlImageSize.git ```
2. ``` cd qlImageSize ```
3. ``` tar xJvf images-samples.txz ``` (decompress sample image in the project)
4. ``` open qlImageSize.xcodeproj ```
5. Xcode top menu: Product -> Scheme -> Edit Scheme
    * Info -> Executable -> Other -> cmd + shift + g -> /usr/bin/qlmanage
     ![1](https://user-images.githubusercontent.com/8273263/78765190-3b0ff180-79ba-11ea-8463-fd1d0aef9702.jpg)
    * Arguments:
    ![3](https://user-images.githubusercontent.com/8273263/78765303-685c9f80-79ba-11ea-8169-e06e4e9e39b5.png)
        [A] copy from 
        ![3](https://user-images.githubusercontent.com/8273263/78765409-8924f500-79ba-11ea-81a0-f8a4cc184029.jpg)
        [B] copy result from terminal: ``` realpath Users/benjamin/btsync/projects/qlImageSize/images-samples/webp.webp ```
        [C] ``` webp ```
    * close
6. add breakpoint in ``` QuickLookGeneratorPluginFactory ``` of main.m
![4](https://user-images.githubusercontent.com/8273263/78765494-a6f25a00-79ba-11ea-8204-faa884061e24.png)
7. Xcode top menu: Product - Run, **break point is hit with webp on macOS 10.15.4** (if not, try to restart Xcode, god bless Apple)
8. but if replace webp to png, **the break point is not hit with png on macOS 10.15.4.**:
    * replace [B] with result from terminal ```realpath Users/benjamin/btsync/projects/qlImageSize/images-samples/png.png ```
    * replace [C] with ``` png ```
    ![5](https://user-images.githubusercontent.com/8273263/78765582-c7baaf80-79ba-11ea-8635-bfb1cc05c65e.png)
    * after run project, **the break point is not hit with png on macOS 10.15.4.**
9. but repeating the same steps wrote above on **macOS 10.14.6**, I find the break point **is hit with both webp and png on macOS 10.14.6.** It works well.


## Conclusion until now:
#### [As sabrex wrote](https://github.com/Nyx0uf/qlImageSize/issues/45#issuecomment-547971897) the macOS 10.15.4 is not calling the custom quick look plugin with the common image file type such as png / jpg.

|  |macOS 10.14.6 |macOS 10.15.4  |
| --- | --- | --- |
|webp |✅  |✅  |
|png  |✅  |❌  |


## PS:
#### [As hazarek wrote](https://github.com/Nyx0uf/qlImageSize/issues/45#issuecomment-549993287) I love this handy feature too. Sadly I can't fix it so far. If anyone have any thought, please leave a comment. 😭😭😭
