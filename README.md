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
a.新建search.xml，添加SearchView用来实现搜索
b.在NotesList.java中显示并获取SearchView控件，初始化控件
c.设置搜索触发方法
d.重新设置setListAdapter,实现实时搜索并显示搜索结果
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
```
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
