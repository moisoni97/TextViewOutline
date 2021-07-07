# Android TextViewOutline Demo
This is a demo project on how to create a `TextView` with an outer stroke.

![textview outline preview](https://user-images.githubusercontent.com/44326908/100282904-239b6e00-2f75-11eb-965a-2054d0f7ee8f.jpg)

* After creating the `TextViewOutline` class as shown in the project, simply call the `<TextViewOutline />` as a normal `<TextView />` inside your layout:
```xml
<games.moisoni.textviewoutline.TextViewOutline
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView Outline"
        android:textColor="@color/white"
        app:outlineColor="@color/black"
        app:outlineSize="3dp" />
```

* Optionally, you can cast shadow:
```xml
<games.moisoni.textviewoutline.TextViewOutline
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView Outline Shadow"
        android:textColor="@color/white"
        android:shadowRadius="1"
        android:shadowColor="#00BCD4"
        android:shadowDx="0"
        android:shadowDy="8"
        app:outlineColor="@color/black"
        app:outlineSize="3dp" />
```

# Steps:

* First create the `attrs.xml` inside `values` folder:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="TextViewOutline">
        <attr name="outlineSize" format="dimension" />
        <attr name="outlineColor" format="color|reference" />
        <attr name="android:shadowRadius" />
        <attr name="android:shadowDx" />
        <attr name="android:shadowDy" />
        <attr name="android:shadowColor" />
    </declare-styleable>
</resources>
```

* Then, create the `TextViewOutline` class:
```java
public class TextViewOutline extends AppCompatTextView {

    private static final int DEFAULT_OUTLINE_SIZE = 0;
    private static final int DEFAULT_OUTLINE_COLOR = Color.TRANSPARENT;

    private int mOutlineSize;
    private int mOutlineColor;
    private int mTextColor;

    private float mShadowRadius;
    private float mShadowDx;
    private float mShadowDy;
    private int mShadowColor;

    public TextViewOutline(@NonNull Context context) {
        super(context);
    }

    public TextViewOutline(@NonNull Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        setAttributes(attrs);
    }

    private void setAttributes(AttributeSet attr) {
        mOutlineSize = DEFAULT_OUTLINE_SIZE;
        mOutlineColor = DEFAULT_OUTLINE_COLOR;

        mTextColor = getCurrentTextColor();
        if (attr != null) {
            TypedArray a = getContext().obtainStyledAttributes(attr, R.styleable.TextViewOutline);
            if (a.hasValue(R.styleable.TextViewOutline_outlineSize)) {
                mOutlineSize = (int) a.getDimension(R.styleable.TextViewOutline_outlineSize, DEFAULT_OUTLINE_SIZE);
            }

            if (a.hasValue(R.styleable.TextViewOutline_outlineColor)) {
                mOutlineColor = a.getColor(R.styleable.TextViewOutline_outlineColor, DEFAULT_OUTLINE_COLOR);
            }

            if (a.hasValue(R.styleable.TextViewOutline_android_shadowRadius)
                    || a.hasValue(R.styleable.TextViewOutline_android_shadowDx)
                    || a.hasValue(R.styleable.TextViewOutline_android_shadowDy)
                    || a.hasValue(R.styleable.TextViewOutline_android_shadowColor)) {

                mShadowRadius = a.getFloat(R.styleable.TextViewOutline_android_shadowRadius, 0);
                mShadowDx = a.getFloat(R.styleable.TextViewOutline_android_shadowDx, 0);
                mShadowDy = a.getFloat(R.styleable.TextViewOutline_android_shadowDy, 0);
                mShadowColor = a.getColor(R.styleable.TextViewOutline_android_shadowColor, Color.TRANSPARENT);
            }

            a.recycle();
        }
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        setPaintToOutline();
    }

    private void setPaintToOutline() {
        Paint paint = getPaint();
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(mOutlineSize);
        super.setTextColor(mOutlineColor);
        super.setShadowLayer(0, 0, 0, Color.TRANSPARENT);
    }

    private void setPainToRegular() {
        Paint paint = getPaint();
        paint.setStyle(Paint.Style.FILL);
        paint.setStrokeWidth(0);
        super.setTextColor(mTextColor);
        super.setShadowLayer(mShadowRadius, mShadowDx, mShadowDy, mShadowColor);
    }

    @Override
    public void setTextColor(int color) {
        super.setTextColor(color);
        mTextColor = color;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        setPaintToOutline();
        super.onDraw(canvas);

        setPainToRegular();
        super.onDraw(canvas);
    }
}
```
