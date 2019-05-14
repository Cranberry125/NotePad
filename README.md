# NotePad
# 期中作业</br>
## 基本功能：</br>
1.添加时间戳</br>
2.搜索功能</br>
## 附加功能：</br>
1.UI美化：设置背景颜色、设置字体颜色及大小</br>
2.导出笔记</br>
3.设置闹钟提醒</br>
4.分享笔记</br>
以下是实现功能的核心代码及解释：</br>
## 基本功能
### 一、添加时间戳</br>
a.对notelist_item.xml做修改，增加一个TextView来显示时间</br>
b.增加一个时间处理工具类-DateUtil,进行时间格式转换</br>
c.在notelist.java 查询列增加修改时间一栏</br>
代码如下：
```
notelist_item.xml
<!--显示时间-->
 <TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/time_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="left"
        android:paddingRight="5dip"
        android:textColor="@color/colorGreen"
        android:textAppearance="?android:attr/textAppearanceSmall"
        />
        
  notelist.java
  private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE,// 1
            //加上时间
            NotePad.Notes.COLUMN_NAME_CREATE_DATE //2

    };
      
```
### 二、搜索功能</br>
a.新建search.xml，添加SearchView用来实现搜索</br>
b.在NotesList.java中显示并获取SearchView控件，初始化控件</br>
c.设置搜索触发方法</br>
d.重新设置setListAdapter,实现实时搜索并显示搜索结果</br>
```
search.xml
 <SearchView
            android:id="@+id/searchView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:iconifiedByDefault="false"
            android:queryHint="请输入搜索内容"
            />

NotesList.java
 private void addSearchView() {
        //给listview添加头部(search)
        View v=View.inflate(this, R.layout.notelistheader,null);

        getListView().addHeaderView(v);
        //给搜索框添加搜索功能
        final EditText et_Search=(EditText)v.findViewById(R.id.et_search);
        et_Search.addTextChangedListener(new TextWatcherForSearch(){
            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
                super.onTextChanged(charSequence, i, i1, i2);
                if (charSequence.length()!=0 && et_Search.getText().toString().length()!=0){
                    String str_Search = et_Search.getText().toString();
                    Cursor search_cursor = managedQuery(
                            getIntent().getData(),            // Use the default content URI for the provider.
                            PROJECTION,                       // Return the note ID and title for each note.
                            NotePad.Notes.COLUMN_NAME_TITLE+" like ?",                             // No where clause, return all records.
                            new String[]{"%"+str_Search+"%"},                    // No where clause, therefore no where column values.
                            NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
                    );
                    adapter.swapCursor(search_cursor);/

                }else {
                    if (cursor!=null)/
                    adapter.swapCursor(cursor);
                }
            }
        });
    }

```
基本功能截图：</br>
时间戳截图：</br>
![时间.jpg](https://i.loli.net/2019/05/14/5cda56974f7a247424.jpg)</br>
搜索截图：</br>
![搜索.jpg](https://i.loli.net/2019/05/14/5cda5697adc2755767.jpg)
![搜索2.jpg](https://i.loli.net/2019/05/14/5cda5698053a480134.jpg)
##  附加功能
### 一、UI美化</br>
a.使用SharedPreferences来保存用户最后一次选择的背景颜色</br>
b.添加设置背景图标按钮，触发一个AlertDialog进行选择背景颜色</br>
c.设置AlertDialog并绑定触发事件</br>
```
back_color.xml
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="40dp"
        android:orientation="horizontal"
        android:gravity="center"
        android:background="@color/colorY">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="更 改 背 景"
            android:textSize="20dp"
            android:textColor="@color/colorwit"/>
    </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:orientation="horizontal"
        android:clickable="true">
        <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#F0E68C"
            android:id="@+id/colorYellow"
            android:clickable="true"
            android:layout_weight="1"/>

    </LinearLayout>

    
NoteList.java
<!--在onCreate()中获取控件同时设置点击事件-->
btn_color=(Button)findViewById(R.id.background);
    btn_color.setOnClickListener(new ClickEvent());
 <!--点击设置背景图标绑定事件-->
  class ClickEvent implements View.OnClickListener {
        @Override
        public void onClick (View v)  {
        }
    }
   <!--为每个TextView颜色设置监听事件-->
 class ClickEvent implements View.OnClickListener {
        @Override
        public void onClick (View v)  {
            final AlertDialog alertDialog = new AlertDialog.Builder(NotesList.this).create();
            alertDialog.show();
            Window window = alertDialog.getWindow();
            window.setContentView(R.layout.back_color);
            TextView color_colorYellow = (TextView)alertDialog.getWindow().findViewById(R.id.colorYellow);
            color_colorYellow.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    preferencescolor="#CDC673";//黄
                    getListView().setBackgroundColor(Color.parseColor(preferencescolor));
                    putColor(preferencescolor);//
                    alertDialog.dismiss();
                }
            }); 
        }
    }
 <!--放入sharepreferences-->
  private void putColor(String color){
        SharedPreferences preferences = getSharedPreferences("preferences", Context.MODE_PRIVATE);
        SharedPreferences.Editor editor = preferences.edit();
        editor.putString("color", color);
        editor.commit();
    }
    
    <!--设置字体-->
    在editor_option_menu中增加字体大小颜色的属性
    <item
        android:id="@+id/font_size"
        android:title="字体大小">
        <!--子菜单-->
        <menu>
            <!--定义一组单选菜单项-->
            <group>
                <!--定义多个菜单项-->
                <item
                    android:id="@+id/font_10"
                    android:title="10"
                    />

                <item
                    android:id="@+id/font_16"
                    android:title="16" />
                <item
                    android:id="@+id/font_20"
                    android:title="20" />
            </group>
        </menu>
    </item>

    <item
        android:title="字体颜色"
        android:id="@+id/font_color"
        >
        <menu>
            <!--定义一组普通菜单项-->
            <group>
                <!--定义两个菜单项-->
                <item
                    android:id="@+id/red_font"
                    android:title="红色" />
                <item
                    android:title="黑色"
                    android:id="@+id/black_font"/>
            </group>
        </menu>
    
```
设置背景：</br>
![设置背景1.jpg](https://i.loli.net/2019/05/14/5cda569834a8565117.jpg)
![设置背景二.jpg](https://i.loli.net/2019/05/14/5cda5aabc9f9878649.jpg)
![设置背景3.jpg](https://i.loli.net/2019/05/14/5cda5698f27a593399.jpg)</br>
改变字体颜色大小：</br>
![字体大小.jpg](https://i.loli.net/2019/05/14/5cda5699752d542946.jpg)</br>
### 二、导出笔记</br>
```
```
导出笔记截图：</br>
![导出笔记1.jpg](https://i.loli.net/2019/05/14/5cda58fd89d3538546.jpg)
![导出笔记3.jpg](https://i.loli.net/2019/05/14/5cda58fdb07e346931.jpg)
![导出笔记2.jpg](https://i.loli.net/2019/05/14/5cda58fdae31361976.jpg)</br>

### 三、设置闹钟提醒</br>
```
```
设置闹钟截图：</br>
![闹钟1.jpg](https://i.loli.net/2019/05/14/5cda596c5ca2f27821.jpg)
![闹钟2.jpg](https://i.loli.net/2019/05/14/5cda596c87fdb46948.jpg)
![闹钟3.jpg](https://i.loli.net/2019/05/14/5cda596c8b78567419.jpg)</br>
四、分享笔记</br>
```
```
### 四、分享笔记截图：</br>
![分享笔记1.jpg](https://i.loli.net/2019/05/14/5cda599c83c5387057.jpg)
![分享笔记2.jpg](https://i.loli.net/2019/05/14/5cda599ca1b7844131.jpg)
![分享笔记3.jpg](https://i.loli.net/2019/05/14/5cda599cac73b83426.jpg)
