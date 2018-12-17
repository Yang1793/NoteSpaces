# 属性动画笔记

## 为什么写笔记
原因一：最近项目比较闲。
原因二：用了好久的属性动画了，一直瞎JB用，没有好好总结一波。

## 视图动画(View Animation)
视图动画分为补间动画和逐帧动画。补间动画举个例子。
```
<?xml version="1.0" encoding="utf-8"?>
<translate
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromXDelta="0"
    android:fromYDelta="0"
    android:toXDelta="50%p"
    android:toYDelta="50%p"
    android:fillAfter="true"
    android:duration="500">
</translate>
```

```
Animation animation = AnimationUtils.loadAnimation(this, R.anim.translate);
animation.start();
view.startAnimation(animation);
```

## 属性动画（Property Animation）

### 基础使用

通过直接改变View的属性，达到动画的效果。<br/>
真实改变了属性。

```
int position = 0;
public void startAnimation(View v){
		//属性动画
    position += 100;
    v.setTranslationX(position);
    v.setAlpha((float)Math.random());
}
```
### 使用ObjectAnimator类改变View属性
使用ObjectAnimator类改变View属性，使得动画效果实现。<br/>
ObjectAnimator.ofFloat(Object target, String propertyName, float... values)<br/>
target: 需要执行动画的View<br/>
propertyName: 这个属性动画的名字，可以查文档得知，也可以通过View.getXxx得知<br/>
values： 是指动画执行的值变化。如果填写多个，执行时间平分。<br/>
```
//	ObjectAnimator oa = ObjectAnimator.ofFloat(v, "translationX", 0f,300f);
//	oa.setDuration(500);
//	oa.start();
//	ObjectAnimator oa = ObjectAnimator.ofFloat(v, "translationY", 0f,300f);
//  oa.setDuration(500);
//  oa.start();
	ObjectAnimator oa = ObjectAnimator.ofFloat(v, "rotationX", 0f,360f);
	oa.setDuration(500);
	oa.start();
```

### 多个动画同时执行

#### 方法一：设置监听
监听其中一个动画，然后回调通过改变View属性，达到多个动画同时执行。<br/>
下面这段代码中的"heihei"是随便填写的，也可以填写真实属性。<br/>
随便填写也能成功执行的原因是ObjectAnimator继承与就是ValueAnimator，没有这个属性的时候，就只是执行一个Value的变化。
```
ObjectAnimator animator = ObjectAnimator.ofFloat(view, "heihei", 0f, 100f);
animator.setDuration(300);
//设置动画监听
animator.addUpdateListener(new AnimatorUpdateListener() {

  @Override
  public void onAnimationUpdate(ValueAnimator animation) {
    //动画在执行的过程当中，不断地调用此方法
    //得到duration时间内 values当中的某一个中间值。0f~100f
    float value = (float) animation.getAnimatedValue();//
    view.setScaleX(0.5f+value/200);//0.5~1
    view.setScaleY(0.5f+value/200);//0.5~1
  }
});
animator.start();
```

#### 方法二：使用PropertyValuesHolder类实现
ObjectAnimator.ofPropertyValuesHolder可以添加多个PropertyValuesHolder对象。<br/>
所以新建多个PropertyValuesHolder对象，每个对象对应一个单独的状态。<br/>
把这些对象通过ObjectAnimator.ofPropertyValuesHolder生成ObjectAnimator<br/>
执行动画，就行了。

```
PropertyValuesHolder holder1 = PropertyValuesHolder.ofFloat("alpha", 1f,0.7f,1f);
PropertyValuesHolder holder2 = PropertyValuesHolder.ofFloat("scaleX", 1f,0.7f,1f);
PropertyValuesHolder holder3 = PropertyValuesHolder.ofFloat("scaleY", 1f,0.7f,1f);

ObjectAnimator animator = ObjectAnimator.ofPropertyValuesHolder(view, holder1,holder2,holder3);
animator.setDuration(1000);
animator.start();        
```
#### 方法三：使用集合
使用 AnimatorSet将多个ObjectAnimator整合在一起，再通过playTogether方法同时执行。<br/>
当然它还有一些其他的方法，比如playSequentially执行动画方法。
```
ObjectAnimator animator1 = ObjectAnimator.ofFloat(iv,"alpha", 1f,0.7f,1f);
ObjectAnimator animator2 = ObjectAnimator.ofFloat(iv,"scaleX", 1f,0.7f,1f);
ObjectAnimator animator3 = ObjectAnimator.ofFloat(iv,"scaleY", 1f,0.7f,1f);

AnimatorSet animatorSet = new AnimatorSet();
animatorSet.setDuration(500);
//animatorSet.play(anim);//执行当个动画
animatorSet.playTogether(animator1,animator2,animator3);//同时执行
//animatorSet.playSequentially(animator1,animator2,animator3);//依次执行动画
animatorSet.start();
```
