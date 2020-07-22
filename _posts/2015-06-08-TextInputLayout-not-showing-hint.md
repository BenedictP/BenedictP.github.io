---
layout: post
title: "TextInputLayout not showing hint"
date: 2015-06-08
comments: true
---

There is another bug in a support library. It's in the new android support design library 22.2.0 (*com.android.support:design:22.2.0*) and is causing that the new [TextInputLayout](http://developer.android.com/reference/android/support/design/widget/TextInputLayout.html?utm_campaign=io15&utm_source=dac&utm_medium=blog) is not showing the hint as usual.

Some user reported that the hints for EditTextFields wrapped with the new TextInputLayout are not working properly. The hint will only appear if the user is focusing the EditTextField. Apparently it's working for versions below 5.0 but not on 5.0 and above.

This bug is already reported to Google: [https://code.google.com/p/android/issues/detail?id=175228](https://code.google.com/p/android/issues/detail?id=175228)

You can find a workaround here: [https://gist.github.com/ljubisa987/e33cd5597da07172c55d](https://gist.github.com/ljubisa987/e33cd5597da07172c55d)
But it seems it's not working on Android M.