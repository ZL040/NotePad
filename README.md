# NotePad功能阐述和演示
1--NoteList中显示条目增加时间戳显示：

 通过从数据库多读取一列时间COLUMN_NAME_MODIFICATION_DATE，显示文件处理时间。
 
 String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE} ;
 
 int[] viewIDs = { android.R.id.text1 ,android.R.id.text2};
 
 添加布局文件notelist-item.xml，代码如下：
 
 <LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:orientation="vertical"
    >

    <TextView
        android:id="@android:id/text1"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="2"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:gravity="center_vertical"
        android:paddingLeft="5dip"
        android:singleLine="true"
        />

    <TextView
        android:id="@android:id/text2"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="center_vertical"
        android:paddingLeft="5dip"
        android:singleLine="true"
        android:text="12sp"
        />

</LinearLayout>

![img_url](https://github.com/ZL040/NotePad/blob/master/demo/1.png)

2--添加笔记查询功能（根据标题查询）

通过添加where语句对数据库进行查询操作，使用LIKE进行模糊匹配。

String where = "title LIKE '%"+keyWord.getText().toString()+"%'";

![image](https://github.com/ZL040/NotePad/blob/master/demo/2.png)

3--可以更换背景颜色
添加如下代码来更换背景颜色

 case R.id.menu_changebg:
            switch ((int)(Math.random()*5)){
                case 0 :
                    mainBg.setBackgroundColor(Color.parseColor("#4169E1"));
                    break;
                case 1:
                    mainBg.setBackgroundColor(Color.parseColor("#98FB98"));
                    break;
                case 2:
                    mainBg.setBackgroundColor(Color.parseColor("#F0E68C"));
                    break;
                case 3:
                    mainBg.setBackgroundColor(Color.parseColor("#D3D3D3"));
                    break;
                case 4:
                    mainBg.setBackgroundColor(Color.parseColor("#000"));
                    break;
            }
            
![image](https://github.com/ZL040/NotePad/blob/master/demo/3.png)
3--可以导出笔记
添加ToFile类生成文件夹再生成文件，不然故意出错。

/**
 * Created by 54476 on 2017/5/20.
 */

public class ToFile {
    public static void writeTxtToFile(String strcontent, String filePath,String fileName) {
        // 生成文件夹之后，再生成文件，不然会出错
        makeFilePath(filePath, fileName);
        String strFilePath = filePath + fileName;
        // 每次写入时，都换行写
        String strContent = strcontent + "\n";
        try {
            File file = new File(strFilePath);
            if (!file.exists()) {
                file.getParentFile().mkdirs();//创建目录
                file.createNewFile();//创建文件
            }else {
                file.delete();//删除重新创建
                file.getParentFile().mkdirs();//创建目录
                file.createNewFile();//创建文件
            }
            RandomAccessFile randomAccessFile = new RandomAccessFile(file, "rwd");
            randomAccessFile.write(strContent.getBytes());
            randomAccessFile.close();
        } catch (Exception e) {
            Log.e("TestFile", "Error on write File:" + e);
        }
    }

    /**
     * 生成文件
     */
    public static File makeFilePath(String filePath, String fileName) {
        File file = null;
        makeRootDirectory(filePath);
        try {
            file = new File(filePath + fileName);
            if (!file.exists()) {
                file.createNewFile();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return file;
    }

    /**
     * 生成文件夹
     */
    public static void makeRootDirectory(String filePath) {
        File file = null;
        try {
            file = new File(filePath);
            if (!file.exists()) {
                file.mkdir();
            }
        } catch (Exception e) {
            Log.i("error:", e + "");
        }
    }

}

导出.txt文件，
![image](https://github.com/ZL040/NotePad/blob/master/demo/4.png)
导出后的效果图：

![image](https://github.com/ZL040/NotePad/blob/master/demo/5.png)
