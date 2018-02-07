---
title: Cocoa NSColor 安全色 / RGB 对照表
date: 2018-02-07 11:38:59:02
categories: 编程之道
tags: 
- 苹果
- IOS
- MacOS
- 颜色
- 安全色
- Cocoa
- AppKit
- UIKit
- XCode
summary: 苹果在 Cocoa 里提供了若干安全色方便开发人员，统一界面风格，可惜这些颜色只给了值参考，在此整理一下。
---

苹果在 Cocoa 里提供了若干安全色方便开发人员，统一界面风格，可惜这些颜色只给了值参考，在此整理一下。

**Xcode Version**： `9.2 (9C40b)`
**Swift Version**：`4`

| 名称 | RGB | 颜色 |
| --- | --- | --- |
| `NSColor.black` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.brown` | `#996633` | <div style="background-color:#996633;width:20px;height:20px"></div> |
| `NSColor.cyan` | `#00ffff` | <div style="background-color:#00ffff;width:20px;height:20px"></div> |
| `NSColor.darkGray` | `#555555` | <div style="background-color:#555555;width:20px;height:20px"></div> |
| `NSColor.gray` | `#808080` | <div style="background-color:#808080;width:20px;height:20px"></div> |
| `NSColor.green` | `#00ff00` | <div style="background-color:#00ff00;width:20px;height:20px"></div> |
| `NSColor.labelColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.lightGray` | `#aaaaaa` | <div style="background-color:#aaaaaa;width:20px;height:20px"></div> |
| `NSColor.magenta` | `#ff00ff` | <div style="background-color:#ff00ff;width:20px;height:20px"></div> |
| `NSColor.orange` | `#ff7f00` | <div style="background-color:#ff7f00;width:20px;height:20px"></div> |
| `NSColor.purple` | `#7f007f` | <div style="background-color:#7f007f;width:20px;height:20px"></div> |
| `NSColor.quaternaryLabelColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.orange` | `#ff7f00` | <div style="background-color:#ff7f00;width:20px;height:20px"></div> |
| `NSColor.white` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.yellow` | `#ffff00` | <div style="background-color:#ffff00;width:20px;height:20px"></div> |
| `NSColor.alternateSelectedControlColor` | `#0069d9` | <div style="background-color:#0069d9;width:20px;height:20px"></div> |
| `NSColor.alternateSelectedControlTextColor` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.controlBackgroundColor` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.controlColor` | `#e7e7e7` | <div style="background-color:#e7e7e7;width:20px;height:20px"></div> |
| `NSColor.controlHighlightColor` | `#e2e2e2` | <div style="background-color:#e2e2e2;width:20px;height:20px"></div> |
| `NSColor.controlLightHighlightColor` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.controlShadowColor` | `#8d8d8d` | <div style="background-color:#8d8d8d;width:20px;height:20px"></div> |
| `NSColor.controlDarkShadowColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.controlTextColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.disabledControlTextColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.gridColor` | `#cccccc` | <div style="background-color:#cccccc;width:20px;height:20px"></div> |
| `NSColor.headerColor` | `#aaaaaa` | <div style="background-color:#aaaaaa;width:20px;height:20px"></div> |
| `NSColor.headerTextColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.highlightColor` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.keyboardFocusIndicatorColor` | `#3b99fc` | <div style="background-color:#aaaaaa;width:20px;height:20px"></div> |
| `NSColor.knobColor` | `#9999bb` | <div style="background-color:#9999bb;width:20px;height:20px"></div> |
| `NSColor.scrollBarColor` | `#aaaaaa` | <div style="background-color:#aaaaaa;width:20px;height:20px"></div> |
| `NSColor.scrubberTexturedBackground` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.secondarySelectedControlColor` | `#dcdcdc` | <div style="background-color:#dcdcdc;width:20px;height:20px"></div> |
| `NSColor.selectedControlColor` | `#b2d7ff` | <div style="background-color:#b2d7ff;width:20px;height:20px"></div> |
| `NSColor.selectedControlTextColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.selectedTextColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.selectedKnobColor` | `#666699` | <div style="background-color:#666699;width:20px;height:20px"></div> |
| `NSColor.shadowColor` | `#000000` | <div style="background-color:#b2d7ff;width:20px;height:20px"></div> |
| `NSColor.systemBlue` | `#1badf8` | <div style="background-color:#1badf8;width:20px;height:20px"></div> |
| `NSColor.systemBrown` | `#a2845e` | <div style="background-color:#a2845e;width:20px;height:20px"></div> |
| `NSColor.systemGray` | `#8e8e91` | <div style="background-color:#8e8e91;width:20px;height:20px"></div> |
| `NSColor.systemGreen` | `#63da38` | <div style="background-color:#63da38;width:20px;height:20px"></div> |
| `NSColor.systemOrange` | `#ff9500` | <div style="background-color:#ff9500;width:20px;height:20px"></div> |
| `NSColor.systemPink` | `#ff2968` | <div style="background-color:#ff2968;width:20px;height:20px"></div> |
| `NSColor.systemPurple` | `#cc73e1` | <div style="background-color:#cc73e1;width:20px;height:20px"></div> |
| `NSColor.systemRed` | `#ff3b30` | <div style="background-color:#ff3b30;width:20px;height:20px"></div> |
| `NSColor.systemYellow` | `#ffcc00` | <div style="background-color:#ffcc00;width:20px;height:20px"></div> |
| `NSColor.textBackgroundColor` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |
| `NSColor.textColor` | `#000000` | <div style="background-color:#000000;width:20px;height:20px"></div> |
| `NSColor.windowBackgroundColor` | `#ececec` | <div style="background-color:#ececec;width:20px;height:20px"></div> |
| `NSColor.windowFrameColor` | `#aaaaaa` | <div style="background-color:#aaaaaa;width:20px;height:20px"></div> |
| `NSColor.windowFrameTextColor` | `#000000` | <div style="background-color:#ececec;width:20px;height:20px"></div> |
| `NSColor.underPageBackgroundColor` | `#ffffff` | <div style="background-color:#ffffff;width:20px;height:20px"></div> |


