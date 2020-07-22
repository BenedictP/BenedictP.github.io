---
layout: post
title: "Use the coordinatorLayout to scroll of the ActionBar"
date: 2015-06-09
comments: true
---

I'm going to show you how to use the new design library 22.2.0 to create an AppBar which is going to scroll of the screen while scrolling up.

With the new design library 22.2 you can create an [Toolbar](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html?utm_campaign=io15&utm_source=dac&utm_medium=blog) which is going to scroll of the screen. In this little tutorial I will show you how to achieve this with the [AppBarLayout](http://developer.android.com/reference/android/support/design/widget/AppBarLayout.html?utm_campaign=io15&utm_source=dac&utm_medium=blog).

<iframe width="420" height="315" src="https://www.youtube.com/embed/D0T7NMwatPI" frameborder="0" allowfullscreen></iframe>

* First we create a new projekt with the minimum SDK *API 14: Android 4.0 (IceCreamSandwich)*
* We have a *MainActivity* extending from *AppCombatActivity*
* After that we add this line to the *build.gradle (Module: app)* under *dependencies*:
```
compile 'com.android.support:design:22.2.0'
```
* Our *main_activity.xml* should look like this:
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>

<android.support.design.widget.CoordinatorLayout 
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
android:layout_width="match_parent"
android:layout_height="match_parent">
		
	<!-- Your Scrollable View -->
	<android.support.v4.widget.NestedScrollView
		android:id="@+id/scroll_view"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		app:layout_behavior="@string/appbar_scrolling_view_behavior">
			
		<include
			layout="@layout/a_very_high_layout"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"/>
	
	</android.support.v4.widget.NestedScrollView>
		
	<android.support.design.widget.AppBarLayout
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:fitsSystemWindows="true">
	
		<android.support.v7.widget.Toolbar
			android:id="@+id/toolbar"
			android:layout_width="match_parent"
			android:layout_height="?attr/actionBarSize"
			app:layout_scrollFlags="scroll|enterAlways"
			app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>
	</android.support.design.widget.AppBarLayout>

</android.support.design.widget.CoordinatorLayout>
{% endhighlight %}
* Your scrollable view could also be a RecyclerView. Otherwise it will not recognize the scrolling.
* You can also use following scrollFlags:
  * *scroll*: this flag should be set for all views that want to scroll off the screen - for views that do not use this flag, they’ll remain pinned to the top of the screen
  * *enterAlways*: this flag ensures that any downward scroll will cause this view to become visible, enabling the ‘quick return’ pattern
  * *enterAlwaysCollapsed*: When your view has declared a minHeight and you use this flag, your View will only enter at its minimum height (i.e., ‘collapsed’), only re-expanding to its full height when the scrolling view has reached it’s top.
  * *exitUntilCollapsed*: this flag causes the view to scroll off until it is ‘collapsed’ (its minHeight) before exiting
* Because we are using an AppBarOverlay we must use a style that hasn't an ActionBar included. The *styles.xml* should look like this:

{% highlight xml %}
<resources>

	<!-- Base application theme. -->
	<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
		<!-- Customize your theme here. -->
	</style>

</resources>
{% endhighlight %}
* In our *MainActivity.java*  the *onCreate* method will set the toolbar as *supportActionBar*:
{% highlight java %}
public class MainActivity extends AppCompatActivity {

	private Toolbar toolbar;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.main_activity);
	toolbar = (Toolbar) findViewById(R.id.toolbar);
	setSupportActionBar(toolbar);
	}

}
{% endhighlight %}
* With the support library you also can set a primary color, primary color dark and an acent color to define your unique app style. This happens in the styles.xml file:
{% highlight xml %}
<resources>

	<!-- Base application theme. -->
	<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
		<!-- Customize your theme here. -->
		<item name="colorPrimary">@color/color_primary</item>
		<item name="colorPrimaryDark">@color/color_primary_dark</item>
	</style>

</resources>
{% endhighlight %}

You can find the full soure code on Github: [https://github.com/BenedictP/AppBarScrollOffScreen](https://github.com/BenedictP/AppBarScrollOffScreen)