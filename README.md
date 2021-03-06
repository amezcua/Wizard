#Android Wizard 
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Android%20Wizard-green.svg?style=flat)](https://android-arsenal.com/details/1/1469) ![Maven Central](https://img.shields.io/maven-central/v/me.panavtec/wizard.svg)

![Logo](art/logo.png)

##Sample app
![Logo](art/wizard.gif)


##Importing to your project
Add this dependency to your build.gradle file:

```java
dependencies {
    compile 'me.panavtec:wizard:{Lib version, see the mvn central badge}'
}
```
##Basic usage

Create a WizardPage for each fragment you need to present, the only mandatory method to override is "createFragment()":

```java
public class WizardPage1 extends WizardPage<Fragment1> {
    @Override public Fragment1 createFragment() {
        return new Fragment1();
    }
}
```

Create a wizard in your activity in this way:

```java
@Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
        wizard = new Wizard.Builder(this, new WizardPage1())
        .build();
    ...
...
```

And call init:

```java
wizard.init();
```

Now the wizard is initalized, to use navigation just call, "**navigatePrevious**", "**navigateNext**" or "**returnToFirst**". 
You can get the current fragment by calling "getCurrentFragment"

##Advanced usage

###Animation transitions
You can declare animations for entering/exiting fragments when creating Wizard in this way:

```java
new Wizard.Builder(this, new WizardPage1(), new WizardPage2(), new WizardPage3())
        .containerId(android.R.id.content)
        .enterAnimation(R.anim.card_slide_right_in)
        .exitAnimation(R.anim.card_slide_left_out)
        .popEnterAnimation(R.anim.card_slide_left_in)
        .popExitAnimation(R.anim.card_slide_right_out)
        .build();
```

This xml animations are uploaded to the sample project.


###Provide a custom container for fragment navigation
if you don't set the containerId attribute, Wizard uses android.R.id.content by default but you can personalize a custom container by calling:

```java
new Wizard.Builder(this, new WizardPage1())
        .containerId(R.id.my_container)
```

###Customize ActionBar in navigation
In the wizard page you can override method "setupActionBar" to customize your actionbar on each navigated fragment, ex:

```java
@Override public void setupActionBar(ActionBar supportActionBar) {
    super.setupActionBar(supportActionBar);
    supportActionBar.hide();
}
```

In this example when the fragment is navigated, the actionbar will be hidden. 

###Option menu in Actionbar

If you have a Fragment that uses actionbar option menus, you can override "hasOptionMenu" and return true to invalidate the option menus when navigating.

###Blocking Fragment

Do you need to block the back navigation of your wizard in determinate steps? It's so easy, just override "allowsBackNavigation" in your WizardPage and add this in your activity:

```java
@Override public void onBackPressed() {
    if (wizard.onBackPressed()) {
        super.onBackPressed();
    }
}
```
###Listeners
You can declare listeners for page changed and for wizard finished events:

```java
WizardPage[] wizardPages = { new WizardPage1(), new WizardPage2(), new WizardPage3() };
new Wizard.Builder(this, wizardPages)
        .containerId(android.R.id.content)
        .pageListener(new WizardPageListener() {
            @Override
            public void onPageChanged(int currentPageIndex, WizardPage page) {
                //Your code for page changed
            }
        })
        .wizardListener(new WizardListener() {
            @Override
            public void onWizardFinished() {
                //Your code for wizard finished
            }
        })
        .build();
```

###Dagger Tips
If you are using Dagger in your project I suggest to use Wizard in this way.
**ActivityModule**:

```java
@Provides @Singleton Wizard provideWizard(ActionBarActivity activity,
                                              SampleWizardPage page) {
    return new Wizard.Builder(activity, page)
            .containerId(android.R.id.content)
            .build();
}
    
@Provides @Singleton SampleWizardPage provideSampleWizardPage(ActionBarActivity activity) {
    return new SampleWizardPage(activity);
}
```

**Activity**:

```java
@Inject Wizard wizard;

@Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    wizard.init();
}
```

**Fragment**:

```java
@Inject Wizard wizard;

@Override protected void someOnClickAction() {
    wizard.navigateNext();
}
```

Developed by
============
Christian Panadero Martinez - <a href="http://panavtec.me">http://panavtec.me</a> - <a href="https://twitter.com/panavtec">@PaNaVTEC</a>

License
=======

    Copyright 2015 Christian Panadero Martinez

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
