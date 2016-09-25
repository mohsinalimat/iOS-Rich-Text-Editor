RichTextEditor-iOS [![Version](http://cocoapod-badges.herokuapp.com/v/iOS-Rich-Text-Editor/badge.png)](http://cocoadocs.org/docsets/iOS-Rich-Text-Editor)
==================

**NOTE: This repo is in desperate need of some help. Code needs cleaned up, and there are some issues with the RTE depending on setting itself to its own delegate in order to make things work properly. I haven't gotten the chance to do this myself. Please help!**

-Forked by Deadpikle for additional fixes and features. Readme updates TODO. There have been many enhancements and improvements. Please bug me for an updated README if I forget, which I probably will. The code is by no means perfectly clean, but it does function! Be wary of using the stock undo/redo with bulleted lists —- it often fails. Also, I have no idea how CocoaPods updates with forks work, so if someone needs me to do that, please point me in the right direction…-

### TODO
- [ ] Drop WEPopover dependency and always present other screens modally.
- [ ] Change style to have starting brackets on the same line as if statements & function calls (etc.)
- [ ] Add directions to the README.md about how to present the modal screens as popovers (see [here](https://rbnsn.me/ios-8-popover-presentations) and [here](https://richardallen.me/2014/11/28/popovers.html)).
- [ ] Make use of iOS NSTextStorage
- [ ] Port fixes and changes from OS X ([see here](https://github.com/Deadpikle/macOS-Rich-Text-Editor))
- [ ] Convert this README file to use ## instead of ----- for h1/2/3/4/5 syntax

## RichTextEditor for iPhone &amp; iPad

**Requirements**: iOS 8.0 or higher 

Features:
- Bold
- Italic
- Underline
- StrikeThrough
- Bulleted lists
- Font
- Font size
- Text background color
- Text foregroud color
- Text alignment
- Paragraph Indent/Outdent

![alt tag](https://raw.github.com/aryaxt/iOS-Rich-Text-Editor/master/ipadScreenShot.png)

![alt tag](https://raw.github.com/aryaxt/iOS-Rich-Text-Editor/master/iphoneScreenshot.png)

### Delegate Warning!

In order to intercept delegate messages, this class uses WZProtocolInterceptor. If you call `self.textView.delegate`, you will get the WZProtocolInterceptor object, *not* the original delegate that you set earlier with `self.textView.delegate = ...`! If you need to get the delegate that you set, call `self.textView.delegate.receiver`. 


Installing
-------------------------
Add the sources to your project, then add `-fno-objc-arc` to the compiler flags for `WETouchableView.m`, `WEPopoverController.m`, and `WEPopoverContainerView.m`. (Yes, that library really needs to be updated.)

Custom Font Size Selection
-------------------------
Font size selection can be customized by implementing the following data source method

```objective-c
- (NSArray *)fontSizeSelectionForRichTextEditor:(RichTextEditor *)richTextEditor
{
	// pass an array of NSNumbers
	return @[@5, @10, @20, @30];
}
```

Custom Font Family Selection
-------------------------
Font family selection can be customized by implementing the following data source method

```objective-c
- (NSArray *)fontFamilySelectionForRichTextEditor:(RichTextEditor *)richTextEditor
{
	// pass an array of Strings
  // Can be taken from [UIFont familyNames]
	return @[@"Helvetica", @"Arial", @"Marion", @"Papyrus"];
}
```

Presentation Style
-------------------------
You can switch between popover, or modal (presenting font-picker, font-size-picker, color-picker dialogs) by implementing the following data source method
```objective-c
- (RichTextEditorToolbarPresentationStyle)presentarionStyleForRichTextEditor:(RichTextEditor *)richTextEditor
{
  // RichTextEditorToolbarPresentationStyleModal Or RichTextEditorToolbarPresentationStylePopover
	return RichTextEditorToolbarPresentationStyleModal;
}
```

Modal Presentation Style
-------------------------
When presentarionStyleForRichTextEditor is a modal, modal-transition-style & modal-presentation-style can be configured
```objective-c
- (UIModalPresentationStyle)modalPresentationStyleForRichTextEditor:(RichTextEditor *)richTextEditor
{
	return UIModalPresentationFormSheet;
}

- (UIModalTransitionStyle)modalTransitionStyleForRichTextEditor:(RichTextEditor *)richTextEditor
{
	return UIModalTransitionStyleFlipHorizontal;
}
```

Customizing Features
-------------------------
Features can be turned on/off by iplementing the following data source method
```objective-c
- (RichTextEditorFeature)featuresEnabledForRichTextEditor:(RichTextEditor *)richTextEditor
{
   return RichTextEditorFeatureFont | 
          RichTextEditorFeatureFontSize |
          RichTextEditorFeatureBold |
          RichTextEditorFeatureParagraphIndentation;
}
```

Enable/Disable RichText Toolbar
-------------------------
You can hide the rich text toolbar by implementing the following method. This method gets called everytime textView becomes first responder.
This can be usefull when you don't want the toolbar, instead you want to use the basic features (bold, italic, underline, strikeThrough), thoguht the UIMeMenuController
```objective-c
- (BOOL)shouldDisplayToolbarForRichTextEditor:(RichTextEditor *)richTextEditor
{
   return YES;
} 
```

Enable/Disable UIMenuController Options
-------------------------
On default the UIMenuController options (bold, italic, underline, strikeThrough) are turned off. You can implement the following method if you want these features to be available through the UIMenuController along with copy/paste/selectAll etc.
```objective-c
- (BOOL)shouldDisplayRichTextOptionsInMenuControllerForRichTextrEditor:(RichTextEditor *)richTextEdiotor
{
   return YES;
} 
```

Credits
-------------------------
iPhone popover by werner77
https://github.com/werner77/WEPopover
