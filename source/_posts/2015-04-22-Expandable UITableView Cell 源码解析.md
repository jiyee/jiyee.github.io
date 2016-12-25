---
title: 'Expandable UITableView Cell 源码解析'
date: 2015-04-22
---

Code4App 上 [开展TableView的Cell](http://code4app.com/ios/%E5%B1%95%E5%BC%80tableView%E7%9A%84cell/5151337b6803fa5f46000000 "展开TableView的Cell")

实现思路：利用 Header 替代 Cell 展现列表，通过 section 替代 row。

在 `tableview:viewForHeaderInSection:` 方法增加列表内容。

在 Header 里增加了一个 UIButton，实现单击展开，在 `tableview:numberOfRowsInSection:` 方法里判断 section 等于 didSection，不展开返回 0，因此不会调用 `tableview:cellForRowAtIndexPath:` 方法。

保存变量 didSection, endSection
endSection 保存及时单击所在的 section
didSection 保存过往展开的 section
ifOpen 保存展开状态，只保留一个展开态

用户操作存在三种状态：
1. 之前未展开，单击一个展开
2. 之前已展开，单击展开列，收起当前列
3. 之前已展开，单击其他列，收起展开列，展开当前列

`didSelectCellRowFirstDo:(BOOL)firstDoInsert nextDo:(BOOL)nextDoInsert`

巧妙的是这个方法里如果 nextDoInsert 等于Y ES，会再调用一次 `[self didSelectCellRowFirstDo:YES nextDo:NO]`

改变 UITableView 时，调用 beginUpdates 和 endUpdates，而 insertRowsAtIndexPaths:withRowAnimation: 指定插入 rows、indexPath 和 animation。

最后，调用了 scrollToNearestSelectedRowAtScrollPosition 方法，无效果，改成 scrollToRowAtIndexPath 可行。

---- 
PS: 看了另外一个简单的项目，[ExtendTableView](http://code4app.com/ios/ExtendTableView/4f6818536803fa791c000004 "ExtendTableView")，它的思路就是插入 row 方式实现。