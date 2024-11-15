# MyNotePad
### 一. `扩展功能如下：`
1. NoteList界面中笔记条目增加时间戳显示；
2. 添加笔记查询功能（根据标题或内容查询）；
3. UI美化；
4. 支持多类型笔记；
### 二. `笔记条目增加时间戳:`
![Alt Text](./001.png)
1. NotesListAdapter.java：负责显示每个笔记的创建时间，并通过 DateUtil.StringToDate() 转换时间戳为易读格式。

```properties
// 创建 NotesListAdapter 并设置给 ListView
NotesListAdapter adapter = new NotesListAdapter(this, cursor, Notes.CONTENT_URI, "com.example.android.notepad.action.EDIT");
listView.setAdapter(adapter);
```

2. DateUtil.java：工具类，提供了时间戳转换为日期的功能。

```properties
public class DateUtil {

    /**
     * 将时间戳字符串转换为日期格式
     * @param timestamp 时间戳（字符串）
     * @return 格式化后的日期字符串
     */
    public static String StringToDate(String timestamp) {
        long time = Long.parseLong(timestamp); // 将字符串时间戳转为 long 类型
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"); // 设置日期格式
        Date date = new Date(time); // 使用时间戳创建日期对象
        return sdf.format(date); // 将日期格式化为字符串
    }
}
```

### 三. `添加笔记查询功能：`
1. NotesListAdapter.java 通过 Cursor 查询从数据库获取笔记数据，并在 readDate() 和 readDate(Cursor cursor) 方法中处理。

```properties
public void readDate() {
    mDate.clear();
    while(cursor.moveToNext()) {
        mDate.add(new NoteBean(cursor.getString(COLUMN_INDEX_TITLE), cursor.getString(COLUMN_INDEX_MODIFICATION_DATE), cursor.getString(COLUMN_INDEX_ID)));
        Log.d("hhh", cursor.getString(COLUMN_INDEX_TITLE) + "," + cursor.getString(COLUMN_INDEX_MODIFICATION_DATE) + "," + cursor.getString(COLUMN_INDEX_ID));
    }
}
```

```properties
public void readDate(Cursor cursor) {
    mDate.clear();
    while(cursor.moveToNext()) {
        mDate.add(new NoteBean(cursor.getString(COLUMN_INDEX_TITLE), cursor.getString(COLUMN_INDEX_MODIFICATION_DATE), cursor.getString(COLUMN_INDEX_ID)));
        Log.d("hhh", cursor.getString(COLUMN_INDEX_TITLE) + "," + cursor.getString(COLUMN_INDEX_MODIFICATION_DATE) + "," + cursor.getString(COLUMN_INDEX_ID));
    }
}
```

2. TitleEditor.java 使用 managedQuery 来查询笔记标题。

```properties
mCursor = managedQuery(
    mUri,        // The URI for the note that is to be retrieved.
    PROJECTION,  // The columns to retrieve
    null,        // No selection criteria are used, so no where columns are needed.
    null,        // No where columns are used, so no where values are needed.
    null         // No sort order is needed.
);

```

```properties
@Override
protected void onResume() {
    super.onResume();

    // Verifies that the query made in onCreate() actually worked.
    if (mCursor != null) {
        mCursor.moveToFirst();
        mText.setText(mCursor.getString(COLUMN_INDEX_TITLE));
    }
}

```
   
3. NotesLiveFolder.java 通过设置 URI 创建 Live Folder，间接依赖笔记数据。

```properties
liveFolderIntent.setData(NotePad.Notes.LIVE_FOLDER_URI);
```

4. NotesListAdapter.java 中的 Search 方法用于在已经查询的数据中进行搜索和过滤。

```properties
public void Search(String searchTitle) {
    searchData = new ArrayList<NoteBean>();
    for (NoteBean noteBean : mDate) {
        if (noteBean.getTitle().indexOf(searchTitle) != -1) {
            searchData.add(noteBean);
        }
    }
    mDate.clear();
    mDate.addAll(searchData);
    notifyDataSetChanged();
}
```

### 四. `UI美化:`
1. 更改背景颜色

```properties
/*背景颜色选择框*/
private  void showpopSelectBgWindows(){
    LayoutInflater inflater = LayoutInflater.from(this);
    View view = inflater.inflate(R.layout.dialog_bg_select_layout, null);
    AlertDialog.Builder builder = new AlertDialog.Builder(this);
    builder.setTitle("背景");//设置标题
    builder.setView(view);
    AlertDialog dialog = builder.create();//获取dialog
    dialog.show();//显示对话框
}
```

```properties
/*
    背景改变的监听
     */
    public void ColorSelect(View view) {
        String color;
        if (view.getId() == R.id.pink) {
            Drawable btnDrawable1 = getResources().getDrawable(R.drawable.pink);
            ll_noteList.setBackgroundDrawable(btnDrawable1);
            lv_notesList.setBackgroundDrawable(btnDrawable1);
        } else if (view.getId() == R.id.Yello) {
            Drawable btnDrawable2 = getResources().getDrawable(R.drawable.yellow);
            ll_noteList.setBackgroundDrawable(btnDrawable2);
            lv_notesList.setBackgroundDrawable(btnDrawable2);
        } else if (view.getId() == R.id.PaleVioletRed) {
            Drawable btnDrawable3 = getResources().getDrawable(R.drawable.palevioletred);
            ll_noteList.setBackgroundDrawable(btnDrawable3);
            lv_notesList.setBackgroundDrawable(btnDrawable3);
        } else if (view.getId() == R.id.LightGrey) {
            Drawable btnDrawable4 = getResources().getDrawable(R.drawable.lightgrey);
            ll_noteList.setBackgroundDrawable(btnDrawable4);
            lv_notesList.setBackgroundDrawable(btnDrawable4);
        } else if (view.getId() == R.id.MediumPurple) {
            Drawable btnDrawable5 = getResources().getDrawable(R.drawable.mediumpurple);
            ll_noteList.setBackgroundDrawable(btnDrawable5);
            lv_notesList.setBackgroundDrawable(btnDrawable5);
        } else if (view.getId() == R.id.DarkGray) {
            Drawable btnDrawable6 = getResources().getDrawable(R.drawable.darkgray);
            ll_noteList.setBackgroundDrawable(btnDrawable6);
            lv_notesList.setBackgroundDrawable(btnDrawable6);
        } else if (view.getId() == R.id.Snow) {
            Drawable btnDrawable7 = getResources().getDrawable(R.drawable.snow);
            ll_noteList.setBackgroundDrawable(btnDrawable7);
            lv_notesList.setBackgroundDrawable(btnDrawable7);
        }
    }
```

```properties
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:weightSum="0.7">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:id="@+id/pink"
        android:background="@drawable/pink"
        android:layout_weight="0.1"
        android:onClick="ColorSelect"
        />
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_weight="0.1"
        android:id="@+id/Yello"
        android:background="@drawable/yellow"
        android:onClick="ColorSelect"
        />
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:id="@+id/PaleVioletRed"
        android:layout_weight="0.1"
        android:background="@drawable/palevioletred"
        android:onClick="ColorSelect"
        />
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_weight="0.1"
        android:id="@+id/LightGrey"
        android:background="@drawable/lightgrey"
        android:onClick="ColorSelect"
        />
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_weight="0.1"
        android:id="@+id/MediumPurple"
        android:background="@drawable/mediumpurple"
        android:onClick="ColorSelect"
        />
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_weight="0.1"
        android:id="@+id/DarkGray"
        android:background="@drawable/darkgray"
        android:onClick="ColorSelect"
        />
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="60dp"
        android:layout_weight="0.1"
        android:id="@+id/Snow"
        android:background="@drawable/snow"
        android:onClick="ColorSelect"
        />

</LinearLayout>
```

2. 
