### What is View in Android?

* View is the basic building block of UI(User Interface) in android.
* A View is a superclass for all the UI components
* Every view occupies a rectangle shape on screen that shows some type of content. It can be an image, a piece of text, a button or anything that an android application can display. Example: TextView, EditText, Button etc.

### What is ViewGroup
* ViewGroup is like a container of Views. A group of view is known as ViewGroup. 
* A ViewGroup can have other views and view groups are its children. 
 * For example, under a LinearLayout, you can add two Buttons and one EditText. Here, LinearLayout is the parent view and the Buttons and EditTexts are the children.

### Difference between View.GONE and View.INVISIBLE?

View.INVISIBLE This view is invisible,but it is drawn and it still takes up space in layout.
View.GONE This view is invisible,and it doesn't take any space for layout purposes.

### Can you a create custom view? How?

1) Create a class and extend it with ```View``` or any subclass of ```View``` for which we need to override some feature. 

```
import android.view.View;

public class CustomView extends View {   
}
```
2) There are multiple constructors, matching super that needs to be created. This is because Views can be created programmatically and via xml. So different types of constuctors and theier implementation is necessary.

```
public class CustomView extends View {

    public CustomView(Context context) {
        super(context);
    }

    public CustomView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public CustomView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    public CustomView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }
}
```

3) Create a ```init()``` like method which initializes views:
4) Override the ```onDraw()``` method, this provides us a ```Canvas``` to draw views. 
```Canvas``` is the interface to the actual surface on which we draw views.
```java
 @Override
  protected void onDraw(Canvas canvas) {
      super.onDraw(canvas);
      Rect rect = new Rect(0,0, 100, 100);
      Paint paint = new Paint();
      paint.setColor(Color.BLACK);
      canvas.drawRect(rect, paint);
  }
```
* CAUTION: Do not do memory allocations on ```onDraw()```, because it is called multiple times during its lifecycle. For example when the activity is destroyed and created again, due to device rotation, the view in drawn again.
So a better way would be to declare the ```Rect``` and ```Paint``` objects in the ```init()``` function once.

```java
private Rect mRectSquare;

public void init(@Nullable AttributeSet attributeSet){
    mRect = new Rect();
    mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
}

@Override
protected void onDraw(Canvas canvas) {
    canvas.drawRect(mRect, mPaint);
}
```
* The ```ANTI_ALIAS_FLAG``` is used to make the edges smooth while drawing

5) For dynamic attributes, create a declare-style in attrs.xml, This has array of <attr> with name and format keys.
```
<declare-styleable name="CustomView2">
    <attr name="color" format="color"/>
    <attr name="size" format="dimension"/>
</declare-styleable>
```
6) To receive these values on the class, create a ```TypedArray``` in ```init()```, extract the values, and recycle ```TypedArray``` for garbage collection:

```
public void init(Context context, @Nullable AttributeSet attrs){
    if(attrs == null)
        return;
    TypedArray typedArray = context.obtainStyledAttributes(attrs,R.styleable.CustomView);
    mSize = typedArray.getDimensionPixelSize(R.styleable.CustomView_size, DEFAULT_SIZE);
    mColor = typedArray.getColor(R.styleable.CustomView_color, DEFAULT_COLOR);
    typedArray.recycle();
}
```
7) Suppose if we want to change the color of drawn rectangle, from the component that is inflating this view, then we can write a public method to do so:

```java
 public void swapColor(){
    mPaint.setColor(mPaint.getColor() == Color.RED ? Color.GREEN : Color.RED);
    postInvalidate();
}

```
* Note after setting color, we are calling ```postInvalidate()```, this will trigger the  ```onDraw()``` method, and the view can be correctly drawn. The other option is ```invalidate()``` which is a blocking call to main thread. We prefer asynchronous ```postInvalidate()``` which informs the view once the view is drawn.

8) Using ```onTouchEvent``` to interact with the components of canvas:

Suppose if we have drawn a circle with center coordinates ```mCircleX``` and ```mCircleY```, and defined radius as ```mCircleRadius```. Then the following method helps us to intercept a touch event, and if clicking circle and moving it, we can move the circle along with it.

```
@Override
public boolean onTouchEvent(MotionEvent event) {
    boolean value =  super.onTouchEvent(event);

    switch (event.getAction()){
        case MotionEvent.ACTION_DOWN:{
            float x = event.getX();
            float y = event.getY();

            double dx = Math.pow(x-mCircleX,2);
            double dy = Math.pow(y-mCircleY,2);

            if(dx+dy < Math.pow(mCircleRadius,2)){
                //touch event on circle
                mCircleX = x;
                mCircleY = y;
                postInvalidate();
                return true;
            }
        }
    }
    return value;
}
```

* The final implementation of Custom View with a square, circle and image above it:

```
package ai.guru.testapp.customview;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Matrix;
import android.graphics.Paint;
import android.graphics.Rect;
import android.graphics.RectF;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewTreeObserver;

import ai.guru.testapp.R;
import androidx.annotation.Nullable;

public class CustomView extends View {

    private static final int SQUARE_SIZE_DEFAULT = 200;
    private static final int CIRCLE_RADIUS_DEFAULT = 100;
    private Rect mRectSquare;
    private Paint mPaintSquare, mPaintCircle;
    private int mSquareColor, mCircleColor;
    private int mSquareSize;
    private float mCircleRadius = CIRCLE_RADIUS_DEFAULT, mCircleX, mCircleY;
    private Bitmap mImage;

    public CustomView(Context context) {
        super(context);
        init(null);
    }

    public CustomView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init(attrs);
    }

    public CustomView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(attrs);
    }

    public CustomView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(attrs);
    }

    public void init(@Nullable AttributeSet attributeSet){
        mRectSquare = new Rect();
        mPaintSquare = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaintCircle = new Paint(Paint.ANTI_ALIAS_FLAG);

        getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                getViewTreeObserver().removeOnGlobalLayoutListener(this);
                mImage = BitmapFactory.decodeResource(getResources(), R.drawable.image);
                mImage = getResizedBitmap(mImage, getWidth(), getHeight());
            }
        });

        if(attributeSet == null)
            return;
        TypedArray ta = getContext().obtainStyledAttributes(attributeSet, R.styleable.CustomView);
        mSquareColor = ta.getColor(R.styleable.CustomView_square_color,Color.GREEN);
        mSquareSize = ta.getDimensionPixelSize(R.styleable.CustomView_square_size, SQUARE_SIZE_DEFAULT);

        mCircleColor = ta.getColor(R.styleable.CustomView_circle_color, Color.RED);
        mCircleRadius = ta.getDimensionPixelSize(R.styleable.CustomView_circle_radius, CIRCLE_RADIUS_DEFAULT);

        mPaintSquare.setColor(mSquareColor);
        mPaintCircle.setColor(mCircleColor);
        ta.recycle();

        mCircleRadius = CIRCLE_RADIUS_DEFAULT;

    }


    @Override
    protected void onDraw(Canvas canvas) {
        //draw a shape,
        mRectSquare.left = 50;
        mRectSquare.top = 50;
        mRectSquare.right = mRectSquare.left + mSquareSize;
        mRectSquare.bottom = mRectSquare.top + mSquareSize ;

        if(mCircleX == 0f || mCircleY == 0f){
            mCircleX = getWidth()/2;
            mCircleY = getHeight()/2;
        }

        //paint object to draw on canvas
       canvas.drawRect(mRectSquare, mPaintSquare);
       canvas.drawCircle(mCircleX, mCircleY,mCircleRadius, mPaintCircle);
       //add image to canvas
       canvas.drawBitmap(mImage, 0,0, null);
    }



    public void swapColor(){
        mPaintSquare.setColor(mPaintSquare.getColor() == mSquareColor ? Color.GREEN : mSquareColor);
        postInvalidate();
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        boolean value =  super.onTouchEvent(event);

        switch (event.getAction()){
            case MotionEvent.ACTION_DOWN:{
                float x = event.getX();
                float y = event.getY();

                double dx = Math.pow(x-mCircleX,2);
                double dy = Math.pow(y-mCircleY,2);

                if(dx+dy < Math.pow(mCircleRadius,2)){
                    //touch event on circle
                    mCircleX = x;
                    mCircleY = y;
                    postInvalidate();
                    return true;
                }
            }

            case MotionEvent.ACTION_MOVE:{
                float x = event.getX();
                float y = event.getY();

                double dx = Math.pow(x-mCircleX,2);
                double dy = Math.pow(y-mCircleY,2);

                if(dx+dy < Math.pow(mCircleRadius,2)){
                    //touch event on circle
                    mCircleX = x;
                    mCircleY = y;
                    postInvalidate();
                    return true;
                }
            }

        }
        return value;
    }

    private Bitmap getResizedBitmap(Bitmap mImageBitmap, int width, int height) {
        Matrix matrix = new Matrix();
        RectF src = new RectF(0,0,mImageBitmap.getWidth(), mImageBitmap.getHeight());
        RectF dest = new RectF(0,0,width, height);
        matrix.setRectToRect(src,dest, Matrix.ScaleToFit.CENTER);
        return Bitmap.createBitmap(mImageBitmap, 0,0, mImageBitmap.getWidth(), mImageBitmap.getHeight(), matrix,true);
    }
}
```

### What is canvas?
* When we draw custom views, Canvas is the interface that allows us to draw custom designs like line, circle,rectangle ect.
* Drawing of canvas happens in Bitmap, (Canvas is a wrapper over Bitmap) In Canvas we draw the outline and then the Paint API helps to fill color. Canvas helps to create the skeleton and Paint helps in beautifying the design.
* Canvas follows the coordinate system to draw itself on the screen. It considers the phone screen as its own coordinate system. The top left corner represents the (0,0) point in the coordinate system and bottom right is then (x,y) point in the coordinate system.
* Canvas works on pixels and not dp.


### Custom View Lifecycle
![](https://github.com/shubhamgupta2901/repo_assets/blob/master/cheatsheets/android/lifecycle/android_view_lifecycle.png)

The views are drawn using following three methods:
1. At first onMeasure() gets called where Android measures the UI from top to bottom, First, it takes the parents container. their children and so on. In onMeasure() children get the constraints provided by their parents. 
2. Then, onLayout() is called to plot the positions of the Widgets.
3. and onDraw() is called where the UI gets rendered. The drawing happens on the main UI thread, so it blocks the main thread till the execution gets completed. For heavier and complicated definitions of onDraw(), you can also feel a little bit of lag.
Also, in a View's lifecycle, which is attached to a component, onDraw can get called multiple number of times, every time, invalidate() is called or the parent component is destroyed and created again, due to config changes. So object should not be created here.


### SurfaceView

* In Android, Views are drawn on the UI/main thread. This is also used  all user interaction. 
* As a result, if we need to update the UI rapdidly like changing the preview of a camera or animations, the renderting takes too much time, and due to the heavy execution of onDraw(), the user experience is also affected.
* For such cases, we can use SurfaceView. It provides a Surface, on which the views are drawn on a background thread, and once Surface is drawn or updated, through callbacks we can get the same information in the UI thread.
* Issues with SurfaceView:
    * surfaceView has dedicated Surface buffer while all the view shares one surface buffer that is allocated by ViewRoot. In another word, surfaceView cost more resources.
    * surfaceView cannot be hardware accelerated (as of JB4.2) while 95% operations on normal View are HW accelerated. [the Android 2D rendering pipeline supports hardware acceleration, meaning that all drawing operations that are performed on a View's canvas use the GPU. Because of the increased resources required to enable hardware acceleration, your app will consume more RAM]

Example:

```java
package com.javacodegeeks.androidsurfaceviewexample;
 
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
 
import android.app.Activity;
import android.hardware.Camera;
import android.hardware.Camera.PictureCallback;
import android.hardware.Camera.ShutterCallback;
import android.os.Bundle;
import android.util.Log;
import android.view.SurfaceHolder;
import android.view.SurfaceView;
import android.view.View;
import android.widget.TextView;
import android.widget.Toast;
 
public class AndroidSurfaceviewExample extends Activity implements SurfaceHolder.Callback {
    TextView testView;
 
    Camera camera;
    SurfaceView surfaceView;
    SurfaceHolder surfaceHolder;
 
    PictureCallback rawCallback;
    ShutterCallback shutterCallback;
    PictureCallback jpegCallback;
 
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
 
        setContentView(R.layout.activity_main);
 
        surfaceView = (SurfaceView) findViewById(R.id.surfaceView);
        surfaceHolder = surfaceView.getHolder();
 
        // Install a SurfaceHolder.Callback so we get notified when the
        // underlying surface is created and destroyed.
        surfaceHolder.addCallback(this);
    }
 
    public void captureImage(View v) throws IOException {
        //take the picture
        camera.takePicture(null, null, jpegCallback);
    }
 
    public void refreshCamera() {
        if (surfaceHolder.getSurface() == null) {
            // preview surface does not exist
            return;
        }
 
        // stop preview before making changes
        try {
            camera.stopPreview();
        } catch (Exception e) {
            // ignore: tried to stop a non-existent preview
        }
 
        // set preview size and make any resize, rotate or
        // reformatting changes here
        // start preview with new settings
        try {
            camera.setPreviewDisplay(surfaceHolder);
            camera.startPreview();
        } catch (Exception e) {
 
        }
    }
 
    public void surfaceChanged(SurfaceHolder holder, int format, int w, int h) {
        // Now that the size is known, set up the camera parameters and begin
        // the preview.
        refreshCamera();
    }
 
    public void surfaceCreated(SurfaceHolder holder) {
        try {
            // open the camera
            camera = Camera.open();
        } catch (RuntimeException e) {
            // check for exceptions
            System.err.println(e);
            return;
        }
        Camera.Parameters param;
        param = camera.getParameters();
 
        // modify parameter
        param.setPreviewSize(352, 288);
        camera.setParameters(param);
        try {
            // The Surface has been created, now tell the camera where to draw
            // the preview.
            camera.setPreviewDisplay(surfaceHolder);
            camera.startPreview();
        } catch (Exception e) {
            // check for exceptions
            System.err.println(e);
            return;
        }
    }
 
    public void surfaceDestroyed(SurfaceHolder holder) {
        // stop preview and release camera
        camera.stopPreview();
        camera.release();
        camera = null;
    }
 
}
```

### Difference between RelativeLayout and LinearLayout?

Linear Layout - Arranges elements either vertically or horizontally. i.e. in a row or column.
Relative Layout - Arranges elements relative to parent and other siblings.

### Constraint Layout
* It allows you to create large and complex layouts with a flat view hierarchy (no nested view groups). This has the benefit of avoiding many problems inherent in nesting layouts. Since we design flat or shallow layout hierarchies. This leads to less complex layouts and improved user interface rendering performance at runtime.
* It's similar to RelativeLayout in that all views are laid out according to relationships between sibling views and the parent layout, but it's more flexible than RelativeLayout and easier to use with Android Studio's Layout Editor.


