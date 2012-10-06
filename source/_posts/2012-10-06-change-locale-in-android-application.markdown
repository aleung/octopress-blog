---
layout: post
title: "Android: 在應用中設定locale"
date: 2012-10-06 00:25
comments: true
tags:
- Android
- SoftwareDev 
---
近幾天把以前做的跑步步频训练软件 [RunningCadence](https://play.google.com/store/apps/details?id=leoliang.runningcadence) 加上了多語言支持，可以在應用中設定界面以及語音播報所用的語言。

對多語言的支持，Android已經做得很完善，開發者只需要在應用中加入各種locale的資源文件。但是，應用只會使用系統全局設置的locale，而要在應用自己的設定中選擇locale，以及要讓設定立即生效，就需要開發者自己去實現了。

本來，一般應用並沒必要自己提供語言設定，用系統全局設定就好了。但這個 RunningCadence 使用了語音合成(TTS)功能，而一般手機上安裝的TTS引擎支持的語言都有限，如果TTS不支持系統locale的語言，那就聽不到聲音了，所以需要花力氣去搞這個應用內的語言選擇。

## 讓應用啓動時使用自己設定的locale，而非系統locale

開發應用時要爲不同locale準備不同的資源，在應用中通過 [Resources](http://developer.android.com/reference/android/content/res/Resources.html) 類來加載資源，各個界面組件的構建都需要用到資源。而具體資源如何選擇是受 [Configuration](http://developer.android.com/reference/android/content/res/Configuration.html) 影響的，Configuration帶有設備的硬件相關配置信息（如屏幕分辨率，屏幕方向）和系統全局配置信息（如locale），由系統底層框架提供。

應用啓動時，Configuration中的locale會被設置爲系統locale。應用若要使用自己的locale，就必須在創建界面之前，將Resources裏的Configuration更改。

這個更改在application的 onCreate() 裏面做最合適，對應用全局生效，因爲它在任何activity創建之前就執行了，不再需要在各個activity裏做任何事情。

``` java
	@Override
	public void onCreate() {
		setLocale();
	}

	public void setLocale() {
		Locale locale = getLocaleFromPref();
		Locale.setDefault(locale);
		Configuration config = getBaseContext().getResources().getConfiguration();
		overwriteConfigurationLocale(config, locale);
	}

	private void overwriteConfigurationLocale(Configuration config, Locale locale) {
		config.locale = locale;
		getBaseContext().getResources()
				.updateConfiguration(config, getBaseContext().getResources().getDisplayMetrics());
	}
```

加了這段代碼後，應用啓動時就會根據 getLocaleFromPref() 返回的語言來顯示了，但是你會發現如果將手機屏幕轉一下，例如豎屏變爲橫屏，界面又會變回系統缺省語言，爲什麼呢？

系統底層框架會在configuration發生了變化時通知應用[^1]。對application是調用 Application.onConfigurationChanged() 方法。對activity的處理採用那種方式，就與manifest文件中<Activity&gt;的 android:configChanges 屬性配置相關：

- 如果發生的是configChanges中指定的事件，調用 Activity.onConfigurationChanged()，不重啓activity；
- 否則重啓activity。

[^1]: 詳細機制見 [Handling Runtime Changes](http://developer.android.com/guide/topics/resources/runtime-changes.html)

屏幕的旋轉就是一種 runtime change，缺省情況下會觸發activity的重啓，也就是銷毀並重新創建activity[^2]，重新創建時使用的是新的Configuration，裏面帶的又是系統locale，因此就造成了界面變回系統缺省語言。

[^2]: 在 [Android Activity Lifecycle in UML](/2010/12/16/Android-activity-lifecycle-in-UML-state-machine-diagram) 文中的狀態圖可以見到configChanged引發的狀態遷移。

爲了避免這種情況，需要在Application的 onConfigurationChanged() 裏面也對Configuration做修改。

``` java
	@Override
	public void onConfigurationChanged(Configuration newConfig) {
		Locale locale = getLocaleFromPref();
		Locale.setDefault(locale);
		overwriteConfigurationLocale(newConfig, locale);
		super.onConfigurationChanged(newConfig);
	}
```

做了這些後，應用就能使用自己的locale設定，而不是系統locale了。完整代碼例子可以參考RunningCadence源碼 [Application.java](https://github.com/aleung/RunningCadence/blob/c658e00bd24a23bd95369bf6e3d87254776ae2cb/RunningCadence/src/leoliang/runningcadence/Application.java)。

## 讓應用preference中的locale設定修改立即生效

應用通常會使用 Preference API 來構造用戶設定界面，在上一步完成後，用戶可以在應用preference裏設置locale，在應用重新啓動時會使用選定的locale。但是用戶在preference裏修改locale後是不會立即生效的，因爲修改沒有反映到 configuration 中去，而且對於已經存在的activity，界面組件都已經創建好了，界面上的文字不可能自動改變。

一種方案是讓整個應用重新啓動，所有資源都重新加載，所有界面都重新創建。我留意了一下，大部分提供應用內語言設定的應用都是這樣做的——在彈出對話框裏選擇語言並確認後，不會返回到設定頁面，而是顯示應用的入口界面——應用已經重啓了。在 RunningCadence 中，我不想用這種方法，因爲用戶體驗會不好——用戶在修改語言後，通常還要選擇另一個選項進行語音合成測試，看看TTS對新選擇的語言能否正常工作——如果應用重啓返回主界面，用戶就得再進入設定界面才能進行測試，一方面是操作麻煩了，另一方面界面無緣無故跳轉也會帶來困惑。

### 在用戶修改設定後，更新Configuration

更新Configuration的方法跟應用啓動時的做法一樣，可以重用 setLocale() 方法，問題是要能在合適的時機去調用。使用SharedPreference的修改通知機制可以做到這點。

``` java
    public class PreferenceActivity extends android.preference.PreferenceActivity implements
    		OnSharedPreferenceChangeListener {

		// ...
    
    	@Override
    	public void onSharedPreferenceChanged(SharedPreferences sharedPreferences, String key) {
    		if (key.equals("pref_language")) {
    			((Application) getApplication()).setLocale();
    			restartActivity();
    		}
    	}
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		addPreferencesFromResource(R.xml.preferences);
    		getPreferenceScreen().getSharedPreferences().registerOnSharedPreferenceChangeListener(this);
    	}
    
    	@Override
    	protected void onStop() {
    		super.onStop();
    		getPreferenceScreen().getSharedPreferences().unregisterOnSharedPreferenceChangeListener(this);
    	}
	}
```

要注意這個listener的寫法，如果按照通常Android程序風格，使用匿名內部類來實現，就會發生詭異的問題，總是不會被回調。這個問題花了我好長時間，誤打誤撞解決了也沒明白什麼回事，寫這篇文章時才看見StackOverflow上有這個問題的[根源解答](http://stackoverflow.com/a/3104265/94148)。

### 將已經存在的界面按照新locale重新顯示

先分析哪些activity是需要重新顯示的，這需要對應用的 [task stack](http://developer.android.com/guide/components/tasks-and-back-stack.html) 結構有一個審視。要知道在locale更改的時刻，哪些activity還在生存着，會在後續操作中重新變爲可見狀態，這些activity的界面需要重建。在RunningCadence裏比較簡單，就是PreferenceActivity本身和調用它的主activity。

要讓activity的界面按新locale重新顯示，最簡單的方法應該就是讓它重啓，這比起對每個界面元件都用重新加載資源去重設要簡單得多。

```java
	private void restartActivity() {
		Intent intent = getIntent();
		finish();
		startActivity(intent);
	}
```

PreferenceActivity的重啓是在OnSharedPreferenceChangeListener得知設定發生了改變的時候進行，在上面的代碼例子裏已經顯示出來了。而主activity的重啓是在當用戶從PreferenceActivity中返回到主activity時，在onActivityResult() 中觸發。

完整代碼例子可以參考RunningCadence源碼 [PreferenceActivity.java](https://github.com/aleung/RunningCadence/blob/f0cdb98b42a94caa5c7e2cec1a8aa6abf91e73b9/RunningCadence/src/leoliang/runningcadence/PreferenceActivity.java)。

## 總結

現在將思路理清了寫下來，感覺不算複雜，但是在做的過程中費了好多腦筋繞了不少彎路，邊上網查資料邊嘗試。Android的API Guides在ICS發佈後改進了好多，很多內容重寫過更清晰容易理解了，另外一個非常有價值的資源是[StackOverflow.com](StackOverflow.com)。

我感覺，對於一般應用沒有太大必要去實現應用內的語言選擇。系統裏所有應用都使用統一的locale本來就挺好的。

如果應用的task stack結構複雜，需要重新顯示的activity很多，可能用重啓整個應用的方法更簡單一些。我不知道重啓應用是怎麼做到的，Android API裏面沒有現成的方法。不過若實現了應用重啓，應用內部各個Activity都不需要做任何額外處理了。

----

