# FuckCalendar
==========================
> 简单使用的日历

![Screenshot](timesSquareScreenshot.png)


Usage
-----

Include `CalendarPickerView` in your layout XML.

```xml
 <com.ldf.calendar.MonthPager
        android:id="@+id/calendar_view"
        android:layout_width="match_parent"
        android:layout_height="270dp"
        android:background="#fff">
    </com.ldf.calendar.MonthPager>
```

目前来看 相比于Dialog选择日历 我的控件更适合于Activity/Fragment

在Activity的`onCreate`   或者Fragment的`onCreateView`  你需要实现这两个方法来启动日历并装填进数据
```java
  @Override
   protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_syllabus);
        initCurrentDate();
        initCalendarView();

    }
    
    private void initCurrentDate() {
        currentDate = new CalendarDate();
    }
 ```
使用此方法回调日历点击事件
 ```
private void initCalendarView() {
        onCellClickListener = new Calendar.OnCellClickListener() {
            @Override
            public void onClickDateCell(CalendarDate date) {
                refreshClickDate(date);
                calendarAdapter.updateAllClickState();// 此代码一定要有
            }

            @Override
            public void refreshDate(CalendarDate date) {
                refreshClickDate(date);
            }

            @Override
            public void onClickOtherMonth(int offset) {
                monthPager.setCurrentItem(mCurrentPage + offset);
            }
        };
        initMonthPage();
        calendarAdapter = new CalendarViewAdapter<>(showCalendars, CURRENT_OFFSET);
        initMonthPager();
    } 
 ```
 
 使用此方法初始化日历数据
 
 ```
 private void initCalendarData() {
        showCalendars = new Calendar[3];
        for (int i = 0; i < 3; i++) {
            showCalendars[i] = new Calendar(context, onCellClickListener);
        }
    }
 ```
 使用此方法给MonthPager添加上相关监听
  ```
  private void  initMonthPager() {
        monthPager.setAdapter(calendarAdapter);
        monthPager.setCurrentItem(MonthPager.CURRENT_DAY_INDEX);
        monthPager.setPageTransformer(false, new ViewPager.PageTransformer() {
            @Override
            public void transformPage(View page, float position) {
                position = (float) Math.sqrt(1 - Math.abs(position));
                page.setAlpha(position);
            }
        });
        monthPager.addOnPageChangeListener(new MonthPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
            }

            @Override
            public void onPageSelected(int position) {
            /// 示例代码
            }

            @Override
            public void onPageScrollStateChanged(int state) {
            }
        });
    }
  ```
 
Download
--------
Gradle:
```groovy
Step 1. Add it in your root build.gradle at the end of repositories:

allprojects {
		repositories {
			...
			maven { url 'https://www.jitpack.io' }
		}
	}
	
Step 2. Add the dependency

	dependencies {
	        compile 'com.github.MagicMashRoom:FuckCalendar:-SNAPSHOT'
	}

```

[![](https://www.jitpack.io/v/MagicMashRoom/FuckCalendar.svg)](https://www.jitpack.io/#MagicMashRoom/FuckCalendar)