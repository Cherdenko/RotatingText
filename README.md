<div align="center"><img src="/screens/gif_cover.gif"/></div>

# RotatingText
[![platform](https://img.shields.io/badge/Platform-Android-yellow.svg?style=flat-square)](https://www.android.com)
[![API](https://img.shields.io/badge/API-16%2B-brightgreen.svg?style=flat-square)](https://android-arsenal.com/api?level=16s)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![By-MDG](https://img.shields.io/badge/By-MDG-orange.svg?style=flat-square)](http://mdg.iitr.ac.in)

Rotating text is an Android library that can be used to make text switching painless and beautiful, with the use of interpolators, typefaces and more customisations.

## ✨ New Features

- **Stop at Last Entry** - Rotation stops at the last word instead of cycling infinitely
- **XML Attributes** - Configure text size, stop behavior, and more directly in XML
- **Layout Preview** - See preview text in Android Studio's design view
- **Last Interpolator** - Use a different animation for the final word transition
- **Improved Text Sizing** - Text size now works correctly at any value

# Usage
Just add the following dependency in your app's `build.gradle`
```
dependencies {
      compile 'com.sdsmdg.harjot:rotatingtext:1.0.2'
}
```

## Example Usage 1 (Simple with XML Attributes)
#### XML

```xml
<com.sdsmdg.harjot.rotatingtext.RotatingTextWrapper
        android:id="@+id/custom_switcher"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:textSize="35sp"
        app:stopAtLast="false"
        app:previewText="This is Word" />
```

#### Java

```java
RotatingTextWrapper rotatingTextWrapper = findViewById(R.id.custom_switcher);

Rotatable rotatable = new Rotatable(Color.parseColor("#FFA036"), 1000, "Word", "Word01", "Word02");
rotatable.setSize(35);
rotatable.setAnimationDuration(500);

rotatingTextWrapper.setContent("This is ?", rotatable);
```

#### Result
<img src="/screens/gif_example_1.gif"/>

## Example Usage 2 (Stop at Last with Custom Interpolator)
#### XML

```xml
<com.sdsmdg.harjot.rotatingtext.RotatingTextWrapper
        android:id="@+id/custom_switcher"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:textSize="35sp"
        app:stopAtLast="true" />
```

#### Java

```java
Typeface typeface = ResourcesCompat.getFont(this, R.font.your_font);

RotatingTextWrapper rotatingTextWrapper = findViewById(R.id.custom_switcher);
rotatingTextWrapper.setTypeface(typeface);

Rotatable rotatable = new Rotatable(Color.parseColor("#FFA036"), 1000, "Loading", "Processing", "Done!");
rotatable.setSize(35);
rotatable.setAnimationDuration(500);
rotatable.setTypeface(typeface);
rotatable.setInterpolator(new BounceInterpolator());
// Different animation for last word!
rotatable.setLastInterpolator(new OvershootInterpolator(2.0f));

rotatingTextWrapper.setContent("Status: ?", rotatable);
```

#### Result
<img src="/screens/gif_example_2.gif"/>

The text will rotate through "Loading" → "Processing" → "Done!" and stop. The last word "Done!" will use a dramatic overshoot animation!

## Example Usage 3 (Multiple Rotatables)
#### XML

```xml
<com.sdsmdg.harjot.rotatingtext.RotatingTextWrapper
        android:id="@+id/custom_switcher"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:textSize="35sp" />
```

#### Java

```java
Typeface typeface = ResourcesCompat.getFont(this, R.font.your_font);

RotatingTextWrapper rotatingTextWrapper = findViewById(R.id.custom_switcher);
rotatingTextWrapper.setTypeface(typeface);

Rotatable rotatable = new Rotatable(Color.parseColor("#FFA036"), 1000, "Word", "Word01", "Word02");
rotatable.setSize(35);
rotatable.setTypeface(typeface);
rotatable.setInterpolator(new AccelerateInterpolator());
rotatable.setAnimationDuration(500);

Rotatable rotatable2 = new Rotatable(Color.parseColor("#123456"), 1000, "Word03", "Word04", "Word05");
rotatable2.setSize(25);
rotatable2.setTypeface(typeface);
rotatable2.setInterpolator(new DecelerateInterpolator());
rotatable2.setAnimationDuration(500);

rotatingTextWrapper.setContent("This is ? and ?", rotatable, rotatable2);
```

#### Result
<img src="/screens/gif_example_3.gif"/>

## Example Usage 4 (Modern Countdown)
#### XML

```xml
<com.sdsmdg.harjot.rotatingtext.RotatingTextWrapper
        android:id="@+id/countdown_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:textSize="38sp"
        app:stopAtLast="true"
        app:previewText="3 2 1 GO!" />
```

#### Java

```java
Typeface typeface = ResourcesCompat.getFont(this, R.font.barlow_condensed_semi_bold);

RotatingTextWrapper wrapper = findViewById(R.id.countdown_text);
wrapper.setTypeface(typeface);

// Create countdown: 10, 9, 8, ... 2, 1, 0
String[] countdown = createCountdownArray(10);

Rotatable rotatable = new Rotatable(
    ContextCompat.getColor(this, R.color.white), 
    200,  // Fast transitions
    countdown
);
rotatable.setSize(38);
rotatable.setStopAtLast(true);  // Stop at 0
rotatable.setAnimationDuration(200);
rotatable.setCenter(true);
rotatable.setTypeface(typeface);
rotatable.setInterpolator(new LinearInterpolator());  // Smooth countdown
rotatable.setLastInterpolator(new BounceInterpolator());  // Bounce on final number!

wrapper.setContent("?", rotatable);
```

# Documentation

Rotating text is made of two parts : `RotatingTextWrapper` and `Rotatable`. <br>
Each rotatable encapsulates the collection of words that are two be periodically switched and also defines various properties related to these words, like, size, color, animation interpolator etc.<br>
Each Rotatable must be a part of a `RotatingTextWrapper`. This defines the actual layout of the text and the positions of the rotating text.

For eg : `rotatingTextWrapper.setContent("This is ?", rotatable);`. Here the `?` denotes the position of the `rotatable`.

## XML Attributes

Configure RotatingTextWrapper directly in XML:

|Attribute         |Type           |Description                             |
|------------------|---------------|----------------------------------------|
|`app:textSize`    | dimension     | Text size (e.g., "24sp", "16dp")       |
|`app:stopAtLast`  | boolean       | Stop at last word instead of cycling   |
|`app:previewText` | string        | Preview text in layout editor          |
|`app:adaptable`   | boolean       | Adapt to parent width                  |

**Example:**
```xml
<com.sdsmdg.harjot.rotatingtext.RotatingTextWrapper
    app:textSize="32sp"
    app:stopAtLast="true"
    app:previewText="Sample Preview" />
```

## RotatingTextWrapper
|Property         |Function                |Description                             |
|-----------------|------------------------|----------------------------------------|
|Content               | setContent(...)                    | Set the actual content. Composed of a String and array of Rotatables. |
|Typeface              | setTypeface(...)                   | Set the typeface of the non-rotating text                     |
|Size                  | setSize(...)                       | Set the size of the non-rotating text                         |
|Stop At Last          | setStopAtLast(...)                 | Stop rotation at last word instead of cycling                 |
|Preview Text          | setPreviewText(...)                | Set preview text for layout editor                            |
|Last Interpolator     | setLastInterpolator(...)           | Set special interpolator for last word animation              |
|Pause                 | pause(x)                           | Method to pause the 'x'th rotatable                           |
|Resume                | resume(x)                          | Method to resume the 'x'th rotatable                          |

## Rotatable
|Property         |Function                |Description                             |
|-----------------|------------------------|----------------------------------------|
|Color                 | setColor(...)                      | Set the color of the rotating text associated with this rotatable     |
|Size                  | setSize(...)                       | Set the size of the rotating text associated with this rotatable      |
|Typeface              | setTypeface(...)                   | Set the typeface of the rotating text associated with this rotatable  |
|Interpolator          | setInterpolator(...)               | Set the animation interpolator used while switching text              |
|Last Interpolator     | setLastInterpolator(...)           | Set different interpolator for animating to last word                 |
|Update Duration       | setUpdateDuration(...)             | Set the interval between switching the words                          |
|Animation Duration    | setAnimationDuration(...)          | Set the duration of the switching animation                           |
|Center Align          | setCenter(...)                     | Align the rotating text to center of the textview if set to **true** |
|Stop At Last          | setStopAtLast(...)                 | Stop at last word in the array instead of cycling                     |

## New Features Guide

### Stop at Last Entry
Stop rotation at the last word instead of cycling back:

```java
rotatable.setStopAtLast(true);
// or
wrapper.setStopAtLast(true);  // Applies to all rotatables
```

### Last Interpolator
Use a different animation for the final word:

```java
rotatable.setInterpolator(new BounceInterpolator());  // For all words
rotatable.setLastInterpolator(new OvershootInterpolator(2.0f));  // Special finish!
```

**Perfect for:**
- Loading sequences with satisfying completions
- Countdowns with dramatic endings
- Status updates with emphasis on final state

### Preview in Layout Editor
See your text in Android Studio's design view:

```xml
<com.sdsmdg.harjot.rotatingtext.RotatingTextWrapper
    app:previewText="This is how it looks" />
```

📚 **For more examples and detailed documentation, see:**
- [NEW_FEATURES.md](NEW_FEATURES.md) - Quick start guide
- [FEATURE_DOCUMENTATION.md](FEATURE_DOCUMENTATION.md) - Complete usage guide
- [LAST_INTERPOLATOR_FEATURE.md](LAST_INTERPOLATOR_FEATURE.md) - Last interpolator deep dive


# License
RotatingText is licensed under `MIT license`. View [license](LICENSE.md).
