# Android 基本知识

 github地址: https://github.com/xiaolongzou/Learn
 
## Android 基本布局

###  LinearLayout

   常见属性：
  
    orientation: 排列方式
    
    gravity：控制组件所包含的子元素的对齐方式。可多个组合比如center|bottom
    
    layout_gravity: 控制改组件在父容器的对齐方式
    
    background：背景图片/颜色
    
    divider：分割线，showDividers才显示
    
    showDividers：设置分割线所在的位置，none，beginning，middle，end
    
    dividerPadding：设置分割线的padding
    
    layout_weight: 权重，用来等比划划分区域的 本质上是将剩余空间进行分配
    
    layout_weight 栗子：
    
        3个子元素
        
        layout_height为match_parent
        
        layout_weight为2 1 1
        
        剩余空间 1 - 3 = -2
        
        1 - 2 * 2/4 = 0
        
        1 - 2 * 1/4 = 1/2
        
        1 - 2 * 1/4 = 1/2
        
        实际使用中通过layout_weight去等比划分 一般将layout_height设置为0（orientation：vertical）

###  RelativeLayout

   所有的子元素位置初始都是相对于原点

   常见属性：

     根据父容器定位
     
     1.layout_alignParentLeft 左对齐
     
     2.layout_alignParentRight 右对齐
     
     3.layout_alignParentTop 顶部对齐
     
     4.layout_alignParentBottom 底部对齐
     
     5.layout_centerHorizontal 水平居中
     
     6.layout_centerVertical 垂直居中
     
     7.layout_centerInParent 中间位置

     根据兄弟组件定位
     
     1.layout_toLeftOf 放置于参考组件左边
     
     2.layout_toRightOf 放置于参考组件右边
     
     3.layout_above 放置于参考组件上边
     
     4.layout_below 放置于参考组件下边
     
     5.layout_alignTop 对齐参考组件的上边界
     
     6.layout_aligntBottom 对齐参考组件的下边界
     
     7.layout_alignLeft 对齐参考组件的左边界
     
     8.layout_alignRight 对齐参考组件的右边界

     margin：设置组件与父容器的间距

     padding：设置组件内元素与父容器的间距
     
###  FrameLayout
   
   FrameLayout是从原点开始绘制，后面的覆盖前面的子View
   
       <FrameLayout
           xmlns:android="http://schemas.android.com/apk/res/android"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
           <FrameLayout
               android:background="#ffff00"
               android:layout_width="500dp"
               android:layout_height="500dp"/>
           <FrameLayout
               android:foregroundGravity="right|bottom"
               android:foreground="@mipmap/ic_launcher"
               android:background="#ff0000"
               android:layout_width="300dp"
               android:layout_height="300dp"/>
       </FrameLayout>

### TableLayout

   可以理解为常用的表格

   常见属性：
        
        android:collapseColumns="0" //隐藏
        android:stretchColumns="1"  //拉伸
        android:shrinkColumns="4 //收缩
        
   子控件设置属性：
   
      android:layout_column="2" //显示在第几列
      android:layout_span="2"  //横向占几列
  
  xml:
  
    <TableLayout
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:shrinkColumns="4"
       xmlns:android="http://schemas.android.com/apk/res/android">

      <TableRow>
          <Button
              android:text="第一个"
              android:layout_height="wrap_content"
              android:layout_width="wrap_content"/>
          <Button
              android:text="第二个"
              android:layout_height="wrap_content"
              android:layout_width="wrap_content"/>
          <Button
              android:text="第三个"
              android:layout_height="wrap_content"
              android:layout_width="wrap_content"/>
          <Button
              android:text="第四个"
              android:layout_height="wrap_content"
              android:layout_width="wrap_content"/>
          <Button
              android:text="第五个"
              android:layout_height="wrap_content"
              android:layout_width="wrap_content"/>
      </TableRow>

      <Button
          android:text="@string/app_name"
          android:layout_height="wrap_content"
          android:layout_width="wrap_content"/>

    </TableLayout>

### GridLayout
   
   和TableLayout类似但是不和TableRow一起使用
   
   属性
   
      android:orientation="vertical" //垂直方向
      android:rowCount="3" //垂直方向一行几个
      android:orientation=""horizontal" //水平方向
      android:columnCount="3" //水平方向时一列几个
   
   子控件属性
   
        android:layout_column="0" //第几列
        android:layout_row="1" //第几行
        android:layout_columnSpan="3" // 横向占几列，配合layout_gravity看效果
        android:layout_rowSpan="3" // 横向占几列，配合layout_gravity看效果
        android:layout_gravity="fill" 
        android:layout_columnWeight="1"  //横向权重
        android:layout_rowWeight="1 " //纵向权重


###  ConstraintLayout

    对于ContraintLayout主要是对AS的Desgin工具的使用
    通过拖拽来实现想要的效果
 
##  listView
   
   新建类Bean，myAdapter
   
     Bean：
     
     package com.example.mylistview;
      public class Bean {
          String name;

          public String getName() {
              return name;
          }

          public void setName(String name) {
              this.name = name;
          }
      }
      
      myAdapter：
      
      1.继承BaseAdapter；
      2.定义私有变量data和context；
      3.实现构造方法，用来获取data和context
      4.重写getCount 一个都是data.size();
      5.getItemId 修改为return position；
      6.重写getView方法 定义一个ViewHolder类去存储textView，使用viewHolder能够避免每次渲染都是去调用findViewById，以便减少性能消耗
      
      
      package com.example.mylistview;
      import android.content.Context;
      import android.view.LayoutInflater;
      import android.view.View;
      import android.view.ViewGroup;
      import android.widget.BaseAdapter;
      import android.widget.TextView;

      import java.util.List;

      public class MyAdapter extends BaseAdapter {


          private List<Bean> data;
          private Context context;

          public MyAdapter(List<Bean> data, Context context) {
              this.data = data;
              this.context = context;
          }

          @Override
          public int getCount() {
              return data.size();
          }

          @Override
          public Object getItem(int position) {
              return null;
          }

          @Override
          public long getItemId(int position) {
              return position;
          }

          @Override
          public View getView(int position, View convertView, ViewGroup parent) {
              ViewHolder viewHolder;
              if (convertView == null) {
                  viewHolder = new ViewHolder();
                  convertView = LayoutInflater.from(context).inflate(R.layout.list_item, parent, false);
                  viewHolder.textView = convertView.findViewById(R.id.tv);
                  convertView.setTag(viewHolder);
              } else {
                  viewHolder = (ViewHolder) convertView.getTag();
              }
              viewHolder.textView.setText(data.get(position).getName());
              return  convertView;
          }

          private  final class ViewHolder {
              TextView textView;
          }
      }
      
      MainActivity里面的调用
      
      1.定义一个data，通过Bean方法为data加入数据；
      
      2.调用findViewById去获取到ListView
      
      3.new一个MyAdapter去为listView设置适配器（setAdapter）；
      
      4.为listView添加点击事件setOnItemClickListener
      
      
      
      public class MainActivity extends AppCompatActivity {

      private List<Bean> data = new ArrayList<>();

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);

           for (int i= 0; i < 100; i++) {
               Bean bean = new Bean();
               bean.setName("legendary" + i);
               data.add(bean);
           }
           ListView listView = findViewById(R.id.lv);
           listView.setAdapter(new MyAdapter(data, this));

           listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
               @Override
               public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                   Log.d("OnItemClick", "position" + position);
               }
           });
       }
      }
      

##  recyclerView

   相比于ListView，recyclerView更加的灵活，实现的效果更多
   
   recyclerView的使用，需要先在build.gradle加入
   
     implementation 'androidx.recyclerview:recyclerview:1.1.0'
     
  类Bean同ListView
  
  类MyAdapter
  
  1.和ListView不同，recyclerView继承RecyclerView.Adapter<MyAdapter.MyViewHodler>, 让我们在
  继承Adapter的时候去实现MyAdapter的时候去创建一个MyViewHolder，来进行性能优化
  
  2.MyViewHolder需要继承RecyclerView.ViewHolder，创建一个TextView textview以及实现构造方法MyViewHolder，
  在MyViewHolder里面通过findViewById去拿到textview，
  
  3.创建构造方法同ListView的Adapter
  
  4.onCreateViewHolder方法： 创建ViewHolder 
  
  5.onBindViewHolder: 绑定数据
  
  6.因为RecyclerView没有设置监听的方法，需要我们在MyAdapter去自己实现，首先创建一个接口及接口对象
  
  7.实现onRecyclerItemClick的回调，通过为itemView设置setOnClickListener实现；
  
  
    public class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyViewHolder> {

    private List<Bean> data;
    private Context context;

    public MyAdapter(List<Bean> data, Context context) {
        this.data = data;
        this.context = context;

    }

    @NonNull
    @Override
    public MyAdapter.MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = View.inflate(context, R.layout.item, null);
        return new MyViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull MyAdapter.MyViewHolder holder, int position) {
        holder.textView.setText(data.get(position).getName());
    }

    @Override
    public int getItemCount() {
        return data == null ? 0 : data.size();
    }

    public class MyViewHolder extends RecyclerView.ViewHolder {

        private TextView textView;

        public MyViewHolder(@NonNull View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.tv);

            itemView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if (onRecyclerItemClickListener != null) {
                        onRecyclerItemClickListener.onRecyclerItemClick(getAdapterPosition());
                    }
                }
            });
        }
    }

    private  OnRecyclerItemClickListener onRecyclerItemClickListener;

    public void setOnRecyclerItemClickListener(OnRecyclerItemClickListener listener) { //定义一个设置监听的方法
        onRecyclerItemClickListener = listener;
    }

    public  interface OnRecyclerItemClickListener { //创建一个接口
        void onRecyclerItemClick(int position);
    }
    }

MainActivity：
  
         public class MainActivity extends AppCompatActivity {

           private List<Bean> data = new ArrayList<>();

           @Override
           protected void onCreate(Bundle savedInstanceState) {
               super.onCreate(savedInstanceState);
               setContentView(R.layout.activity_main);
               for (int i = 9000; i < 20000; i ++) {
                   if (i % 4 != 0) {
                       continue;
                   }
                   Bean bean = new Bean();
                   bean.setName("可惜"+ i);
                   data.add(bean);
               }

               RecyclerView recyclerView = findViewById(R.id.lv);

               //线性布局
       //        LinearLayoutManager linearLayoutManager = new LinearLayoutManager(this);
       //        recyclerView.setLayoutManager(linearLayoutManager);

               //网格布局
       //        GridLayoutManager gridLayoutManager = new GridLayoutManager(this, 3);
       //        recyclerView.setLayoutManager(gridLayoutManager);

               //瀑布流布局
               StaggeredGridLayoutManager staggeredGridLayoutManager = new StaggeredGridLayoutManager(3, LinearLayout.VERTICAL);
               recyclerView.setLayoutManager(staggeredGridLayoutManager);

               MyAdapter myAdapter = new MyAdapter(data, this);
               recyclerView.setAdapter(myAdapter);

               myAdapter.setOnRecyclerItemClickListener(new MyAdapter.OnRecyclerItemClickListener() {
                   @Override
                   public void onRecyclerItemClick(int position) {
                       Log.d("itemClick", "position"+ position);
                   }
               });
           }
       }

##   单位与尺寸

###  px和pt的区别
  
   px（像素）：不同设备显示效果相同
   
   pt： point，是一个标准的长度单位 1pt = 1/72英寸，用于印刷业，非常简单易用
   
### dp和sp的作用
  
  dp：即dip，device independent pixels（设备独立像素），不同设备显示不同的效果，这个和设备硬件有关，一般我们为了支持WVGA HVGA和QVGA推荐使用这个，不依赖像素
  
  sp：scaled pixels（放大像素），主要用于字体best for textSize
  
### LayoutParams是什么

   LayoutParams相当于一个layout的信息包，它封装了layout的高，宽，位置等信息。
   
   栗子：
   
      
        LinearLayout linearLayout = new LinearLayout(this);
        LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
        linearLayout.setLayoutParams(layoutParams);

        TextView textView = new TextView(this);
        textView.setText("123123");
        LinearLayout.LayoutParams textLayoutParams = new LinearLayout.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT);
    //  textView.setLayoutParams(textLayoutParams);
    //  linearLayout.addView(textView);
        linearLayout.addView(textView, textLayoutParams);
        
        setContentView(linearLayout);






  
  
  
  
  
  
  
  
  
  
  
